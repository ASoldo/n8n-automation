# AGENTS.md

## Global Goals
- **Intent‑driven resources**  
  Every Kubernetes manifest should declare clear intent (e.g. “n8n web UI”, “PostgreSQL data store”).  
- **Documentation first**  
  Whenever you add or modify a service folder, link to the relevant upstream docs (e.g. Minikube, Helm charts, official k8s API) in the folder’s `README.md`.

## Repository Layout
- `k8s/` – top‑level namespace & infrastructure  
- `k8s/<service>/` – one service per sub‑folder  
- `AGENTS.md` – this file, merged first  
- `README.md` – high‑level project overview and getting‑started

## Naming Conventions
- **Resource names**: `<service>-<resource>`, all lowercase, dashes (`-`) only  
  - e.g. `n8n-deployment`, `harbor-service`
- **Namespaces**: match folder name, e.g. `namespace: n8n`, `namespace: harbor`

## YAML Quality & Linting
- Run `kubectl apply --dry-run=client -f <folder>/` on every change.  
- Lint YAML with `yamllint --config-file .yamllint.yml` (requires two-space indent, no tabs).  
- Validate against Kubernetes schemas via `kubeval --kubernetes-version="1.28.0"`.

## Deployment & Testing
- **Minikube smoke tests**:  
  1. `minikube start`  
  2. `kubectl apply -f k8s/_root/namespace.yaml`  
  3. `for svc in harbor n8n nginx-protected ollama postfix-relay postgres twingate wireguard; do`  
     `kubectl apply -f k8s/$svc && kubectl rollout status deploy/$svc; done`  
- **Health checks**:  
  - Ensure each Service has a corresponding `readinessProbe` and `livenessProbe`.  
  - HTTP services must return `2xx` on `/healthz` or equivalent endpoint.

## Versioning & Rollbacks
- All image tags must be explicit (no `:latest`).  
- If you bump a container image, add a note in `CHANGELOG.md` with version and release date.

## PR / Commit Guidelines
- **Format**:  
  [<service>] <short summary>
  What changed
  Why it’s needed

