# Dashboard Files

This folder contains the Linux Node monitoring dashboard in multiple formats.

## Files

### linux-node.json
The complete dashboard definition that can be imported into any Grafana instance.

**Contents:**
- All panel configurations
- Layout and styling
- Variable definitions

**How to Import:**
1. Log into your Grafana instance
2. Go to Dashboards → Import
3. Upload this JSON file
4. Select your Prometheus data source
5. Click Import

### linux-node-overview.pdf
A visual export of the dashboard showing actual metrics from the monitoring period.

**Generated:** November 17, 2025  
**Data Range:** 7 day period  
**Shows:**
- System overview (uptime, specs, OS info)
- CPU usage and load averages
- Memory utilization breakdown
- Disk I/O and space usage
- Network traffic and errors

This PDF demonstrates what the dashboard looks like with real production data.


## Additional Resources

- [Grafana Cloud: External snapshot](https://rzkw.grafana.net/dashboard/snapshot/haCjimhM8nujLNgXjHtgPZSWHQ9tWGZ7)

- [Grafana Labs: Share Dashboards and Panels](https://grafana.com/docs/grafana/latest/dashboards/share-dashboards-panels/)

- [Grafana - Externally shared dashboards](https://grafana.com/docs/grafana/latest/dashboards/share-dashboards-panels/shared-dashboards/#important-notes-about-sharing-your-dashboard-externally)