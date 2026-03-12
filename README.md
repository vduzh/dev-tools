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
| Nexus | 8081 | Maven/Docker artifact repository |

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
