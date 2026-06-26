# Nexlayer — gatus

<!-- nexlayer:meta version=1 analyzed=2026-06-26T04:28:49Z repo=https://github.com/armondhonore/gatus branch=master -->

> **For AI agents (Claude Code, Cursor, Gemini CLI, Copilot):**
> This file is the **project context** for this Nexlayer deployment — tech stack, env vars, secrets, live URL.
> For full platform detail (nexlayer.yaml schema, Dockerfile rules, CI/CD, task recipes) read **`nexlayer.skills`** in this repo.
>
> **Critical rules (full detail in `nexlayer.skills`):**
> - Inter-pod refs: `${podName:port}` only — never `localhost` or bare hostnames
> - Docker Hub images: prefix with `mirror.gcr.io/library/` — bare tags fail on the cluster
> - Secrets: set in the Nexlayer dashboard — never commit to `nexlayer.yaml` or Dockerfile
>
> **This file:** `agent-managed` sections update automatically. `user-editable` sections (Local Development Setup, Nexlayer Deployment Plan, Build Notes) are yours — preserved across re-analysis.

## Project Summary
<!-- nexlayer:section agent-managed=project_summary -->
Gatus is a developer-oriented health dashboard that monitors services via HTTP, ICMP, TCP, and DNS, evaluating results based on custom conditions and providing alerting integration.
<!-- nexlayer:end -->

## Technology Stack
<!-- nexlayer:section agent-managed=tech_stack -->
| Name | Kind | Version | Detected From |
|------|------|---------|---------------|
| Go | language | 1.26.3 | go.mod |
| Fiber | framework | v2.52.13 | go.mod |
| SQLite | database | modernc.org/sqlite | go.mod |
| PostgreSQL | database | lib/pq | go.mod |
<!-- nexlayer:end -->

## Repository Structure
<!-- nexlayer:section agent-managed=structure_map -->
- main.go — Application entry point
- api/ — REST API implementation
- controller/ — Core orchestration logic
- storage/ — Database abstraction layers
- web/ — Frontend and UI assets
- alerting/ — Notification system implementation
- watchdog/ — Health check scheduling logic
<!-- nexlayer:end -->

## External Services Required
<!-- nexlayer:section agent-managed=external_deps -->
Services that must be configured separately (not deployed by Nexlayer):

- AWS SES (for email alerts)
- Slack/Discord/PagerDuty (for alerting)
- GitHub/Gitea (for optional authentication/integration)
<!-- nexlayer:end -->

## Local Development Setup
<!-- nexlayer:section user-editable=local_setup -->
### Prerequisites

- Go >= 1.26.3
- Make

### Environment variables

Copy `.env.example` to `.env.local` and fill in:

```
GATUS_CONFIG_FILE=config.yaml
```

### Steps

1. `make build` — Compile the Go binary
2. `./gatus` — Run the application using the provided config.yaml

<!-- nexlayer:end -->

## Nexlayer Setup
<!-- nexlayer:section agent-managed=nexlayer_setup -->
### Pod Environment Variables

| Pod | Variable | Value | Kind |
|-----|----------|-------|------|
| `gatus-config` | `mountPath` | `/config` | plain |
| `gatus-config` | `size` | `1Gi` | plain |
| `gatus-data` | `mountPath` | `/data` | plain |
| `gatus-data` | `size` | `2Gi` | plain |

### nexlayer.yaml

```yaml
application:
  name: gatus
  pods:
  - name: app
    image: mirror.gcr.io/twinproduction/gatus:latest
    path: /
    servicePorts:
    - 8080
    volumes:
    - name: gatus-config
      mountPath: /config
      size: 1Gi
    - name: gatus-data
      mountPath: /data
      size: 2Gi
```

<!-- nexlayer:end -->

## Nexlayer Deployment Plan
<!-- nexlayer:section user-editable=deployment_plan -->
### Pod Topology

| Pod | Image | Port | Role |
|-----|-------|------|------|
| gatus | mirror.gcr.io/library/golang:1.26-alpine | 8080 | web |
| db | mirror.gcr.io/library/postgres:16-alpine | 5432 | database |

### Deployment notes

- Gatus is configured to use PostgreSQL for persistence in a production Nexlayer environment to avoid local file dependency on the web pod.
- The application pod connects to the database pod using the address db.pod:5432.

<!-- nexlayer:end -->

## Build Notes
<!-- nexlayer:section user-editable=build_notes -->
<!-- Add notes for future builds here — preserved across re-analysis -->
<!-- nexlayer:end -->

## Nexlayer Configuration
<!-- nexlayer:section agent-managed=nexlayer_config -->
**Last deployed:** 2026-06-26T05:02:07Z  
**Live URL:** https://relaxed-weasel-gatus.cloud.nexlayer.ai  
**Runtime:**  · **Port:** auto-detected  
**Deploy branch:** master  

```yaml
application:
  name: gatus
  pods:
  - name: app
    image: mirror.gcr.io/twinproduction/gatus:latest
    path: /
    servicePorts:
    - 8080
    volumes:
    - name: gatus-config
      mountPath: /config
      size: 1Gi
    - name: gatus-data
      mountPath: /data
      size: 2Gi
```
<!-- nexlayer:end -->

## Build History
<!-- nexlayer:section agent-managed=build_history -->
| Date | Status | Notes |
|------|--------|-------|
| 2026-06-26T04:28:49Z | analyzed | initial repo analysis |
| 2026-06-26T05:02:07Z | success | deployed https://relaxed-weasel-gatus.cloud.nexlayer.ai |
<!-- nexlayer:end -->
