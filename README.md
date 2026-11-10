# SDT316 — AWS High Availability & Load Balancing (Homework 7)

This repository folder records AWS Academy Modules 8–10 and the EC2/ALB failover experiment performed in **eu-central-1 (Frankfurt)** on November 10, 2026.

## Architecture

- Default VPC in Frankfurt
- Three Amazon Linux 2023 EC2 instances (`sdt316-web-1`, `sdt316-web-2`, `sdt316-web-3`) in Availability Zones `1a`, `1b`, and `1c`
- `sdt316-web-sg`: port 80 from the ALB security group, port 22 from a designated client IP
- Application Load Balancer `sdt316-alb`, listener `HTTP:80`, target group `sdt316-tg`
- Health checks on `GET /`; nginx index page displays the EC2 instance ID and Availability Zone

## Evidence

| Screenshot | What it shows |
|---|---|
| `screenshots/01-academy-modules-8-10.png` | Knowledge Checks: Modules 8, 9, 10 (90, 90, 100) |
| `screenshots/02-web-security-group.png` | Backend SG: HTTP sourced from ALB SG, SSH from My IP |
| `screenshots/03-three-ec2-instances-in-azs.png` | Three study instances running in distinct AZs (also shows earlier Homework 6 EC2) |
| `screenshots/04-ssh-local-curl-all-instances.png` | SSH and local nginx responses for all three instances |
| `screenshots/05-target-group-3-healthy.png` | Three healthy targets before failure |
| `screenshots/06-alb-load-distribution.png` | ALB sends requests to all three instance IDs |
| `screenshots/07-web-2-unhealthy.png` | web-2 Unhealthy while the other two remain Healthy |
| `screenshots/08-failure-recovery-curl.png` | After failure, requests reach only two instances; after restart, all three return |

Written explanations: [`answers.md`](answers.md).

**Evidence scope:** The screenshots show the Module 8–10 knowledge-check grades, not a dedicated all-modules-completed dashboard. They also show recovery through HTTP traffic; there is no separate screenshot of the Targets tab after recovery.

**Privacy:** AWS Account ID / owner ID and optional personal IP fields have been concealed in console screenshots; no credentials, private keys or authentication codes are included.

**Cost reminder:** The ALB and EC2 instances can incur ongoing charges. Remove the ALB and terminate unnecessary EC2 instances after collecting evidence. Deleting the ALB does not automatically terminate its instances.
