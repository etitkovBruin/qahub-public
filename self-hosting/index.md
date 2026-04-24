# Self-Hosting QA Hub Backend

QA Hub is a self-hosted QA platform. The Chrome extension connects to your own backend, so your QA data stays in infrastructure you control.

## Quick Start

1. Install the QA Hub extension from your Chrome Web Store listing.
2. Download the backend release bundle from your published release location, or clone the public QA Hub repository that you provide for self-hosted deployments.

3. Change into the extracted or cloned project directory, then copy `extension-ui/self-hosting/.env.example` to `extension-ui/self-hosting/.env` and set at least `JWT_SECRET_KEY` and `SECRETS_MASTER_KEY`. Generate your own values; do not reuse the placeholders.
4. Start the backend stack. If you cloned the source repository or are self-hosting from a laptop, use the source-build compose file:

```bash
docker compose -f extension-ui/self-hosting/docker-compose.yml up -d --build
```

If you are using a published backend image, set `QAHUB_API_IMAGE` and `QAHUB_VERSION`, then use the production compose file:

```bash
docker compose -f extension-ui/self-hosting/docker-compose.prod.yml up -d
```

5. Open the extension, enter your backend URL, and sign in.

## Release Bundle

Each backend release should publish:

- `extension-ui/self-hosting/docker-compose.prod.yml`
- `extension-ui/self-hosting/.env.example`
- `docs/self-hosting/index.md`
- `CHANGELOG.md` (optional)

That allows self-hosters to deploy without cloning the full repository.

## Prerequisites

| Tool | Minimum Version |
|---|---|
| Docker Engine | 20.10+ |
| Docker Compose | v2 |
| .NET SDK (optional) | 8.0+ |

## Configuration

Create your environment file:

```bash
cp extension-ui/self-hosting/.env.example extension-ui/self-hosting/.env
```

Common values:

- `JWT_SECRET_KEY`: required, at least 32 characters
- `SECRETS_MASTER_KEY`: required, at least 32 characters, used to protect stored environment and API secrets
- `QAHUB_API_IMAGE`: published backend image name, used by `docker-compose.prod.yml`
- `QAHUB_VERSION`: backend image tag, used by `docker-compose.prod.yml`
- `API_PORT`: host port for the API, defaults to `5120`

Optional values cover TeamCity, Jira, GitHub, alert webhooks, and AI integration.

## Run From Source

```bash
docker compose -f extension-ui/self-hosting/docker-compose.yml up -d --build
```

Verify the deployment:

```bash
curl http://localhost:5120/health
curl http://localhost:5120/version
```

`/health` confirms the API and MongoDB are reachable. `/version` reports the backend version and the minimum compatible extension version.

## Run a Published Image

Use this path only after you have published a real backend image and updated `QAHUB_API_IMAGE` and `QAHUB_VERSION` in `.env`.

```bash
docker compose -f extension-ui/self-hosting/docker-compose.prod.yml up -d
```

## HTTPS

Chrome Web Store extensions can connect to `http://` only for localhost. For team or production use, expose QA Hub through HTTPS with a reverse proxy or load balancer.

Example Caddy configuration:

```text
qahub.yourcompany.com {
    reverse_proxy qahub-api:8080
}
```

## Updating

1. Change `QAHUB_VERSION` in `.env`.
2. Pull the new image.
3. Restart the stack.

```bash
docker compose -f extension-ui/self-hosting/docker-compose.prod.yml pull
docker compose -f extension-ui/self-hosting/docker-compose.prod.yml up -d
```

MongoDB data remains in the `mongo-data` volume across upgrades.

## Development Mode

For local development, use the source-based compose file, which builds the API from the repo-root `Dockerfile`:

```bash
docker compose -f extension-ui/self-hosting/docker-compose.yml up -d
```

Or run the API directly:

```bash
dotnet run --project QAHubAPI
```

## Chrome Extension Setup

On first launch the extension shows a "Connect to your backend" screen. Enter your public backend URL, save it, and then sign in or register.

## Troubleshooting

| Symptom | Fix |
|---|---|
| Extension cannot connect | Verify the backend URL, port, and HTTPS configuration |
| `JWT_SECRET_KEY` startup error | Set a random secret with at least 32 characters |
| `SECRETS_MASTER_KEY` startup error | Set a separate random secret with at least 32 characters and keep it stable across upgrades |
| MongoDB connection error | Confirm the `mongo` container is healthy and the connection string is correct |
| AI Test Creation finds no ticket | Open a Jira Cloud issue page and allow Atlassian access when prompted |
