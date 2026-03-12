# dev-tools

Shared development tools. Not tied to any specific project — serves all repositories.

## Quick Start

```bash
docker compose up -d
```

## Services

| Service | Port | Description |
|---------|------|-------------|
| Nexus | 8081 | Maven/Docker artifact repository |

## Nexus

### Web UI

- http://localhost:8081

### Initial setup

```bash
# Get initial admin password
docker exec nexus cat /nexus-data/admin.password
```

Login as `admin` with the password above, then set a new password through the UI.

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
