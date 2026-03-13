# dev-tools

Shared development tools. Not tied to any specific project — serves all repositories.

## Quick Start

```bash
docker compose up -d
```

## Commands

```bash
docker compose up -d       # start
docker compose down        # stop (data preserved)
docker compose down -v     # stop and delete all data
```

## Services

| Service | Port | Description |
|---------|------|-------------|
| Nexus | 8081 | Web UI, Maven repository |
| Nexus Docker | 8082 | Docker registry (hosted) |

## Nexus

### Web UI

- http://localhost:8081

### Initial setup

1. Open http://localhost:8081
2. Click **Sign In** (top right)
3. Login: `admin`, password:
   ```bash
   docker exec nexus cat /nexus-data/admin.password
   ```
4. Nexus will prompt you to set a new password (e.g. `admin123` for local dev)
5. Choose "Enable anonymous access" if you want Gradle/Maven to read without credentials

### Usage in Gradle/Maven

```kotlin
// build.gradle.kts
repositories {
    maven {
        url = uri("http://localhost:8081/repository/maven-public/")
    }
}

publishing {
    repositories {
        maven {
            url = uri("http://localhost:8081/repository/maven-releases/")
            credentials {
                username = "admin"
                password = "admin123"
            }
        }
    }
}
```

### Docker Registry Setup

After initial Nexus setup, create a Docker hosted repository:

1. Open http://localhost:8081 → **Settings** (gear icon) → **Repositories** → **Create repository**
2. Select **docker (hosted)**
3. Configure:
   - **Name:** `docker-hosted`
   - **HTTP port:** `8082`
   - **Enable Docker V1 API:** unchecked
4. Click **Create repository**

#### Docker client configuration

Nexus runs over HTTP locally, so Docker needs to trust it as an insecure registry.

Add to Docker Desktop → **Settings** → **Docker Engine** (`daemon.json`):

```json
{
  "insecure-registries": ["localhost:8082"]
}
```

Restart Docker Desktop after saving.

#### Usage

```bash
docker build -t localhost:8082/ms-profile:latest .
docker login localhost:8082 -u admin -p admin123
docker push localhost:8082/ms-profile:latest
```

### Data

All data is stored in the `nexus-data` Docker volume. Survives container restarts.

To fully reset:

```bash
docker compose down -v
docker compose up -d
```

## Configuration

Ports are configurable via `.env`:

| Variable | Default | Description |
|----------|---------|-------------|
| `NEXUS_PORT` | 8081 | Nexus UI and API |
| `NEXUS_DOCKER_PORT` | 8082 | Docker registry |
