# Incident Response Checklist

Use this checklist alongside any playbook. Tick items as you go and record times in UTC.

## Preparation
- [ ] Logging enabled and centralized in the SIEM (endpoint, network, identity, email, cloud)
- [ ] SIEM detection rules in place and tested for each playbook type
- [ ] EDR deployed on all endpoints with isolation capability
- [ ] Communication and escalation contacts documented and current
- [ ] Backups taken, isolated, and restore-tested
- [ ] Analyst access permissions granted in advance (isolate, purge, reset, block)
- [ ] Playbooks reviewed and current

## Detection
- [ ] Alert source and alert ID recorded
- [ ] Ticket opened with detection time
- [ ] Correct playbook selected

## Triage
- [ ] Host, user, IP, process, hash, severity, and time recorded
- [ ] Confirmed true positive vs false positive
- [ ] Severity assigned and escalated if criteria met

## Investigation
- [ ] Evidence collected and preserved before changes (logs, process list, connections, file hashes)
- [ ] Scope determined (other hosts, users, IPs affected)
- [ ] Initial access vector identified or marked unknown
- [ ] Timeline started

## Containment
- [ ] Affected hosts isolated or traffic blocked
- [ ] Compromised accounts disabled or sessions revoked
- [ ] Malicious processes stopped and persistence disabled
- [ ] Containment verified (no new activity)

## Eradication
- [ ] Malicious files, accounts, and persistence removed
- [ ] Root cause fixed (patch, config change, MFA)
- [ ] Credentials rotated

## Recovery
- [ ] Systems restored or rebuilt from known-good state
- [ ] Users notified and access restored
- [ ] Enhanced monitoring in place with an end date

## Lessons Learned
- [ ] Post-incident review held within 2 weeks
- [ ] Timeline and report finalized
- [ ] Detections, playbooks, and training updated
- [ ] IOCs shared with the team
