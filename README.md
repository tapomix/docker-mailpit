# Tapomix / Docker - Mailpit

[Mailpit](https://mailpit.axllent.org/) is a small, fast, low-memory email testing tool and API for developers.
This configuration runs Mailpit behind [Traefik](https://traefik.io/traefik) with security hardening (rootless, read-only, ...).

## Features

- Modern web UI to view and test captured emails
- SMTP server on port 1025
- REST API for integration testing
- HTML/spam checks and fast message processing
- SQLite database for persistent storage

## Installation

Clone the repository and navigate to the project directory:

```bash
git clone https://github.com/tapomix/docker-mailpit.git mailpit
```

## Configuration

Copy the environment template file and edit it with your values:

```bash
cp .env.dist .env
```

### Environment Variables

| Variable | Description |
| -------- | ----------- |
| `CONTAINER_NAME` | Name of the Docker container |
| `SERVICE_VERSION` | Docker image version to use (default: *`latest`*) |
| `TRAEFIK_HOST` | Hostname for Traefik routing (default: *`${CONTAINER_NAME}.domain.ext`*) |
| `TRAEFIK_NET` | Traefik network name (default: *`traefik-net`*) |
| `TZ` | Timezone (default: *`Etc/UTC`*) |
| `UID` | User ID for file permissions (default: *`1000`*) |
| `GID` | Group ID for file permissions (default: *`1000`*) |

## Usage

### Start the service

```bash
docker compose up -d
```

### Access the web interface

Open your browser and navigate to the configured `TRAEFIK_HOST` (e.g., `https://mailpit.domain.ext`).

### Send test emails

For dockerized services, add the Mailpit network to your service:

```yaml
services:
  my-app:
    # ...
    networks:
      - mail-net

networks:
  mail-net:
    external: true
    name: mailpit # or ${SERVICE_NET} if customized
```

Configure your application to use Mailpit as SMTP server:

- **Host**: `mailpit` (container name) or `localhost` (if exposed)
- **Port**: `1025`
- **Authentication**: None required (accepts any credentials)

### Expose SMTP port (optional)

If you need to send emails from outside the Docker network, add a port mapping in `compose.override.yaml`:

```yaml
services:
  mailpit:
    ports:
      - 1025:1025
```

### API endpoints

Mailpit provides a REST API for integration testing:

- `GET /api/v1/info` - Server information
- `GET /api/v1/messages` - List messages
- `GET /api/v1/message/{id}` - Get message details
- `DELETE /api/v1/messages` - Delete all messages
- `POST /api/v1/send` - Send a message via HTTP

Full API documentation: <https://mailpit.axllent.org/docs/api-v1/>

## Security

This configuration includes security hardening:

- **Rootless**: Runs as non-root user (`UID`/`GID`)
- **Read-only**: Filesystem is read-only with mounted volume for data
- **No new privileges**: Prevents privilege escalation
- **Dropped capabilities**: All capabilities are dropped

## Alternatives

- [MailHog](https://github.com/mailhog/MailHog) - Popular but no longer maintained (Mailpit is its spiritual successor)
- [Inbucket](https://inbucket.org/) - Disposable email platform with REST API
- [smtp4dev](https://github.com/rnwood/smtp4dev) - Cross-platform SMTP server with web interface

## Resources

- Mailpit documentation: <https://mailpit.axllent.org/docs/>
