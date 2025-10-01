# Instagram Discord Bot - Architecture

## Core Technologies

- **Node.js v18+** - JavaScript runtime
- **TypeScript 5.x** - Type-safe development
- **Discord.js v14** - Discord API wrapper
- **Cheerio** - HTML parsing for Instagram scraping
- **Axios** - HTTP client
- **better-sqlite3** - Lightweight database
- **node-cron** - Task scheduler
- **dotenv** - Environment variables

---

## Project Structure & File Purposes

### `src/index.ts`
Entry point. Initializes Discord client, starts scheduler, handles graceful shutdown.

### `src/config.ts`
Loads and validates environment variables. Exports centralized config object.

### `src/types/instagram.types.ts`
TypeScript interfaces for Instagram post data and scraping responses.

### `src/services/instagram.ts`
Scrapes Instagram profile page with Cheerio, parses HTML, extracts post data, returns `InstagramPost[]` array.

### `src/services/discord.ts`
Manages Discord client connection, sends formatted embeds to target channel, handles rate limits.

### `src/services/database.ts`
SQLite operations: initialize schema, check if post already shared, mark posts as posted, prevent duplicates.

### `src/services/scheduler.ts`
Manages cron job. Periodically checks Instagram, filters new posts, posts to Discord, marks as posted.

### `src/utils/logger.ts`
Centralized logging with timestamps and log levels (info, warn, error).

---

## Data Flow

1. Bot starts → connects to Discord → initializes database → starts scheduler
2. Every 10 minutes → scrapes Instagram → filters new posts → posts to Discord → saves to database
3. Shutdown → stops scheduler → closes connections
