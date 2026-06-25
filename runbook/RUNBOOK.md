# RUNBOOK: example-service

Operational runbook for the fictional `example-service`. It is a
template: replace the commands, URLs, and names with your own. Keep this
file short and current — it is what an on-call engineer follows under
pressure.

## Service summary

- **What it does:** serves the `example-service` HTTP API and processes
  its background job queue.
- **Owners:** platform-team (`#example-service` chat channel).
- **Runtime:** containerized service behind a load balancer, plus a
  worker pool draining the job queue.
- **Dashboards:** `https://dashboards.example.internal/example-service`
- **Alerts:** paged via the on-call rotation on error-rate and
  latency-SLO breaches.

## Health checks

Before and after any operation, confirm the service is healthy:

```sh
# Liveness and readiness
curl -fsS https://example-service.internal/healthz
curl -fsS https://example-service.internal/readyz

# Queue depth should be draining, not growing
example-cli queue depth
```

A healthy service returns `200` from both endpoints and shows a stable
or falling queue depth.

## Deploy

1. Confirm CI is green on the commit you intend to ship.
2. Note the currently deployed version so you can roll back to it:
   ```sh
   example-cli releases current
   ```
3. Deploy the new version:
   ```sh
   example-cli deploy --version <git-sha>
   ```
4. Watch the rollout until all instances report the new version:
   ```sh
   example-cli rollout status
   ```
5. Run the health checks above. Confirm error rate and latency on the
   dashboard return to baseline within five minutes.
6. If anything looks wrong, go to **Rollback**.

## Rollback

Roll back when a deploy raises error rate or latency past the SLO, or
when a regression is reported.

1. Identify the last known-good version (the one you noted at deploy
   time, or):
   ```sh
   example-cli releases history --limit 5
   ```
2. Redeploy it:
   ```sh
   example-cli deploy --version <last-good-sha>
   ```
3. Watch the rollout and run the health checks above.
4. Confirm error rate and latency return to baseline.
5. Open an incident note describing what was rolled back and why.

Configuration changes roll back the same way: reapply the prior value
from the config history rather than reconstructing it by hand.

## Incident response

When an alert fires or a user reports an outage:

1. **Acknowledge** the page so the rotation knows it is being handled.
2. **Assess.** Check the dashboard and health checks. Classify severity
   by user impact (full outage, degraded, or single-feature).
3. **Communicate.** Post a short status in `#example-service`: what is
   affected, when it started, and who is on it. Update on a regular
   cadence until resolved.
4. **Mitigate first.** Prefer the fastest safe path back to health:
   - Recent deploy suspected → **Rollback**.
   - Bad config change → reapply the prior config value.
   - Overload → scale out the worker pool:
     ```sh
     example-cli scale workers --replicas <n>
     ```
   - Dependency down → enable the documented degraded mode if available.
5. **Verify** recovery with the health checks and dashboard.
6. **Close out.** Mark the incident resolved, post a final status, and
   file a follow-up for a blameless postmortem.

## Escalation

- First responder: on-call engineer for `example-service`.
- Secondary: platform-team lead.
- If a downstream dependency is the root cause, page that service's
  on-call rotation and link both incidents.

## After an incident

- Write a blameless postmortem within two business days.
- File follow-up work for any manual step that should be automated.
- If the incident exposed a missing or wrong step here, update this
  runbook in the same change.
