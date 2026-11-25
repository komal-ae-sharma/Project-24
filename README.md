### Incident Notes: Project 24 - Manual Chaos Test

- **Date & Time:** November 26, 2026
- **Target Infrastructure:** Application Load Balancer (`My-App-ALB`) with 2 EC2 instances (`WebServer-1` - `i-09476d5f4fdc645ed`, `WebServer-2` - `i-0277d96568d390b17`)
- **Simulated Failure (Root Cause):** Manually stopped the Apache (`httpd`) web service on `WebServer-1` using `sudo systemctl stop httpd`.
- **System Impact & Failover Observation:**
  - ALB health checks detected the failure on port 80 and marked `WebServer-1` as **Unhealthy** in the target group (`My-App-TG`).
  - Application availability remained uninterrupted; ALB automatically routed 100% of incoming traffic to `WebServer-2` ("Response from WebServer 2").
- **Recovery Action:** Restarted the Apache web service on `WebServer-1` using `sudo systemctl start httpd` and verified active status (`systemctl status httpd`).
- **Post-Recovery Verification:** Within ~30–60 seconds, `WebServer-1` passed ALB health checks and returned to **Healthy** status. Load balancing resumed normally across both `WebServer-1` and `WebServer-2`.
