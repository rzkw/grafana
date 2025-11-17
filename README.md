<p align="center">
    <img src="/screenshots/Linux node _ overview-1762941418040.png" alt="Dashboard Overview on Grafana Cloud" width="600">
    <br>
     <em>Main dashboard showing CPU, memory, and disk metrics</em>
</p>

# Overview
This repository documents a monitoring implementation for a bare-metal Ubuntu Linux server using Grafana Alloy to collect system metrics and send them to Grafana Cloud for visualization and analysis.

### Environment:
• Server OS: Ubuntu Server 25.10 LTS \
• Monitoring Agent: Grafana Alloy \
• Cloud Platform: Grafana Cloud \
• Deployment Type: Bare metal \
• Installation Method: Official script from Grafana Cloud

# What This Does

### Why Monitor Your Linux Server?
Monitoring helps you understand what's happening on your server in real-time. Think of it like a health
check-up for your computer - it tells you if things are running smoothly or if there are problems.

### What Metrics Are Collected?
This setup tracks the four essential health indicators of your Linux server:

1. CPU Usage - How hard your processor is working
    - Shows if your server is overloaded or running efficiently
    - Helps identify performance bottlenecks 

2. Memory (RAM) Usage - How much memory your applications are using
    - Prevents out-of-memory crashes
    - Helps with capacity planning

3. Disk Usage - How full your storage drives are
    - Prevents "disk full" errors
    - Tracks read/write performance

4. Network Activity - How much data is being sent and received
    - Monitors bandwidth usage
    - Detects unusual traffic patterns

### Simple explanation:

1. Your Ubuntu server generates performance data constantly
2. Grafana Alloy collects this data every few seconds
3. Alloy securely sends the data to Grafana Cloud over the internet
4. Grafana Cloud stores the data and creates visual dashboards

### Prerequisites
Before starting, you need:

- Grafana Cloud Account - Free tier available at [grafana.com](https://grafana.com/)
- A server running Linux with admin privileges
- ufw/firewall rule set to allow outbound HTTPS/port 443
- Some Linux knowledge

### Setup Guide

#### 1: Access Grafana Cloud Integration Wizard

1. Log into your Grafana Cloud account
2. Navigate to: Connections → Add new connection
3. Click on Linux Server under 'Most Popular'

#### 2: Install Grafana Alloy

1. Select your platform and architecture - I chose Debian on an Arm64
2. Click the 'Run Grafana Alloy' button to generate an API token
3. On your server: copy the given command to install and run Grafana Alloy as a systemd service. 
4. Use the default 'Simple set-up' under 'Make configuration selections'
5. Open the Alloy configuration file:

```
sudo nano /etc/alloy/config.alloy
```

and append the given code snippets. 

6. Save the config file, restart and enable Alloy:

```
sudo systemctl restart alloy.service
sudo systemctl enable --now alloy.service
sudo systemctl status alloy
```
You should see active (running) in green text.

7. Test your connection works by the 'Test connection' button


## Key Files/Directories

- README.md: You're reading it! Start here.
- config/config.alloy.example: Template configuration (credentials removed)
- screenshots/: Visual examples of dashboards and metrics
- docs/: Extended documentation for advanced topics
- scripts/: Helpful automation scripts


## Additional Resources

- [Official Grafana Alloy Documentation](https://grafana.com/docs/alloy/latest/)
- [Linux Node Integration Docs](https://grafana.com/docs/grafana-cloud/monitor-infrastructure/integrations/integration-reference/integration-linux-node/)
- [Grafana Cloud Free Tier](https://grafana.com/auth/sign-up/create-user?pg=hp&plcmt=cloud-promo&cta=create-free-account)


## Contributing
This is a personal/team documentation repository. If you'd like to suggest improvements:

1. Create a branch with your changes
2. Submit a pull request with a clear description
3. Tag relevant team members (or me!) for review


## License
MIT License - Free to use and modify

**Questions or Issues?**

- Check the Troubleshooting section first
- Review extended docs in the docs/folder
- For team members: Contact the infrastructure team

**Last Updated:** November 2025
