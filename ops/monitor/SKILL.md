---
name: monitor
description: |
  Service health monitoring, alerting patterns, and incident response.
  Check if services are up, diagnose issues, set up alerting rules.
  Use when: checking service health, something is down, setting up monitoring,
  investigating slow performance, or responding to an alert.
  Trigger words: is it up, healthcheck, monitoring, alert, downtime, slow,
  service down, 500 error, high latency, incident, on-call.
  NOT for: deploying code (use ops/deploy), code review (use code/review),
  or debugging application logic (use code/debug).
argument-hint: "[action: check|diagnose|setup] [service-url-or-name]"
---

# Monitor

Is it up? Is it healthy? Should someone know?

## Input

$ARGUMENTS — action and service to check.

## Quick Health Check

### HTTP Service
```bash
# Basic: is it responding?
curl -s -o /dev/null -w "HTTP %{http_code} in %{time_total}s\n" https://$URL

# Health endpoint (preferred)
curl -s https://$URL/health | python3 -m json.tool

# Full check: status + response time + SSL expiry
curl -s -o /dev/null -w "Status: %{http_code}\nTime: %{time_total}s\nSSL expires: %{ssl_verify_result}\n" https://$URL
```

### Docker Service
```bash
# Container status
docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}"

# Resource usage
docker stats --no-stream --format "table {{.Name}}\t{{.CPUPerc}}\t{{.MemUsage}}"

# Recent logs (last 100 lines, errors only)
docker logs --tail 100 $CONTAINER 2>&1 | grep -iE "error|fatal|panic|exception"
```

### systemd Service
```bash
systemctl is-active $SERVICE
journalctl -u $SERVICE --since "1 hour ago" --no-pager -n 50
```

## Diagnose

When something is wrong, check in this order:

```
DIAGNOSIS FLOW (top-down, stop when you find it)

1. DNS resolving?
   → dig $DOMAIN +short
   → nslookup $DOMAIN

2. Port open?
   → nc -zv $HOST $PORT -w 5
   → curl -v --connect-timeout 5 https://$URL 2>&1 | head -20

3. SSL valid?
   → echo | openssl s_client -connect $HOST:443 2>/dev/null | openssl x509 -dates -noout

4. Service running?
   → docker ps | grep $SERVICE
   → systemctl is-active $SERVICE

5. Service healthy (internal)?
   → docker logs --tail 50 $CONTAINER
   → journalctl -u $SERVICE -n 50

6. Database connected?
   → Check connection pool metrics
   → Test query: SELECT 1

7. External dependencies up?
   → curl each external API endpoint
   → Check status pages (statuspage.io of your deps)

8. Resources exhausted?
   → df -h (disk)
   → free -m (memory)
   → top -bn1 | head -20 (CPU)
   → docker stats --no-stream (per container)
```

## Alerting Patterns

### What to Alert On (and what not to)

| Alert | Severity | When |
|-------|----------|------|
| Service down (health check fails 3x) | CRITICAL | Immediately |
| Error rate > 5% of requests | HIGH | Within 5 min |
| Response time > 2x baseline | MEDIUM | Within 15 min |
| Disk > 85% | MEDIUM | Within 1 hour |
| Memory > 90% | HIGH | Within 5 min |
| SSL cert expires < 14 days | MEDIUM | Daily check |
| Database connections > 80% of pool | HIGH | Within 5 min |
| CPU sustained > 90% for 10 min | MEDIUM | Within 15 min |

### What NOT to Alert On
- Single failed request (noise)
- Normal traffic spikes (expected)
- Warning-level log messages (too noisy)
- Successful deploys (info, not alert)
- Metrics within normal variance (±20%)

### Alert Fatigue is Real
If you get more than 5 alerts per week that don't require action, fix your thresholds. Every ignored alert makes the next real alert more likely to be ignored.

## Incident Response

### When Something is Down

```
1. ACKNOWLEDGE (1 min)
   - Confirm the issue is real (not a false alert)
   - Note the start time

2. ASSESS (5 min)
   - Who is impacted? (all users / some / internal only)
   - What's broken? (complete outage / degraded / specific feature)
   - Quick diagnosis: run the diagnosis flow above

3. MITIGATE (15 min)
   - Can you rollback? → Do it
   - Can you restart? → Do it
   - Can you redirect traffic? → Do it
   - Fix it NOW, understand it LATER

4. COMMUNICATE (ongoing)
   - Status page updated (if you have one)
   - Team notified
   - Customers notified if impact > 15 min

5. RESOLVE
   - Confirm service is healthy
   - Monitor for 30 min after fix
   - Note the end time

6. POST-MORTEM (within 48 hours)
   - Timeline of events
   - Root cause
   - What we'll do to prevent recurrence
   - No blame. Just facts and fixes.
```

## Monitoring Checklist

```
MINIMUM VIABLE MONITORING
[ ] Health endpoint exists (returns 200 + basic status)
[ ] Uptime monitoring (external check every 1-5 min)
[ ] Error logging (errors captured with stack traces)
[ ] Basic alerting (email/Slack when down)

PRODUCTION-GRADE MONITORING
[ ] All of above, plus:
[ ] Response time tracking (P50, P95, P99)
[ ] Error rate tracking (% of requests)
[ ] Resource metrics (CPU, memory, disk, connections)
[ ] SSL certificate expiry monitoring
[ ] Dependency health checks
[ ] Log aggregation (searchable)
[ ] Dashboards (at-a-glance status)
[ ] On-call rotation (if team)
```

## Related Skills

- `ops/deploy/` — Deploy fixes when something breaks
- `code/debug/` — Debug the application-level issue
- `code/ship/` — Ship the hotfix
