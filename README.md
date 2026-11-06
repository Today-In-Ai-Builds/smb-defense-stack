# Small-Business Defensive Stack
Video: https://youtu.be/nML6u5HcB64 (Today in AI). Published by Today in AI; see LICENSES.md for every upstream project's own licence.

A curated, defence-only set of well-maintained open-source tools plus a setup order for a business of roughly 5-50 people.
No offensive tools, no exploit code, no network scanning. Nothing here was installed or run on the machine that wrote it.
Not security advice for your business: review the setup with whoever runs your IT.

Triggering story (2026-10-29): the Binance hot-wallet theft, which Binance linked to a compromised DevOps engineer's laptop.
The lesson we draw is our OPINION: one compromised endpoint should not be enough to reach the keys to the business.

## The stack (order = setup order)

| # | Layer | Tool | Repo | Licence | Last commit | Protects against |
|---|---|---|---|---|---|---|
| 1 | Backups (offline/immutable copy) | restic | restic/restic | BSD-2-Clause | 2026-10-18 (latest release v0.19.1, 2026-07-05) | Ransomware and accidental deletion: encrypted, verifiable, restorable backups |
| 2 | Patching (apps on Windows) | WinGet (Windows Package Manager) | microsoft/winget-cli | MIT | 2026-10-30 | Known-vulnerability exploitation in third-party apps. OS patching: use built-in Windows Update / your Linux distro's updater (no extra tool) |
| 3 | MFA / single sign-on | Keycloak | keycloak/keycloak | Apache-2.0 | 2026-11-02 (release 26.8.1, 2026-10-13) | Stolen or guessed passwords |
| 4 | Endpoint and log monitoring | Wazuh | wazuh/wazuh | Mixed: GPL-2.0 for most, AGPL-3.0 for the engine, per its LICENSE (see flags) | 2026-11-01 (release v4.14.9, 2026-10-15) | Spotting a compromised endpoint: log collection, file integrity, alerts |
| 5 | Network segmentation / firewall | OPNsense | OPNsense/core | BSD-2-Clause | 2026-11-02 | Lateral movement: separate VLANs for office, servers, guests, backups |
| 6 | Optional: IP blocklist at the edge | CrowdSec | crowdsecurity/crowdsec | MIT | 2026-10-30 (release v1.8.2, 2026-10-26) | Known-bad IPs hitting exposed services |
| 7 | Incident-response case tracking | DFIR-IRIS | dfir-iris/iris-web | LGPL-3.0 | 2026-09-23 (release v2.4.29, 2026-08-10) | Chaos during an incident: tasks, evidence notes, timeline in one place |

Checked 2026-11-02 with read-only `gh api`. Licence file links and notes are in LICENSES.md.

## Setup order for a small business (plan, not yet executed)

1. Inventory: list every laptop, server, cloud account and who admins it. (Our own template: `templates/inventory.csv` columns.)
2. Backups first (restic): follow the 3-2-1 idea (3 copies, 2 media, 1 offsite or offline). Encrypt with a key stored separately. TEST A RESTORE and log the date.
3. Patching: turn on automatic OS updates; use WinGet to list and upgrade app versions on a schedule. Keep a "last patched" column in the inventory.
4. MFA: put admin and email accounts behind MFA first. Keycloak is for businesses that run their own apps; if you only use SaaS, use the MFA built into your identity provider and skip Keycloak.
5. Segmentation: put servers, office PCs, guest Wi-Fi, and the backup target on separate networks (OPNsense on a small box or VM). Default-deny between them. Open only what is needed.
6. Monitoring: Wazuh manager on its own host; install agents on endpoints one team at a time. Start with default rules, tune after a week.
7. Optional edge blocklist: CrowdSec on any internet-facing Linux host.
8. Incident basics: write a one-page runbook (who decides, who calls the insurer/lawyer, how to isolate a machine, where backups are). Rehearse once a quarter. Keep IRIS ready for tracking.
9. Review quarterly: restore test, user list, patch levels, alert noise.

## Templates (our own, to write before ship-check)

`templates/inventory.csv`, `templates/ir-runbook.md`, `templates/restore-test-log.md`. Intentionally not written yet; they are small and
only worth writing after Kevin approves the direction.

## Flags (read before recommending)

- Wazuh licensing is mixed (GPL-2.0 plus AGPL-3.0 for the engine). Fine to run in-house; talk to counsel before bundling or hosting it for others.
- CrowdSec's blocklist works by sharing signals with a community service. Read what is shared before using it on a business network.
- This is a starting list, not a security programme. Say so on screen.
- We did not install or test any of these tools. All claims come from each repo's own README/description and the table above.


## What we actually ran (2026-11-02)

restic 0.19.1 backup + incremental + restore + check in a throwaway Docker container on a scratch folder (`lab/restic_test.sh`, output in `lab/restic_test_output.txt`), and a read-only `winget upgrade` listing (`lab/winget_upgrade_listing.txt`: 20 upgrades available on the test PC). Nothing else was run. 3-2-1, Keycloak MFA types, OPNsense VLANs and Wazuh FIM are now checked against primary docs (sources.md T9-T10c).
