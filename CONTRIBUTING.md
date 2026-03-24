# Contributing to Claude Coached

Thanks for your interest in making the AI cycling coach better. Contributions of all kinds are welcome.

## Types of Contributions

- **Coaching prompts** — Improve existing protocols in `CLAUDE.md` or add new ones (nutrition, recovery, mental training)
- **Skills** — New slash commands in `.claude/skills/` (weekly review, plan builder, race prep, FTP testing)
- **Code** — Scripts, tools, and utilities that enhance the coaching experience (metric calculations, charts, data processing, workout file generators)
- **Integrations** — New MCP server support (intervals.icu, TrainingPeaks, Wahoo, Garmin)
- **Documentation** — Setup guides, tutorials, examples, translations
- **Bug reports** — Wrong metric calculations, bad coaching advice, broken workflows

## Getting Started

1. Fork the repo
2. Create a branch for your change
3. Make your changes
4. Test your changes (see below)
5. Open a pull request

## Testing

This project doesn't have a traditional test suite (yet). When submitting changes:

- **Prompt/skill changes**: describe how you tested the coaching interaction — what you asked, what the coach did, and whether the output was correct
- **Metric calculations**: verify against a known source (Strava, TrainingPeaks, intervals.icu, or manual calculation)
- **New integrations**: confirm the MCP server connects and returns expected data
- **Documentation**: ensure all steps work on a fresh setup

## Style Guide

### Prompts and Skills
- Be specific and actionable — "compute NP using 30s rolling average" not "calculate power"
- Use markdown formatting consistently with the existing `CLAUDE.md` structure
- Include formulas where applicable — don't assume the model knows sport science by heart
- Test with real workout data when possible

### Code
- Keep it simple — prefer a standalone script over an overengineered framework
- Python for data processing and calculations
- Include comments for non-obvious sport science logic
- No external dependencies unless truly necessary

### Documentation
- Write for a technical audience (Claude Code users) but don't assume cycling domain knowledge
- Include examples and expected output
- Keep the setup guide up to date with any new requirements

## PII Check

Before submitting, grep your changes for any personal data:
- Real names, Strava IDs, calendar IDs
- Specific race results or power numbers that identify a real person
- API keys or tokens

The PR template includes a PII checklist — please complete it.

## Contributor License Agreement (CLA)

This project uses a [CLA](CLA.md). When you open your first pull request, you'll be asked to sign it via the CLA Assistant bot.

**Why?** The CLA grants the project owner a license to use contributions under any license, including commercial licenses. This preserves the option for dual-licensing in the future while keeping the project open-source under AGPLv3. It does not transfer your copyright — you retain full ownership of your contributions.

## Questions?

Open an issue or start a discussion. No question is too basic.
