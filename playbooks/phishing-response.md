# Phishing Response Playbook

**Purpose:** Handle reported or detected phishing emails, from a single suspicious message to a credential-harvesting campaign.

**Phases:** Preparation > Detection > Triage > Investigation > Containment > Eradication > Recovery > Lessons Learned

## 0. Preparation
Have these in place before an incident of this type:
- [ ] Mail gateway logging retained for at least 90 days, including headers and URL clicks
- [ ] A user report button wired to a SOC mailbox or ticket queue
- [ ] Ability to search and purge messages across all mailboxes, with the permission granted in advance
- [ ] SPF, DKIM, and DMARC configured for your own domains
- [ ] SIEM rules for lookalike domains and for sign-ins shortly after a URL click
- [ ] Contact list for the email admin and for user communications

## 1. Detection
Common signals that should open this playbook:
- User reports a suspicious email through the report button or help desk
- Mail gateway flags a message for spoofed sender, lookalike domain, or malicious attachment
- Several users receive the same message within a short window
- A login from an unusual location follows shortly after a user clicked a link

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
- Did anyone click the link, open the attachment, or enter credentials?
- How many mailboxes received the same message?
- Is the sender external, a lookalike domain, or a compromised partner account?

## 3. Investigation
1. Collect the original message with full headers (do not forward as a screenshot).
2. Review headers: Return-Path, Reply-To, Received chain, SPF/DKIM/DMARC results.
3. Extract URLs, domains, and attachments. Hash attachments (SHA256) and check reputation services without opening them on a workstation.
4. Search mail logs for every recipient of the same sender, subject, or URL.
5. Check proxy/DNS logs for which hosts visited the URLs and when.
6. For users who entered credentials, review sign-in logs for new devices, locations, or MFA prompts after the click time.
7. Check for new mailbox forwarding rules or inbox rules created by the affected users.

## 4. Containment
1. Purge the message from all mailboxes.
2. Block the sender address/domain and malicious URLs at the mail gateway, proxy, and DNS filter.
3. Reset passwords and revoke active sessions for users who submitted credentials.
4. Isolate any host where an attachment was executed and hand off to the Malware Response playbook.

## 5. Eradication
1. Remove malicious inbox/forwarding rules.
2. Remove any OAuth app consents granted through the phishing page.
3. Confirm no payload persists on hosts that opened attachments.

## 6. Recovery
1. Restore normal mail flow and remove temporary blocks that are too broad.
2. Monitor affected accounts for 14 days for anomalous sign-ins.
3. Notify affected users with what happened and what to watch for.

## 7. Lessons Learned
- Why did the message pass filtering? Tune rules or add the lookalike domain pattern.
- Was the report fast? Track time from delivery to report and to purge.
- Feed the indicators into the blocklist and awareness training examples.

## Escalation Criteria
| Severity | Condition |
|---|---|
| Low | Reported email, no clicks, no credential entry |
| Medium | Clicks recorded, no credential entry or execution |
| High | Credentials submitted or attachment executed on one host |
| Critical | Executive/admin account compromised, multiple hosts executed payload, or data access confirmed |

## MITRE ATT&CK Mapping
| Technique ID | Name | Where it appears |
|---|---|---|
| T1566.001 | Spearphishing Attachment | Malicious attachment delivery |
| T1566.002 | Spearphishing Link | Credential-harvesting URL |
| T1204 | User Execution | User opens file or link |
| T1114.003 | Email Forwarding Rule | Post-compromise mailbox rules |
| T1078 | Valid Accounts | Use of harvested credentials |

---
*Original procedure written for this repository. See the README for reference frameworks.*
