# Cross-Platform Installation and Run Guide (macOS, Windows, Linux)

This guide explains how to run AppBoilerplate on every major operating system.

## 1) Required tools (all OSes)

- **Node.js 18+** (Node 22 recommended)
- **pnpm 9+**
- **Docker 20+** (only required for self-hosted Convex)
- **Android Studio** (required for Android emulator/dev builds)

Install pnpm:

```bash
corepack enable
corepack prepare pnpm@9 --activate
```

## 2) Platform-specific notes

### macOS

- Supports **iOS + Android** development.
- Install **Xcode 15+** for iOS simulator and iOS builds.
- Install **Android Studio** for Android emulator.

### Windows

- Supports **Android** development.
- iOS simulator/build tools are **not available** on Windows.
- Install Android Studio and ensure `adb` is in PATH.

### Linux

- Supports **Android** development.
- iOS simulator/build tools are **not available** on Linux.
- Install Android Studio and ensure `adb` is in PATH.

## 3) Clone and bootstrap

```bash
git clone https://github.com/diaspoai/AppBoilerplate.git my-app
cd my-app
pnpm install
```

Copy env file:

### macOS/Linux

```bash
cp apps/mobile/.env.development.example apps/mobile/.env.development
```

### Windows PowerShell

```powershell
Copy-Item apps/mobile/.env.development.example apps/mobile/.env.development
```

## 4) Start backend (self-hosted Convex via Docker)

```bash
cd packages/backend
npx degit get-convex/convex-backend/self-hosted/docker/docker-compose.yml docker-compose.yml
docker compose pull
docker compose up -d
docker compose exec backend ./generate_admin_key.sh
```

Create `packages/backend/.env.local` and set:

```env
CONVEX_SELF_HOSTED_URL=http://127.0.0.1:3210
CONVEX_SELF_HOSTED_ADMIN_KEY=<generated-admin-key>
```

Run Convex dev:

```bash
npx convex dev
```

## 5) Start the mobile app

From repo root:

```bash
pnpm dev
```

Or from `apps/mobile`:

```bash
pnpm dev
```

### Device/emulator commands

- **Android (all OSes):**
  ```bash
  pnpm expo run:android
  ```
- **iOS (macOS only):**
  ```bash
  pnpm expo run:ios
  ```

## 6) Why scripts now work cross-platform

Project scripts use `cross-env` where environment variables are needed:

- `apps/mobile`:
  - `pnpm dev`
  - `pnpm dev:staging`
- `packages/backend`:
  - `pnpm lint`

This avoids shell-specific syntax issues between bash/zsh and Windows shells.
