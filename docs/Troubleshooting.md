# Troubleshooting Guide

This guide covers the most common issues when running the Linux Node integration with Grafana Alloy. For comprehensive troubleshooting, refer to the [official Grafana Cloud troubleshooting documentation](https://grafana.com/docs/grafana-cloud/monitor-infrastructure/integrations/troubleshoot/install-troubleshoot-linux-alloy/).

---

## Most Common Issue: No Metrics Found

### Problem
You've installed Alloy successfully, but you see the message: **"No metrics were found for this integration"** in Grafana Cloud.

### What This Means
This message means that Alloy started and connected to your Grafana Cloud Prometheus endpoint, but failed to scrape your application Prometheus metrics endpoint.

### Quick Checks

**1. Verify Alloy is Running**

Run `systemctl status alloy` and validate that Alloy is running. You should see it listed as Active: active (running).

**2. Check Alloy Logs**

View recent logs to identify errors:
```bash
sudo journalctl -u alloy -n 50 --no-pager
```

**3. Verify Local Metrics Endpoint**

Check if metrics are being collected locally:
```bash
curl http://localhost:12345/metrics
```

You should see a long list of metrics. If this works, the problem is with sending data to Grafana Cloud.

---

## Common Error Messages

### "Active: inactive (dead)"

The Alloy service has been stopped. Run `systemctl start alloy` and check again.

### "Active: failed"

Alloy has encountered a startup error. Check the logs for specific error messages.

---

## Installation Issues

### Missing Prerequisites

**curl is not installed:**
The script uses curl to download the Alloy installation package, so this is a pre-requisite to execute it.

**gzip is not installed:**
The script uses gzip to decompress the Alloy installation package, so this is a pre-requisite to execute it.

Install missing packages:
```bash
sudo apt update
sudo apt install curl gzip
```

### Unsupported Distribution

Unfortunately, Alloy doesn't yet support your Linux distribution. Check the [official help page](https://grafana.com/docs/grafana-cloud/get-help/) for support options.

---

## Connection Issues

### Cannot Connect to Grafana Cloud

**Possible causes:**
- Incorrect URL or authentication details
- Firewall blocking outbound HTTPS (port 443)
- Network connectivity problems

**What to check:**

If you find errors about connecting to your Grafana Cloud Prometheus endpoint, make sure you didn't change the URL or authentication details. You can verify your endpoint and create new keys under the Cloud Portal.

Test connectivity:
```bash
curl -I https://prometheus-prod-13-prod-us-east-0.grafana.net
```

---

## Configuration Issues

### Configuration Errors

If there are component configuration error logs, make sure you have followed all instructions provided. You can check Alloy components reference documentation for help.

**Check your configuration file:**
```bash
sudo nano /etc/alloy/config.alloy
```

Verify:
- No syntax errors
- Correct Grafana Cloud endpoints
- Valid authentication tokens

---

## Service Not Starting After Reboot

If the Alloy service runs when started manually but doesn't run after a reboot, run the following command to enable the service at boot: `sudo systemctl enable alloy`.

---

## Using the Alloy UI for Debugging

Alloy includes a web interface for debugging at `http://localhost:12345`.

**What you can check:**
- Component health status
- Configuration validation
- Live metrics collection
- Recent errors and warnings

**Access the UI:**
```bash
# If accessing remotely, use SSH port forwarding
ssh -L 12345:localhost:12345 user@your-server
```

Then open `http://localhost:12345` in your browser.

For more details, see: [Debug Grafana Alloy](https://grafana.com/docs/alloy/latest/troubleshoot/debug/)

---

## Getting Help

If you've tried the steps above and still have issues:

1. **Check official documentation:**
   - [Linux Alloy Troubleshooting](https://grafana.com/docs/grafana-cloud/monitor-infrastructure/integrations/troubleshoot/install-troubleshoot-linux-alloy/)
   - [Alloy Debug Guide](https://grafana.com/docs/alloy/latest/troubleshoot/debug/)

2. **Gather diagnostic information:**
   - Alloy logs: `sudo journalctl -u alloy -n 100`
   - Service status: `systemctl status alloy`
   - Configuration file: `/etc/alloy/config.alloy`

3. **Contact Grafana Support:**
   - [Official Help Page](https://grafana.com/docs/grafana-cloud/get-help/)
   - Grafana Labs Community Forums

---

## Quick Reference: Troubleshooting Commands

```bash
# Check if Alloy is running
sudo systemctl status alloy

# Start Alloy
sudo systemctl start alloy

# Enable Alloy to start on boot
sudo systemctl enable alloy

# Restart Alloy (after config changes)
sudo systemctl restart alloy

# View Alloy logs (last 50 lines)
sudo journalctl -u alloy -n 50 --no-pager

# View Alloy logs (follow in real-time)
sudo journalctl -u alloy -f

# Check local metrics collection
curl http://localhost:12345/metrics

# Test Grafana Cloud connectivity
curl -I https://prometheus-prod-13-prod-us-east-0.grafana.net
```

---

**Remember:** Most issues are related to service status, configuration errors, or network connectivity. Start with the basics before diving into complex troubleshooting.