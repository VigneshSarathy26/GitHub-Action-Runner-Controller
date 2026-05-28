# GitHub-Action-Runner-Controller

### How to Optimize the runner cost

- Optimize Workflow: Consolidate redundant job, use conditional triggers, and remove duplicate test runs.
- Use Ephemeral Runners: Runners are created for a single job and then destroyed,ensuring a clean state.
- Self-Host with Spot Instances: Using Spot Instances for self-hosted runners can reduce costs by up to 70%.
- Scale to Zero: Use auto-scaling(e.g: ARC on K8s) to scale the number of runners to zero when not in use.
- Do Not Use: Large instances or heavy CPU/GPU workloads on a single runner.
- Use Docker for Runners: Run multiple lightweight Docker containers per host instead of full Virtual Machines(VMs).
- Implement local caching: setup persisent caches(eg. apt cache, npm cache, maven cache,docker) shared across runners.
- Static path (For predictable costs): Run a fixed number of always-on self-hosted runners. Your monthly bill is flat and predictable but you risk jobs being queued if demand spikes.
- Maintenance: you are responsible for OS updates, security patches,disk space cleanup, and uptime
- Handle preemption Gracefully: If you spot instances,implement a retry strategy in your workflow(max-attempts: 2) and setup a termination notice handler. This ensures that if an instance is reclaimed,the job simply restarts on a new one without failing.
- Audit Workflow: Identify slow jobs(e.g: long end-to-end tests) consuming most minutes.
- Use GitHub Actions Usage report: via the API or billing dashboard,track which workflows consume the most minutes and storage.
- **_ Caching reduces time but increase storage costs if not managed (GB-Hours Billing) _**
- Use path filters: to skip runs when unrelated files changes(e.g, dont run backend test when only docs changed).
- **Cancel Redundant CI Runs: When commits are pushed in rapid succession,jobs triggered by earlier commits often keep running even though their results are obsolete. Use the concurrency key to automatically cancel stale jobs in favor of the newest one. This ensures that only the latest version is tested,saving time and resources.**
