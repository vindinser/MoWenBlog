# AGENTS.md

## Project Overview

MOWEN is a full-stack blog + IM chat system (forked from POETIZE). Three packages in one repo, no workspace manager — each runs independently.

## Structure

| Directory | Stack | Port | Purpose |
|---|---|---|---|
| `mowen-ui/` | Vue 2 + Element UI | 8090 | Blog frontend |
| `mowen-im-ui/` | Vue 3 + Element Plus + Naive UI | 9529 | IM/chat frontend |
| `mowen-server/` | Spring Boot 2.7 + MyBatis-Plus | 9528 | Java backend |

## Commands

### Frontend (both packages)

```bash
cd mowen-ui  # or mowen-im-ui
npm install
npm run dev   # same as npm start / npm run serve
npm run build
npm run lint
```

**Critical**: All npm scripts set `NODE_OPTIONS=--openssl-legacy-provider` via `set` (Windows). If running on Linux/macOS, this `set` syntax won't work — use:
```bash
NODE_OPTIONS=--openssl-legacy-provider npm run dev
```

### Backend

```bash
cd mowen-server
mvn package          # builds poetry-web/target/mowen-server.jar
# or run PoetryApplication in IDE
```

Prerequisites: MySQL running, database `mowen` created, SQL imported from `mowen-server/sql/mowen.sql`. Edit `application.yml` datasource to point to your MySQL instance.

## Key Gotchas

- **No `npm start` / `npm run dev` / `npm run serve` distinction** — all three run `vue-cli-service serve`. Don't search for a missing script.
- **`lintOnSave: false`** in both `vue.config.js` files — ESLint won't run on save during dev. Run `npm run lint` manually.
- **IM frontend depends on blog frontend for auth** — login module lives in `mowen-ui`. IM is optional.
- **`application.yml` contains hardcoded credentials** (email, DB, Qiniu). These are committed; do not rotate or commit new ones.
- **File storage**: defaults to Qiniu cloud (`store.type: qiniu`). Local storage requires Nginx proxy to serve files.
- **Java 8 target** — `pom.xml` sets `<java.version>1.8</java.version>`. Do not bump to 17+ without migrating from `javax.*` to `jakarta.*`.
- **Windows-only scripts** — the `set` in npm scripts is Windows cmd syntax. CI or Linux dev needs adjustment.
- **No CI/CD configured** — no `.github/workflows`, no `.gitlab-ci.yml`.
- **Maven repo**: uses Huawei Cloud mirror (`mirrors.huaweicloud.com`), not Maven Central.

## Database

- Engine: MySQL
- Import: `mowen-server/sql/mowen.sql`
- Connection config: `mowen-server/poetry-web/src/main/resources/application.yml`
- Default DB user: `root`, DB name: `mowen`
- Druid connection pool, logical delete enabled (1=deleted, 0=active)

## Default Credentials

- Admin: username `ZS`, password `aaa`
