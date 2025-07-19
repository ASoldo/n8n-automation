<!--
AGENTS.md - Registry of automation agents used in this project
-->

# Agents

> This document serves as a central registry and usage guide for all automation agents defined or supported in this repository. An *agent* is a self‑contained component or service that performs specific tasks (e.g., interacting with Kubernetes, APIs, or other systems) as part of an automation workflow.

> **Table of Contents**

- [Agents Overview](#agents-overview)
- [Agent Registry](#agent-registry)
- [Defining a New Agent](#defining-a-new-agent)
- [Configuring Agents](#configuring-agents)
- [Running Agents](#running-agents)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)

## Agents Overview

Agents encapsulate reusable automation logic or connectivity to external systems. By defining agents centrally, you can maintain consistent configuration, environment variables, and runtime parameters across workflows.

Key benefits:

- **Reusability**: Share agents across multiple workflows or projects.
- **Consistency**: Standardize environment and credentials management.
- **Isolation**: Run agents in separate containers or processes for security and stability.

## Agent Registry

| Name           | Version       | Description                                    | Config Location             |
| -------------- | ------------- | ---------------------------------------------- | --------------------------- |
| `kubernetes`   | v1.0.0        | Executes `kubectl` commands against a cluster   | `agents/kubernetes/config`  |
| `http-client`  | v2.1.0        | HTTP client for RESTful API interactions        | `agents/http-client/`       |
| `database`     | v1.3.2        | Database connectivity (PostgreSQL, MySQL, etc.) | `agents/database/config`    |

> _Add or update entries above as new agents are introduced or versioned._

## Defining a New Agent

To introduce a new agent:

1. Create a directory under `agents/` with the agent name.
2. Provide an executable script or Dockerfile for the agent logic.
3. Add a configuration subdirectory or file for environment variables, credentials, and defaults.
4. Document the agent entry in the [Agent Registry](#agent-registry) table above.

```text
agents/
├── my-agent/
│   ├── Dockerfile
│   ├── run.sh         # entrypoint wrapper
│   └── config/
│       └── example.env
```

## Configuring Agents

Each agent may require configuration elements such as credentials, endpoint URLs, or runtime flags. Store these configuration files in the agent’s `config/` directory and load them at startup.

Example `.env` format:

```ini
# agents/kubernetes/config/example.env
KUBECONFIG=/path/to/kubeconfig
NAMESPACE=default
TIMEOUT=60s
```

> **Tip:** Do not commit sensitive credentials to Git. Use secrets management or CI/CD variables.

## Running Agents

You can invoke agents directly (e.g., via scripts or containers) or integrate them into CI/CD pipelines and workflows.

### Local execution

```bash
# Source configuration
source agents/kubernetes/config/example.env
# Run the agent script
bash agents/kubernetes/run.sh apply -f ./manifests/
```

### Docker-based execution

```bash
# Build the agent image
docker build -t my-org/kubernetes-agent:latest agents/kubernetes/

# Run with mounted config
docker run --rm \
  --env-file agents/kubernetes/config/example.env \
  my-org/kubernetes-agent:latest apply -f /manifests/
```

## Best Practices

- **Version pinning**: Tag agent Docker images or dependencies to avoid unintended upgrades.
- **Least privilege**: Grant agents only the necessary permissions (e.g., RBAC roles in Kubernetes).
- **Logging & Monitoring**: Ensure agents emit structured logs and metrics for observability.
- **Immutable configurations**: Treat configuration files as code and track changes via Git.

## Troubleshooting

- Check agent logs and exit codes for error details.
- Validate configuration files (`env`, JSON, YAML) before running agents.
- Use verbose or debug flags if supported (`--verbose`, `--debug`).

## Contributing

Contributions to agents (bug fixes, new features, documentation) are welcome:

1. Fork the repository.
2. Create a feature branch: `git checkout -b agent-<name>`.
3. Update the agent source, configuration, and this documentation.
4. Submit a pull request describing your changes.

---
*This file was generated to help maintain and use automation agents consistently across the project.*
