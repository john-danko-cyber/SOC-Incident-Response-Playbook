# Ransomware Response Playbook

**Purpose:** Limit the spread and impact of ransomware, preserve recovery options, and restore operations from known-good backups.

**Phases:** Detection > Triage > Investigation > Containment > Eradication > Recovery > Lessons Learned

## 1. Detection
Common signals that should open this playbook:
- Mass file renames or new extensions on file shares
- Ransom notes appearing in folders
- Endpoint protection alerts on encryption behavior or shadow copy deletion
- Sudden spike in file write activity from one account or host

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
- Which hosts and shares are affected right now, and is it still spreading?
- Are backups reachable from the infected network?
- Which account is doing the encryption?

## 3. Investigation
1. Identify patient zero: earliest host showing encryption activity.
2. Identify the account performing the writes and where else it is logged in.
3. Find the initial access path (phishing, exposed RDP, vulnerable VPN, stolen credentials).
4. Check for data exfiltration before encryption (large outbound transfers, archive tools).
5. Record the ransom note text, extension, and any contact details for identification; do not contact the attacker without leadership and legal approval.
6. Verify integrity and isolation of backups.

## 4. Containment
1. Isolate affected hosts immediately; disconnect, do not power off (preserves memory evidence).
2. Disable the accounts used for encryption and reset privileged credentials.
3. Take backups offline or confirm they are immutable.
4. Block known IOCs and restrict SMB/RDP between segments.

## 5. Eradication
1. Rebuild affected hosts from known-good images rather than cleaning them.
2. Remove attacker persistence found anywhere in the environment (accounts, tasks, GPOs, remote tools).
3. Patch or close the initial access path.

## 6. Recovery
1. Restore data from verified clean backups, priority systems first.
2. Scan restored data before returning it to users.
3. Monitor closely for re-infection for at least 30 days.

## 7. Lessons Learned
- Recovery time versus targets: what slowed restoration?
- Were backups isolated enough?
- Update the business continuity plan and tabletop this scenario.

## Escalation Criteria
| Severity | Condition |
|---|---|
| High | Encryption on a single host, contained quickly |
| Critical | Any encryption on servers or shares, multiple hosts, data exfiltration, or backups affected. Notify leadership, legal, and consider law enforcement reporting |

## MITRE ATT&CK Mapping
| Technique ID | Name | Where it appears |
|---|---|---|
| T1486 | Data Encrypted for Impact | File encryption |
| T1490 | Inhibit System Recovery | Shadow copy deletion |
| T1021.001 | Remote Desktop Protocol | Spread / initial access |
| T1567 | Exfiltration Over Web Service | Data theft before encryption |
| T1078 | Valid Accounts | Use of stolen credentials |

---
*Original procedure written for this repository. See the README for reference frameworks.*
