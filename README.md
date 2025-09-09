# Jenkins Docker Setup

A Docker Compose setup for running Jenkins with Docker-in-Docker (DinD) support.

## Overview

This setup provides a complete Jenkins CI/CD environment with:
- Jenkins server running on port 8080
- Docker-in-Docker service for building Docker images
- Persistent data volumes
- Health checks and resource limits
- Logging configuration

## Prerequisites

- Docker and Docker Compose installed
- Available ports: 8080 (Jenkins UI), 2376 (Docker daemon), 50000 (Jenkins agents)

## Quick Start

1. Start the services:
```bash
docker compose up -d
```

2. Access Jenkins at `http://localhost:8080`

3. Stop the services:
```bash
docker compose down
```

## Services

### Jenkins
- **Image**: `jenkins/jenkins:2.526-jdk21`
- **Container**: `jenkins-core`
- **Ports**: 8080 (UI), 50000 (agent communication)
- **URL**: https://jenkins.zandome.dev/
- **Timezone**: Europe/Madrid

### Docker-in-Docker (DinD)
- **Image**: `docker:27-dind`
- **Container**: `docker-dind`
- **Port**: 2376 (Docker daemon)
- **Purpose**: Enables Jenkins to build Docker images

## Configuration

### Environment Variables
- `JENKINS_URL`: https://jenkins.zandome.dev/
- `TZ`: Europe/Madrid
- `JAVA_OPTS`: JVM optimization settings
- `DOCKER_HOST`: Connection to DinD service

### Resource Limits
- **Jenkins**: 4GB RAM, 2 CPU cores (limit), 1GB RAM, 0.5 CPU (reserved)
- **Docker DinD**: 2GB RAM, 1 CPU core (limit), 512MB RAM, 0.25 CPU (reserved)

### Volumes
- `jenkins_data`: Jenkins home directory persistence
- `jenkins_docker_certs`: Docker TLS certificates

## Health Checks
- Jenkins: Checks HTTP endpoint on port 8080
- Docker DinD: Verifies Docker daemon is running

## Logging
- Driver: json-file
- Max size: 10MB per file
- Max files: 3 rotating log files

## Network
- Custom bridge network `jenkins` for service communication
- Docker DinD accessible as `docker` hostname from Jenkins

## License

MIT License - see [LICENSE](LICENSE) file for details.