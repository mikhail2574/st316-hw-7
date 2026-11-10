# SDT316 — AWS High Availability & Load Balancing

## Task 4 — Written Answers

### 1. What did the load balancer do when one instance failed?

The Application Load Balancer marked `sdt316-web-2` unhealthy after repeated failed HTTP health checks and routed subsequent requests to the two healthy targets in other Availability Zones. With a 10-second health-check interval and an unhealthy threshold of two, detection takes multiple checks, so some requests could fail during the detection window; after the target was marked unhealthy, all 12 requests in our test succeeded.

### 2. What does the HTTP `/` health check prove, and what would it miss?

A successful HTTP `GET /` returning status `200` demonstrates that the instance is reachable from the load balancer and its web server can respond on port 80 for that path. It does not prove that every application function works: for example, a broken database connection could go undetected if the nginx welcome page still responds with HTTP `200`.

### 3. Why restrict instance HTTP access to the ALB's security group?

Allowing TCP port 80 only from `sdt316-alb-sg` prevents direct public HTTP access to the backend instances and keeps web traffic going through the intended load balancer entry point. This reduces the risk of bypassing ALB-based controls and supports the **Security** pillar of the AWS Well-Architected Framework through network segmentation and least-privilege access.

### 4. What happens if an entire Availability Zone fails?

If one Availability Zone fails, the ALB can continue sending requests to the two healthy instances in the remaining zones, although available capacity falls and performance may suffer under load. If all three instances were in the failed zone, the entire application backend would become unavailable. An **EC2 Auto Scaling Group** spanning multiple Availability Zones would help automatically launch replacement instances in available zones and restore capacity.

## Observed Evidence

- Before failure: all three targets Healthy, with HTTP responses from three distinct instance IDs.
- During failure: nginx stopped on `sdt316-web-2`; the target became Unhealthy, while 12 sampled requests after detection were served by only `web-1` and `web-3`.
- After recovery: nginx started again; `web-2` reappeared in the ALB response sample alongside the other two instances.
