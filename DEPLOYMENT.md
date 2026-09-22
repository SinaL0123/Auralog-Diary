# Personal fork deployment: `auralogdiary.com`

This guide prepares a **new, personal deployment** of the course project. It does not transfer ownership of the team repository, its secrets, data, Docker Hub account, or the FastAPI/Gemini/Faster-Whisper work. Keep the original attribution in the fork. Sina Liu's documented project work is the Flask user workflows, tests/CI, Dockerfiles, and Docker Compose integration.

## What this configuration does

`docker-compose.personal-production.yml` builds the checked-out source on one Droplet, runs MongoDB and the AI service only on the internal Docker network, and exposes the Flask app only through Caddy on ports 80 and 443. `Caddyfile` serves `auralogdiary.com` and obtains/renews HTTPS automatically after DNS points to the Droplet.

Use a 2 GB Droplet for the initial build and smoke test. DigitalOcean's 1 GB Basic Droplet is listed at $6/month and 2 GB at $12/month; it uses per-second billing with a 60-second/$0.01 minimum. The application has MongoDB plus Python/Faster-Whisper in one host, so 512 MB is not a reliable deployment target. Check the live price before creating resources: [DigitalOcean Droplet pricing](https://www.digitalocean.com/pricing/droplets).

## Required user actions (do these yourself)

1. **GitHub:** Sign in, open the public team repository, use **Fork**, and select the personal account. Clone that fork. Do not copy or request the former repository's Actions secrets. This local checkout has not been forked or pushed.
2. **DigitalOcean:** Create a new account, enable MFA and billing alerts, add an SSH public key, and create a 2 GB Ubuntu Droplet. Restrict SSH (22) to the administrator's IP in the cloud firewall; allow only TCP 80 and 443 publicly. Do not expose 27017 or 8000.
3. **Domain DNS:** In the domain registrar's DNS panel, create an `A` record for `@` pointing to the Droplet's public IPv4 address. Wait for it to resolve before starting Caddy. Do not add `www` unless you also add it to `Caddyfile` and create a matching DNS record.
4. **Gemini:** Create a fresh, restricted Gemini API key in the owner's Google AI Studio account. It is required for live conversation and diary-generation requests. Never commit it or reuse a former team key.

## Personal fork handoff (no push has been performed)

The intended personal remote is `https://github.com/SinaL0123/Auralog-Diary`. This local checkout still points to the team repository and has not been reconfigured or pushed. After reviewing the local diff, the owner can preserve the team repository as `upstream` and set the personal fork as `origin`:

```bash
git remote rename origin upstream
git remote add origin https://github.com/SinaL0123/Auralog-Diary.git
git remote -v
```

Use a review branch rather than pushing directly to `main`. The exact push commands, to run only after explicit confirmation, are:

```bash
git switch -c codex/personal-deployment-hardening
git add .gitignore web-app/app.py web-app/tests/test_app.py .env.production.example Caddyfile docker-compose.personal-production.yml DEPLOYMENT.md
git commit -m "Prepare secure personal deployment"
git push -u origin codex/personal-deployment-hardening
```

The pre-existing local benchmark files (`BENCHMARKS.md`, `scripts/`, and `benchmark-results.json`) may be reviewed and committed separately. This keeps the deployment hardening review focused.

## On-Droplet steps after the actions above

```bash
git clone https://github.com/YOUR-ACCOUNT/5-final-finally.git auralog
cd auralog
cp .env.production.example .env.production
openssl rand -hex 32
# Put the generated value in FLASK_SECRET_KEY, then add the fresh GEMINI_API_KEY and CADDY_EMAIL.
docker compose --env-file .env.production -f docker-compose.personal-production.yml up --build -d
docker compose --env-file .env.production -f docker-compose.personal-production.yml ps
curl -I https://auralogdiary.com
```

The production setting fails the Flask service closed if `FLASK_SECRET_KEY` is missing. It also sends `Secure`, `HttpOnly`, `SameSite=Lax` session cookies through HTTPS.

## Data recovery and authentication migration

If an old Mongo export is recovered, restore it into this new volume only after keeping an encrypted, verified backup. This code recognizes a legacy user document whose `password` field is plaintext; on that user's first successful login, it replaces that value with a Werkzeug password hash. This is intentionally gradual because plaintext passwords cannot be pre-hashed without the user supplying them. Accounts that never sign in retain their legacy value, so force a password-reset process before treating a recovered user database as broadly public.

The original project has no automatic Mongo backup. After the first successful smoke test, take an encrypted off-host `mongodump` and test restoring it into a separate Mongo instance. Do not treat a Docker volume as a backup.

## Checks and rollback

Use a disposable account to check registration, login/logout, a text conversation, diary creation, and diary visibility. A live AI check uses Gemini quota. If any check fails, stop the stack with `docker compose -f docker-compose.personal-production.yml down`; do **not** add `-v`, which deletes the Mongo volume. Keep `.env.production` and backup archives off Git.

For a source update, pull a reviewed commit, take a Mongo export first, then run `docker compose --env-file .env.production -f docker-compose.personal-production.yml up --build -d`. The personal compose file is intentionally separate from the original team's Docker Hub/Actions deployment files.
