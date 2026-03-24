# Roadmap

Feature ideas and future directions for Claude Coached. This is an unprioritised long list — not a commitment or a sequence. Contributions welcome — see [CONTRIBUTING.md](../CONTRIBUTING.md).

## New Skills

- `/weekly-review` — Automated weekly training review: fetch all activities, compute weekly TSS/hours, compare planned vs actual, update training log
- `/build-plan` — Interactive training plan builder: gather goals and constraints, generate periodized mesocycles, create calendar events
- `/race-prep` — Race-specific preparation: taper protocol, pacing strategy, nutrition plan, equipment checklist
- `/ftp-test` — FTP test protocol: prescribe test, analyse results, update zones across all files
- `/compare` — Compare two activities side-by-side: same route over time, interval progression, fitness tracking

## New Integrations

- **[intervals.icu](https://intervals.icu/)** — Richer analytics, fitness/fatigue charts, power curve tracking
- **Weather API** — Factor weather into outdoor ride planning (heat, wind, rain)
- **[TrainingPeaks](https://www.trainingpeaks.com/)** — Import/export workouts for athletes already on the platform
- **Wahoo / Garmin Connect** — Direct device sync for athletes not on Strava
- **Indoor apps (Zwift, Rouvy, MyWhoosh, etc.)** — Push structured workouts to indoor platforms, pull completed ride data back
- **Nutrition tracking** — Integration with MyFitnessPal or similar for fueling analysis

## Coaching Enhancements

- **Running analysis** — Pace/HR analysis for multisport athletes, running power support
- **HR-only mode** — Full coaching for athletes without a power meter, using heart rate zones and RPE
- **Mental training** — Pre-race visualization, process goals, dealing with race anxiety
- **Heat and altitude protocols** — Acclimatization plans, adjusted zones for environmental stress
- **Women's health periodization** — Adapt training load across the menstrual cycle
- **Injury prevention** — Monitor training load spikes, suggest mobility/prehab work
- **Group ride analysis** — Identify surges, drafting efficiency, match-burning

## Quality of Life

- **CTL/ATL/TSB charts** — Generate fitness/fatigue/form plots from training log data
- **Season summaries** — End-of-season report with progression, highlights, year-over-year comparison
- **Workout library** — Generate structured workout files (.zwo, .fit, .erg) ready to sync to head units or indoor apps
- **Equipment tracking** — Component mileage, maintenance reminders, tire/chain wear estimates
- **Export/import** — Backup and restore athlete data, migrate between setups
- **Multi-athlete support** — Coach multiple athletes from separate data directories

## Infrastructure

- **Test suite** — Validate metric calculations (NP, TSS, IF, decoupling) against known values
- **CI checks** — Lint CLAUDE.md and skills for structural issues, PII scanning
- **Setup wizard** — Interactive script to guide MCP server configuration and athlete profile creation
