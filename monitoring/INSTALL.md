# Monitoring installation

The immutable artifact installer calls `deploy/install_monitoring.sh` after all
four bot containers pass their health gate. It creates the `bitcoinlivemon` system user,
installs an exact hash-versioned Python environment, creates a root-owned
monitor-only env file, installs hardened systemd units, and starts only the
units whose enable flags are true. The installer selects exactly one of the
simulation, testnet, or live mode-specific API services.

Simulation-first install automatically generates a 64-hex-character monitor
token without printing it. Inspect it only when configuring a local MCP client:

```bash
sudo grep '^MONITOR_TOKEN=' /etc/bitcoin-live/simulation-monitor.env
curl -H 'Authorization: Bearer TOKEN' \
  http://127.0.0.1:8093/api/v1/health
```

The LIVE repository cannot assume a TestNet identity. Its testnet monitor
template is not an authenticated-TestNet validation path.

Telegram reporting uses a separate bot token in the monitor env. Set
`TELEGRAM_REPORTS_ENABLED=true` only after those values are populated, then:

```bash
sudo systemctl enable --now bitcoin-live-monitor-report-simulation.timer
```

For live, edit `/etc/bitcoin-live/live-monitor.env` only after the
exact live artifact completes promotion. Its template has
`MONITOR_ENABLED=false`, port 8093, a separate audit file, and reporting off.

Never copy `/etc/bitcoin-live/.env` into the monitor service. The
monitor units intentionally cannot read that trading-credential file.
