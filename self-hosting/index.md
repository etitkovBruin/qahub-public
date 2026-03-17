# Self-Hosting QA Hub Backend

QA Hub is a self-hosted QA platform. The Chrome extension connects to your own backend, so your QA data stays in infrastructure you control.

## Quick Start

1. Install the QA Hub extension from your Chrome Web Store listing.
2. Download the backend release bundle, or clone your public repository mirror:

```bash
git clone https://github.com/your-org/qahub-extension.git
cd qahub-extension
```

3. Copy `.env.example` to `.env` and set at least `JWT_SECRET_KEY`.
4. Start the production stack:

```bash
docker compose -f docker-compose.prod.yml up -d
```

5. Open the extension, enter your backend URL, and sign in.

## Release Bundle

Each backend release should publish:

- `docker-compose.prod.yml`
- `.env.example`
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
cp .env.example .env
```

Common values:

- `JWT_SECRET_KEY`: required, at least 32 characters
- `QAHUB_API_IMAGE`: published backend image name
- `QAHUB_VERSION`: backend image tag
- `API_PORT`: host port for the API, defaults to `5120`

Optional values cover TeamCity, Jira, GitHub, alert webhooks, and AI integration.

## Run the Production Stack

```bash
docker compose -f docker-compose.prod.yml up -d
```

Verify the deployment:

```bash
curl http://localhost:5120/health
curl http://localhost:5120/version
```

`/health` confirms the API and MongoDB are reachable. `/version` reports the backend version and the minimum compatible extension version.

## HTTPS

Chrome Web Store extensions can connect to `http://` only for localhost. For team or production use, expose QA Hub through HTTPS with a reverse proxy or load balancer.

Example Caddy configuration:

```text
qahub.yourcompany.com {
    reverse_proxy qahub-api:80
}
```

## Updating

1. Change `QAHUB_VERSION` in `.env`.
2. Pull the new image.
3. Restart the stack.

```bash
docker compose -f docker-compose.prod.yml pull
docker compose -f docker-compose.prod.yml up -d
```

MongoDB data remains in the `mongo-data` volume across upgrades.

## Development Mode

For local development, use the source-based compose file:

```bash
docker compose up -d
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
| MongoDB connection error | Confirm the `mongo` container is healthy and the connection string is correct |
| AI Test Creation finds no ticket | Open a Jira Cloud issue page and allow Atlassian access when prompted |
