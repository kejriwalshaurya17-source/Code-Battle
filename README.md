# CodeBattle

CodeBattle is a real-time competitive debugging MVP for SIH 2026 Smart Education.

## Current MVP
- Real Socket.IO ranked matchmaking by language + difficulty.
- First-to-3 rounds with role rotation.
- Server-authoritative timers, roles, scores, sabotage limits and results.
- Common procedural problem-statement + starter-code generation powers ranked multiplayer, Friend Room, Computer Battle and Solo Practice. It uses multiple problem families per difficulty, fresh randomized test data and unique per-round challenge IDs, so the standard challenge pool is not a finite list of repeated questions.
- Sabotage budget is randomly 1 or 2 changed lines per round.
- During debugging, the Saboteur receives a read-only live stream of the Debugger's code edits.
- Computer Battle uses the same battle loop with CodeBot sabotage/debugging, difficulty-based behavior and intentional-loss probability.
- Language and difficulty selection is available for ranked, computer and solo practice.
- Friend rooms have a working create/join UI, selected language + difficulty, optional validated custom challenges, and otherwise consume the same common generator as every other battle mode.
- Ranked leaderboard is persisted per difficulty; only registered ranked wins earn points.
- Ranked fullscreen monitoring: 3 exits or more than 10 seconds outside fullscreen forfeits the match.
- Local JavaScript/Python execution is isolated behind a replaceable execution service boundary.

## Run
```
npm install
npm --prefix client install
npm run build
npm start
```
Open http://localhost:4000.

For ranked multiplayer, use two registered browser sessions with the same language and difficulty.

## Production replacement
The local executor uses child processes for MVP development. Before public deployment replace it with an isolated container/sandbox worker with CPU, memory, time, filesystem and network restrictions. Add Redis for matchmaking and PostgreSQL for durable scale.

## Admin Control Center

The built-in primary admin account is the registered username **`admin`**. When the user `admin` logs in, the Dashboard shows an **Admin Panel** action. The panel is intentionally hidden from every other username.

Admin actions are enforced server-side and require an authenticated admin session. The panel can:

- View registered users and platform/match totals
- Reset a user's password (sessions are revoked after reset)
- Grant or remove admin permissions for other accounts
- Set a user's leaderboard points and wins per difficulty
- Reset a user's leaderboard stats and match history
- Permanently delete a user account (the primary `admin` account cannot be deleted)

The `Users` table includes an `is_admin` permission flag, and existing SQLite databases are migrated automatically when the server starts.

**Important:** keep the `admin` account password private. The UI restriction is based on the exact username `admin`; API authorization also accepts explicitly promoted admin accounts for future/admin tooling.

## Render deployment

This prototype is configured to run as a single Render web service.

- Build command: `npm run install:all && npm run build`
- Start command: `npm start`
- Node: 22.5+
- Optional environment variable: `ADMIN_PASSWORD` (defaults to `CodeBattleAdmin123!` for the prototype)

The client automatically uses the same origin in production, while local Vite development continues to use `http://localhost:4000`.
