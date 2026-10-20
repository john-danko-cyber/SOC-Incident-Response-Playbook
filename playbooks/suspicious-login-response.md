# Suspicious Login Response Playbook

**Purpose:** Investigate sign-ins that do not match a user's normal behavior and decide quickly whether the account is compromised.

**Phases:** Preparation > Detection > Triage > Investigation > Containment > Eradication > Recovery > Lessons Learned

## 0. Preparation
Have these in place before an incident of this type:
- [ ] Identity provider sign-in logs (IP, location, device, user agent) sent to the SIEM
- [ ] SIEM rules for impossible travel, new countries, and MFA fatigue
- [ ] Permission to revoke sessions and reset MFA for any user
- [ ] Conditional access policies and number-matching MFA where possible
- [ ] An out-of-band way to contact users (phone or chat) to verify logins
- [ ] Known VPN and corporate egress IPs listed to reduce false positives

## 1. Detection
Common signals that should open this playbook:
- Impossible travel (two locations too far apart for the time between logins)
- Login from a new country, device, or anonymizing service
- Unexpected MFA push the user did not trigger
- Login outside normal hours to a sensitive system

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
- Does the user recognize the login? Contact them through a separate channel.
- Was MFA satisfied, and how?
- What did the session access after login?

## 3. Investigation
1. Pull sign-in logs for the user for the last 30 days to build a baseline.
2. Compare the suspicious login: IP, ASN, location, device, user agent, app.
3. Check for MFA fatigue patterns (many prompts before one approval).
4. Review activity during the session: file access, mailbox rules, admin changes, new MFA methods.
5. Check whether the same IP logged into other accounts.

## 4. Containment
1. Revoke all active sessions and refresh tokens.
2. Reset the password and re-register MFA if a new method was added.
3. Block the source IP if it is clearly malicious.
4. Temporarily restrict the account if it is privileged.

## 5. Eradication
1. Remove attacker-added MFA methods, app passwords, OAuth consents, and mailbox rules.
2. Reverse any unauthorized changes made in the session.

## 6. Recovery
1. Restore access for the user with fresh credentials.
2. Monitor the account for 14 days.
3. Confirm with the user that everything looks normal.

## 7. Lessons Learned
- Would conditional access or number-matching MFA have stopped this?
- Was the user able to report the unexpected prompt easily?
- Tune impossible-travel rules to reduce false positives from VPNs.

## Escalation Criteria
| Severity | Condition |
|---|---|
| Low | User confirms the login (travel, new device) |
| Medium | User does not recognize it, no access to sensitive data |
| High | Confirmed compromise with mailbox or file access |
| Critical | Privileged account compromised or changes to security settings |

## MITRE ATT&CK Mapping
| Technique ID | Name | Where it appears |
|---|---|---|
| T1078 | Valid Accounts | Use of stolen credentials |
| T1621 | MFA Request Generation | MFA fatigue |
| T1098.005 | Device Registration | New MFA method added |
| T1550.004 | Web Session Cookie | Token replay |

---
*Original procedure written for this repository. See the README for reference frameworks.*
