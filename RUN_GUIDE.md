# Ikirezi: how to run everything

This guide covers Ubuntu, macOS and Windows (WSL). It assumes all four parts sit side by side in one folder:

```
ikirezi/
  backend/              FastAPI API            http://127.0.0.1:8000
  admin-dashboard/      Next.js admin console  http://localhost:3000
  professional-portal/  Next.js pro portal     http://localhost:3001
  mobile/               Flutter app
  docs/                 master plan, brand, this guide
```

Start the **backend first**. Every other part talks to it.

## 0. Install the tools (one time)

| Tool | Needed for | Install |
|---|---|---|
| uv | backend | `curl -LsSf https://astral.sh/uv/install.sh \| sh`, then reopen the terminal |
| Node.js 20+ | both web portals | https://nodejs.org (or `nvm install 20`) |
| Flutter 3.27+ | mobile app | https://docs.flutter.dev/get-started/install |
| Android Studio | Android emulator | https://developer.android.com/studio |

Check the installs:
```bash
uv --version && node -v && flutter --version
```

## 1. Backend (terminal 1)

```bash
cd ikirezi/backend
uv sync
cp .env.example .env
```

Open `.env` and set two random secrets. Generate each one with:
```bash
uv run python -c "import secrets; print(secrets.token_urlsafe(48))"
```
```
IKIREZI_JWT_SECRET=<paste value 1>
IKIREZI_ENCRYPTION_KEY=<paste value 2>
IKIREZI_DATABASE_URL=sqlite:///./ikirezi.db
```

Seed the data and start the API:
```bash
uv run python -m app.seed
uv run uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

Open http://127.0.0.1:8000/docs.

The seed creates these accounts. The password for all of them is `IKIREZI_SEED_ADMIN_PASSWORD`, which is `ChangeMe!2026x` by default.

| Role | Email | Signs in at |
|---|---|---|
| Super admin | admin@ikirezi.app | Admin dashboard |
| Professional | amani@ikirezi.app | Professionals portal |
| Mentor (contributor) | mentor@ikirezi.app | Mobile app |
| Member | member@ikirezi.app | Mobile app |

While SMTP is empty, **email codes (OTP) print in this terminal**, like this:
`[DEV EMAIL] ... Your Ikirezi code is 123456`.

Run the tests with:
```bash
uv run pytest -q
```

To start the database over, delete it, seed again and restart uvicorn:
```bash
rm ikirezi.db
uv run python -m app.seed
```

### PostgreSQL instead of SQLite (optional)

Create the user and database:
```bash
sudo -u postgres psql -c "CREATE USER ikirezi WITH PASSWORD 'ikirezi';"
sudo -u postgres psql -c "CREATE DATABASE ikirezi OWNER ikirezi;"
```

Then point `.env` at it:
```
IKIREZI_DATABASE_URL=postgresql+psycopg://ikirezi:ikirezi@localhost:5432/ikirezi
```

## 2. Admin dashboard (terminal 2)

```bash
cd ikirezi/admin-dashboard
npm install
cp .env.example .env.local
npm run dev
```

Open http://localhost:3000 and sign in as `admin@ikirezi.app`:
1. Enter the password.
2. Enter the email code from terminal 1.
3. On first login, scan the QR code with Google Authenticator or Microsoft Authenticator, then enter the 6-digit code from the app.

## 3. Professionals portal (terminal 3)

```bash
cd ikirezi/professional-portal
npm install
cp .env.example .env.local
npm run dev
```

Open http://localhost:3001 and sign in as `amani@ikirezi.app`. The steps are the same as the admin: password, email code, then authenticator.

## 4. Mobile app (terminal 4)

```bash
cd ikirezi/mobile
flutter create . --platforms=android,ios,linux --org com.telemedafrica
flutter pub get
```
The `flutter create` line only needs to run once.

Then run it on one of these devices. Each one needs a different address for the backend.

| Device | Command |
|---|---|
| Android emulator | `flutter run --dart-define=API_BASE_URL=http://10.0.2.2:8000/api/v1` |
| Real Android phone (same Wi-Fi) | `flutter run --dart-define=API_BASE_URL=http://<your-laptop-IP>:8000/api/v1` |
| Linux desktop (quick test) | `flutter run -d linux --dart-define=API_BASE_URL=http://127.0.0.1:8000/api/v1` |

To find your laptop's IP on Ubuntu, run `hostname -I`. Uvicorn must be started with `--host 0.0.0.0`, as in step 1.

To test an Android build, add this inside `<manifest>` in `android/app/src/main/AndroidManifest.xml`:
```xml
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.POST_NOTIFICATIONS"/>
```
Also add `android:usesCleartextTraffic="true"` to `<application>`. Local testing uses http, so the app needs this. Remove it before release.

Sign in as `member@ikirezi.app`, or create a new account. Registration rules:
- `country` is 2 letters (`RW`)
- `language` is `en` or `rw`
- the password has 10+ characters with an uppercase letter, a lowercase letter, a number and a symbol

## 5. Test it in Swagger (no app needed)

1. `POST /api/v1/auth/login` with `{"email":"member@ikirezi.app","password":"ChangeMe!2026x"}`. Copy `challenge_token` from the response.
2. `POST /api/v1/auth/verify-otp` with `{"challenge_token":"...","code":"<code from terminal 1>"}`. Copy `access_token` from the response.
3. Click **Authorize**, paste the token, and the padlocked endpoints open.

## Common problems

| Error | Fix |
|---|---|
| `unsupported operand type(s) for \|` | You ran system Python 3.9. Always use `uv run ...` |
| `password authentication failed for user "ikirezi"` | Use the SQLite URL above, or create the Postgres user |
| `422 Unprocessable Entity` on register | Check the `country`, `language`, `device_id` (8+ characters) and password rules |
| Admin login says "not available for your account" | That portal only accepts its own role. Admins use port 3000, professionals use 3001 |
| App can't connect | Backend not running, wrong `API_BASE_URL`, or the phone isn't on the same Wi-Fi |
| `EADDRINUSE :3000` | Something else is using the port. Run `npx kill-port 3000` or close it |
