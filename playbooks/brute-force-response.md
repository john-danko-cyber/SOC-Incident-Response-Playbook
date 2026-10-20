# Brute-Force Response Playbook

**Purpose:** Respond to password guessing, password spraying, and credential-stuffing attempts against any login surface.

**Phases:** Preparation > Detection > Triage > Investigation > Containment > Eradication > Recovery > Lessons Learned

## 0. Preparation
Have these in place before an incident of this type:
- [ ] Authentication logs from VPN, RDP, SSH, web apps, and email centralized in the SIEM
- [ ] SIEM rules for failure-count thresholds per account and per source IP
- [ ] Account lockout and rate-limiting policies documented and enforced
- [ ] MFA enabled on all internet-facing services
- [ ] Permission to block IPs at the firewall or WAF without waiting on a change window
- [ ] Inventory of internet-exposed services

## 1. Detection
Common signals that should open this playbook:
- Many failed logins for one account in a short window
- Failed logins for many accounts from one source IP (spraying)
- Account lockout alerts spiking
- Successful login immediately after a run of failures

## 2. Triage
## Triage Fields
Record these on the ticket before deeper work:

| Field | What to capture |
|---|---|
| Host | Hostname, asset owner, OS, business criticality |
| User | Account name, role, privilege level, recent activity |
| IP | Source/destination IPs, ports, geolocation, reputation |
| Process | Process name, path, parent process, command line |
| Hash | SHA256 of any file involved |
| Severity | Low / Medium / High / Critical (see Escalation Criteria) |
| Time | First seen, last seen, detection time, all in UTC with local equivalent |

Additional triage questions:
- Was any attempt successful?
- Is the target an internet-facing service (VPN, RDP, SSH, web app, email)?
- Is the source one IP, a range, or a distributed set?

## 3. Investigation
1. Pull authentication logs for the window: account, source IP, result, protocol.
2. Count failures per account and per source IP to tell guessing from spraying.
3. Check the source IPs against reputation feeds and whether they belong to hosting providers or anonymizers.
4. Look for any success from the same sources and review what that session did.
5. Confirm whether targeted accounts are real, disabled, or service accounts.
6. Check whether the exposed service should be internet-facing at all.

## 4. Containment
1. Block the source IPs or ranges at the firewall or WAF.
2. Lock or force a password reset on any account with a successful login from an attacker source; revoke sessions.
3. Enable or tighten rate limiting and lockout thresholds on the targeted service.
4. If RDP/SSH is directly exposed, restrict it to VPN or allowlisted IPs.

## 5. Eradication
1. Remove any persistence created by a successful session (new users, keys, tasks).
2. Disable unused or default accounts that were targeted.
3. Enforce MFA on the targeted service.

## 6. Recovery
1. Lift temporary blocks only after the campaign stops.
2. Monitor the targeted accounts and service for 7-14 days.
3. Confirm legitimate users regained access.

## 7. Lessons Learned
- Was the exposed service necessary?
- Were lockout and alert thresholds right?
- Add the attacking ranges and patterns to detections.

## Escalation Criteria
| Severity | Condition |
|---|---|
| Low | Failed attempts only, blocked by existing controls |
| Medium | Sustained campaign against an internet-facing service, no success |
| High | One successful login from an attacker source |
| Critical | Admin/service account compromised or lateral movement observed |

## MITRE ATT&CK Mapping
| Technique ID | Name | Where it appears |
|---|---|---|
| T1110.001 | Password Guessing | Many tries on one account |
| T1110.003 | Password Spraying | One password, many accounts |
| T1110.004 | Credential Stuffing | Leaked credential pairs |
| T1133 | External Remote Services | VPN/RDP/SSH targets |
| T1078 | Valid Accounts | Successful logins |

---
*Original procedure written for this repository. See the README for reference frameworks.*
