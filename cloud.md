Cloud Run

1. services: Runs all the time and scales automatically based on the traffic.

1.1 Deployment:

1. From Image: Build and push the image to a container registry and then deploy it to Cloud Run.
2. From Source: Continuously deploy changes to the service using dockerfile or by inbuilt google cloud or githubaction or other cicd pipeline.

1.2 CPU allocation:

only for processing: cheap
always allocated: expensive

1.3 Autoscaling:
min - 0 (no instances - so delayed to start when requested)
max - 100

1.4 Ingress:
internal - only accessible within the VPC (for internal services)
external - accessible from the internet

1.5 Memory allocation:
128 MiB to Max 32GiB

1.6 CPU allocation:
A single instance can have multiple cpus running in parallel to handle the traffic.
vCPU to Max 8 vCPUs

1.7 Request timeout:
default - 5 minutes

1.8 Request concurrency:
default - 80
max - 1000

1.9 Execution environment:
default - Linux
first generation:
Second generation:

other
environment variables
secrets
health checks
cloud sql connections

max - 60 minutes

1.10 SLO service level objective
Define the rule swhen the we should get notification if the metrix coross the threshold
by default its just notification but we can configure to fire the mail

Logs Tab: system logs from cloud run

2. jobs: Runs once or on a schedule.

Metrix of cloudrun is shown ih the matrix tab in google cloud run itself
