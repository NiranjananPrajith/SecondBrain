# Incident Report: n8n Crash Loop Caused VPS CPU Spike

**Date:** 2026-04-03
**Status:** RESOLVED
**Server:** SRV816549 (Hostinger VPS)
**User:** redvelvetrenders / ubuntu

---

## Overview

The VPS experienced severe performance degradation. Monitoring tools identified a `node` process consuming 115%+ CPU. Attempts to terminate the process failed as it immediately respawned with a new PID.

---

## Root Cause

A Docker container named `n8n-data_n8n_1` was running an older n8n automation image and was caught in an **infinite crash loop**:

- Container logs reported: `Error: Command 'start' not found`
- Docker restart policy automatically restarted the container every time the Node process crashed
- Rapid-fire crashing/restarting created massive CPU overhead
- The process appeared "unkillable" because it kept respawning

---

## Timeline

- **Pre-incident:** VPS throttled for 4-5 hours prior
- **Incident:** 20-30 minute outage
- **Resolution:** Container stopped, Docker purged, services restored

---

## Actions Taken

1. **Investigation** — Ruled out PM2 and Systemd services. Identified process owner as `ubuntu` and source as Docker.

2. **Containment** — Executed `sudo docker stop` and `sudo docker rm` to halt the restart loop.

3. **Cleanup** — Ran `sudo docker system prune` — reclaimed 1.02GB of disk space.

4. **Decommissioning** — Docker and n8n completely purged:
   - `docker system prune`
   - Removed Docker binaries: docker, dockerd, docker-proxy, docker-init, docker-compose, containerd, containerd-shim, runc
   - Stopped lingering docker.socket and containerd.socket units

5. **Service Restoration** — PM2 services restored via standard command:
   ```bash
   PORT=[####] pm2 start npm --name "[name]" -- start
   ```

---

## Final Status

- CPU Usage: Returned to normal idle levels (0-5%)
- Memory: Reclaimed from Docker/Node buffers
- Persistence: All auto-restart managers for the offending process deleted

---

## Impact

- **Outage duration:** ~20-30 minutes
- **Throttling:** 4-5 hours prior to incident
- **Data loss:** None
- **User impact:** Moderate (service degraded/unavailable during outage)

---

## Action Items

- [ ] Consider setting up CPU monitoring alerts to detect spikes before they hit Hostinger limits
- [ ] If n8n is needed in future, ensure it runs without Docker restart policy or run it in PM2 instead
- [ ] Schedule weekly `docker system prune` if Docker is used again
- [ ] Review Hostinger monitoring settings for earlier alert thresholds

---

## Related Files

- Docker was installed via manual binaries (not apt/snap) — binary locations documented in incident notes
- PM2 standard for future Docker-less service management: `PORT=[####] pm2 start npm --name "[name]" -- start`
