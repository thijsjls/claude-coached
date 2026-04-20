# Setup Guide

This guide walks you through setting up Claude Coached as your personal AI cycling coach.

## Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed and working
- [uv](https://docs.astral.sh/uv/) installed (Python package manager — used by Strava and Intervals.icu MCP servers)
- A [Strava](https://www.strava.com/) account with activities (ideally with a power meter)
- A [Google Calendar](https://calendar.google.com/) account
- An [Intervals.icu](https://intervals.icu/) account (free — for analytics and device sync)
- Basic comfort with the command line

## Step 1: Clone the Repository

```bash
git clone https://github.com/thijsjls/claude-coached.git
cd claude-coached
```

## Tip: Let Claude Help You Set Up

You don't have to follow this guide alone. Once you've cloned the repo, run `claude` in the project directory and ask it to help you set up the MCP servers. Claude Code is great at walking you through OAuth flows, finding the right config values, and debugging connection issues. This is especially useful for the Strava and Google Calendar MCP setup — it can guide you step by step.

## Step 2: Set Up the Strava MCP Server

The coach reads your workout data from Strava via an MCP server.

### 2a. Create a Strava API Application

1. Go to [Strava API Settings](https://www.strava.com/settings/api)
2. Create a new application:
   - **Application Name**: Claude Coached (or anything you like)
   - **Category**: Training / Analysis
   - **Website**: `http://localhost`
   - **Authorization Callback Domain**: `localhost`
3. Note your **Client ID** and **Client Secret**

### 2b. Install the Strava MCP Server

Install the [Strava MCP server](https://github.com/nicobako/strava-mcp):

```bash
# Using uvx (recommended)
uvx strava-mcp

# Or using pip
pip install strava-mcp
```

### 2c. Configure in Claude Code

Add the Strava MCP server to your Claude Code settings. In your project directory, create or edit `.claude/settings.local.json`:

```json
{
  "mcpServers": {
    "strava": {
      "command": "uvx",
      "args": ["strava-mcp"],
      "env": {
        "STRAVA_CLIENT_ID": "your_client_id",
        "STRAVA_CLIENT_SECRET": "your_client_secret",
        "STRAVA_REFRESH_TOKEN": "your_refresh_token"
      }
    }
  }
}
```

### 2d. Authenticate with Strava

The first time you use the Strava MCP server, you'll need to complete the OAuth flow to get a refresh token. Follow the instructions in the [Strava MCP server docs](https://github.com/nicobako/strava-mcp) for authentication.

## Step 3: Set Up the Google Calendar MCP Server

The coach creates workout events on your calendar and checks for scheduling conflicts.

### 3a. Enable the Google Calendar API

1. Go to the [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project (or use an existing one)
3. Enable the **Google Calendar API**
4. Create **OAuth 2.0 credentials** (Desktop application type)
5. Download the credentials JSON file

### 3b. Install the Google Calendar MCP Server

Install the [Google Calendar MCP server](https://github.com/nspady/google-calendar-mcp):

```bash
npm install -g @nspady/google-calendar-mcp
```

### 3c. Configure in Claude Code

Add the Google Calendar MCP server to your `.claude/settings.local.json`:

```json
{
  "mcpServers": {
    "strava": { "..." : "..." },
    "google-calendar": {
      "command": "npx",
      "args": ["-y", "@nspady/google-calendar-mcp"],
      "env": {
        "GOOGLE_CLIENT_ID": "your_client_id",
        "GOOGLE_CLIENT_SECRET": "your_client_secret",
        "GOOGLE_REFRESH_TOKEN": "your_refresh_token"
      }
    }
  }
}
```

### 3d. Create a "Sport" Calendar

1. In Google Calendar, create a new calendar called **"Sport"** (or any name you prefer)
2. Go to **Settings** → select the sport calendar → **Integrate calendar**
3. Copy the **Calendar ID** (looks like `abc123@group.calendar.google.com`)
4. Paste this ID into `data/athlete.md` under **Sport Calendar ID**

This keeps your training events separate from your personal/work calendar.

## Step 4: Set Up the Intervals.icu MCP Server

Intervals.icu provides training analytics (CTL/ATL/TSB, power curves, zone analysis) and syncs structured workouts to your devices (Garmin, Wahoo, Zwift, COROS, and more).

### 4a. Create an Intervals.icu Account

1. Go to [intervals.icu](https://intervals.icu/) and sign up (free)
2. **Connect Strava**: Settings → Connections → Link Strava account. Activities will auto-sync.
3. **Connect your devices**: Settings → Connections → Link Garmin / Wahoo / Zwift / etc. This enables automatic workout sync — planned workouts will appear on your device.
4. Note your **Athlete ID** — visible in the URL when logged in: `intervals.icu/athlete/{id}` (e.g. `i123456`)

### 4b. Generate an API Key

1. Go to [Intervals.icu Settings](https://intervals.icu/settings)
2. Scroll to **Developer**
3. Generate an API key
4. Save it somewhere safe — you'll need it for the config

### 4c. Install the Intervals.icu MCP Server

The [Intervals.icu MCP server](https://github.com/eddmann/intervals-icu-mcp) is bundled in `intervals-icu-mcp/` with patches for known API issues. Install its dependencies and configure authentication:

```bash
cd intervals-icu-mcp
uv sync
uv run intervals-icu-mcp-auth
cd ..
```

The auth command will prompt you for your API key and athlete ID, storing them in a local `.env` file.

### 4d. Configure in Claude Code

Add the Intervals.icu MCP server to your project's `.mcp.json` (create it in the project root if it doesn't exist):

```json
{
  "mcpServers": {
    "intervals-icu": {
      "type": "stdio",
      "command": "uv",
      "args": ["run", "--directory", "/absolute/path/to/claude-coached/intervals-icu-mcp", "intervals-icu-mcp"]
    }
  }
}
```

Replace `/absolute/path/to/claude-coached` with the actual path to your project directory.

### 4e. Verify the Connection

Start Claude Code and ask: **"Can you fetch my recent activities from Intervals.icu?"**

If it works, you'll see activity data with pre-computed metrics (TSS, NP, IF). The coach will use these instead of calculating them manually from Strava streams.

## Step 5: Fill In Your Athlete Profile

Open `data/athlete.md` and fill in the placeholders. See `examples/athlete.example.md` for a complete example.

At minimum, fill in:
- **FTP** (Functional Threshold Power) — if unsure, use your best 20-minute power × 0.95
- **Weight**
- **Training availability** — hours per week and weekly pattern
- **Race calendar** — upcoming events with priorities
- **Sport Calendar ID** — from Step 3d
- **Intervals.icu Athlete ID** — from Step 4a

The more detail you provide, the better the coaching will be. But you can always add more later — the coach will ask for what it needs.

## Step 6: Start Coaching

```bash
cd claude-coached
claude
```

The coach will read your athlete profile and start the conversation. Try:

- **"Analyse my last ride"** — or use `/analyse` to run the workout analysis skill
- **"Build me a training plan"** — the coach will ask about your goals and constraints
- **"How should I prepare for [race name]?"** — race-specific advice
- **"Weekly review"** — summarise the past week's training

## Troubleshooting

### Strava MCP not connecting
- Check that your Client ID, Client Secret, and Refresh Token are correct in `.claude/settings.local.json`
- Make sure the refresh token hasn't expired — you may need to re-authenticate
- Test the connection: ask Claude "Can you fetch my recent Strava activities?"

### Google Calendar MCP not connecting
- Verify your OAuth credentials are correct
- Make sure the Google Calendar API is enabled in your Cloud Console project
- Test: ask Claude "Can you list my Google calendars?"

### No power data
- The coach works best with a power meter, but can adapt to heart rate and pace data
- If you only have HR data, the coach will use HR zones instead of power zones — update your HR zones in `data/athlete.md`

### Coach gives wrong zone targets
- Double-check your FTP in `data/athlete.md` — all zones are derived from this
- If your FTP feels wrong (workouts feel too easy or too hard), ask the coach to help you estimate it from recent data

### Intervals.icu RACE category returns 400 error
- The `RACE` event category triggers a different API schema in Intervals.icu and returns a 400 JSON parse error when creating events
- **Workaround**: Use `WORKOUT` category with race details in the description. The coach handles this automatically.
- This is an upstream Intervals.icu API issue, not a bug in the MCP server

### Settings file not found
- `.claude/settings.local.json` is created in the project directory, not your home directory
- This file is gitignored — it won't be committed (it contains your API secrets)
