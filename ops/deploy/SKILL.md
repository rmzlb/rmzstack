---
name: deploy
description: |
  Generic deployment checklist and workflow. Pre-deploy verification, deploy execution,
  post-deploy validation. Works with any hosting (Docker, VPS, Vercel, AWS, Railway).
  Use when: deploying to staging/production, preparing a release, verifying a deploy,
  or rolling back a broken deploy.
  Trigger words: deploy, release, push to prod, go live, rollback, staging,
  production, ship to prod, is it deployed.
  NOT for: writing code (use code skills), CI/CD pipeline setup (infra work),
  or monitoring after deploy (use ops/monitor).
argument-hint: "[environment: staging|production] [service-name]"
---

# Deploy

The checklist between "code is ready" and "users see it."

## Input

$ARGUMENTS — environment and/or service name.

## Pre-Deploy (Before You Touch the Button)

```
PRE-DEPLOY GATE — ALL must pass
[ ] Code reviewed and approved
[ ] All tests pass on the branch being deployed
[ ] No pending migrations that haven't been tested
[ ] Environment variables set for target environment
[ ] Secrets rotated if needed (not expired)
[ ] Build succeeds locally / in CI
[ ] Changelog / release notes written
[ ] Team notified (if team project)
```

### Database Migrations
```
[ ] Migration tested on staging first (never prod-first)
[ ] Migration is reversible (has a down/rollback)
[ ] Migration doesn't lock tables for long (< 5 seconds for large tables)
[ ] Data backfill separated from schema change
[ ] Backward compatible (old code works with new schema during rollout)
```

### Environment Check
```bash
# Verify you're deploying to the RIGHT environment
echo "Deploying to: $ENVIRONMENT"
echo "Service: $SERVICE"
echo "Branch: $(git branch --show-current)"
echo "Last commit: $(git log --oneline -1)"
echo ""
echo "IS THIS CORRECT? Confirm before proceeding."
```

## Deploy Execution

### Docker-Based
```bash
# Build
docker build -t $SERVICE:$VERSION .

# Tag
docker tag $SERVICE:$VERSION $REGISTRY/$SERVICE:$VERSION
docker tag $SERVICE:$VERSION $REGISTRY/$SERVICE:latest

# Push
docker push $REGISTRY/$SERVICE:$VERSION
docker push $REGISTRY/$SERVICE:latest

# Deploy (varies by orchestrator)
# Docker Compose: docker compose up -d
# Docker Swarm: docker service update --image $IMAGE $SERVICE
# Kubernetes: kubectl set image deployment/$SERVICE $CONTAINER=$IMAGE
```

### Git-Based (Vercel, Railway, Dokploy, Heroku)
```bash
git push origin main  # or deploy branch
# Platform auto-builds and deploys
```

### Direct Server
```bash
ssh $SERVER "cd /app && git pull && npm run build && pm2 restart all"
```

## Post-Deploy Validation

Run IMMEDIATELY after deploy. Not tomorrow. Not "when I have time."

```
POST-DEPLOY CHECKLIST (within 5 minutes)
[ ] Service is responding (HTTP 200 on health endpoint)
[ ] Homepage/main page loads correctly
[ ] Login/auth works
[ ] Core user flow works (the ONE thing your app does)
[ ] No new errors in logs (check last 5 minutes)
[ ] Response times normal (not 2x slower than pre-deploy)
[ ] Database connections healthy
[ ] External integrations working (APIs, webhooks, email)
```

### Quick Smoke Test Script
```bash
# Health check
curl -s -o /dev/null -w "%{http_code}" https://$DOMAIN/health

# Response time
curl -s -o /dev/null -w "%{time_total}" https://$DOMAIN/

# Check for errors in last 5 min
# Docker: docker logs --since 5m $CONTAINER 2>&1 | grep -i error
# systemd: journalctl -u $SERVICE --since "5 min ago" | grep -i error
```

## Rollback

If post-deploy validation fails:

```
ROLLBACK DECISION TREE
├── Users are impacted NOW?
│   └── YES → Rollback immediately, investigate later
├── New errors in logs but users OK?
│   └── Monitor for 15 min, rollback if worsening
├── Performance degraded but functional?
│   └── Monitor for 30 min, rollback if not improving
└── Minor issue, no user impact?
    └── Fix forward (hotfix), don't rollback
```

### How to Rollback
```bash
# Docker
docker service update --rollback $SERVICE  # Swarm
kubectl rollout undo deployment/$SERVICE   # K8s

# Git-based platform
git revert HEAD && git push  # Revert the deploy commit

# Direct server
ssh $SERVER "cd /app && git checkout $PREVIOUS_COMMIT && npm run build && pm2 restart all"
```

## Deploy Log

After every deploy, log:
```yaml
deploy:
  date: "YYYY-MM-DD HH:MM UTC"
  service: "[service name]"
  environment: "[staging/production]"
  version: "[commit hash or tag]"
  deployer: "[who triggered it]"
  status: "[success/rolled-back/partial]"
  duration: "[build + deploy time]"
  issues: "[any problems encountered]"
  notes: "[anything notable]"
```

## Related Skills

- `code/review/` — Review code before deploying
- `code/ship/` — Prepare the PR/commit for deploy
- `ops/monitor/` — Monitor after deploy
