<!-- .github/copilot-instructions.md - guidance for AI coding agents on this repo -->
# Repo-specific instructions for AI coding agents

This repository contains documentation and configuration for monitoring a Linux node with Grafana Alloy and Grafana Cloud. The guidance below is concise and focused on patterns and workflows that let an AI agent be productive immediately.

1. Big picture
- **Purpose**: This is primarily a documentation/config repo that defines a Grafana Alloy configuration and a Grafana dashboard for a Linux node. The system collects host metrics, forwards them to Grafana Cloud (Prometheus remote_write and Loki), and stores dashboard JSONs under `dashboards/`.
- **Major components**: `config/config.alloy` (Alloy configuration), `dashboards/linux-node.json` (dashboard definition), `docs/` (extended docs), and top-level `README.md` (setup steps).
- **Data flows**: node exporters -> Alloy collectors -> `prometheus.remote_write` and `loki.write` endpoints in `config/config.alloy` -> Grafana Cloud. Relabeling blocks map exporter labels (`instance`, `job`) and filter metrics via `prometheus.relabel` rules.

2. Critical developer workflows (executable steps)
- Inspect or update Alloy config: `config/config.alloy` is a template with blank credentials/URLs. To test locally, copy it to `/etc/alloy/config.alloy` on a running Linux host and fill `url`, `username`, `password`, and `endpoint.url` entries.
- Restart Alloy service to apply changes:
  - `sudo systemctl restart alloy.service`
  - `sudo systemctl enable --now alloy.service`
  - `sudo systemctl status alloy` (expect `active (running)`)
- Import dashboard: open `dashboards/linux-node.json` → Grafana UI → Dashboards → Import → upload the JSON → select a Prometheus data source.

3. Project-specific conventions and patterns
- **Config style**: `config/config.alloy` uses named blocks (e.g. `prometheus.remote_write "metrics_service"`, `loki.write "grafana_cloud_loki"`) and `forward_to` semantics. When adding receivers/processors, follow the existing pattern: declare modules, then reference them in `forward_to` lists.
- **Relabeling pattern**: use `prometheus.relabel` rules to `keep` desired metrics (see existing `regex` for `up|node_arp_entries|...`). Use `target_label`/`replacement` to set `job` and `instance` values if needed.
- **Dashboard artifacts**: dashboard JSONs are self-contained — prefer editing in Grafana and exporting the JSON. Store exported JSON under `dashboards/` and update `dashboards/README.md` with any notable changes (data ranges, panels added).
- **Secrets and credentials**: the repository stores blanks for sensitive fields. Never add real API tokens or passwords to the repo. When proposing changes that require secrets, add instructions to use environment variables or document the manual injection steps.

4. Integration points and external dependencies
- **Grafana Cloud**: remote_write and Loki endpoints — set in `prometheus.remote_write.*` and `loki.write.*` blocks in `config/config.alloy`.
- **Alloy**: configuration file format visible in `config/config.alloy`. Examples of blocks to edit: `remotecfg`, `prometheus.remote_write`, `loki.write`, `prometheus.exporter.unix`.
- **Prometheus exporters**: `integrations_node_exporter` is the main exporter configured. Watch `disable_collectors`, `filesystem` excludes, and `netdev` device excludes to avoid noisy metrics.

5. Concrete examples to reference in code changes
- When adding a new remote_write target, follow the pattern:
  - `prometheus.remote_write "<name>" { endpoint { url = "" basic_auth { username = "" password = "" } } }`
- To add a new relabel rule that forces `job` label: use `prometheus.relabel "<name>" { rule { target_label = "job" replacement = "my/job" } }` and include it in `forward_to` lists.
- When updating dashboards, include the updated JSON in `dashboards/` and a short note in `dashboards/README.md` describing what changed.

6. What I couldn't automatically infer (ask user if needed)
- CI/build/test commands: there are no automated tests or build files present. If you expect unit tests or automation, ask where CI lives (external org pipeline?) and what commands to run.
- Any scripts referenced in `README.md` (`scripts/`) were not found in the repository root — confirm whether those live elsewhere.

7. Commit and PR guidance for AI agents
- Use small, focused commits. Example commit message formats:
  - `docs: update Alloy config example for new relay` 
  - `dashboards: export updated linux-node.json after panel change`
- Open a PR against `main` with a clear description and reference to the relevant files: `config/config.alloy`, `dashboards/linux-node.json`, and `dashboards/README.md`.

8. Quick file map (start here)
- `README.md` — high-level setup and prerequisites (contains systemd commands and manual steps).
- `config/config.alloy` — canonical Alloy config template used by this project; edit with care.
- `dashboards/linux-node.json` and `dashboards/README.md` — dashboard source and instructions for import/export.
- `docs/` — extended documentation; review before making doc edits.

If anything here is unclear or you want me to include more examples (e.g., a step-by-step editing example that updates a relabel rule and shows the resulting metrics change in Grafana), tell me which area to expand and I will iterate.
