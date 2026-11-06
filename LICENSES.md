# Component licences (defensive stack)

Checked 2026-11-02 with read-only `gh api repos/<repo>/license` and by reading LICENSE files. Last commit = newest commit on the default branch (gh api commits/<branch>).

| Tool | Repo | Licence (SPDX per GitHub) | Licence file | Last commit |
|---|---|---|---|---|
| restic | restic/restic | BSD-2-Clause | https://github.com/restic/restic/blob/master/LICENSE | 2026-10-18 |
| WinGet CLI | microsoft/winget-cli | MIT | https://github.com/microsoft/winget-cli/blob/master/LICENSE | 2026-10-30 |
| Keycloak | keycloak/keycloak | Apache-2.0 | https://github.com/keycloak/keycloak/blob/main/LICENSE.txt | 2026-11-02 |
| Wazuh | wazuh/wazuh | NOASSERTION (mixed, see below) | https://github.com/wazuh/wazuh/blob/main/LICENSE | 2026-11-01 |
| OPNsense | OPNsense/core | BSD-2-Clause | https://github.com/opnsense/core/blob/master/LICENSE | 2026-11-02 |
| CrowdSec | crowdsecurity/crowdsec | MIT | https://github.com/crowdsecurity/crowdsec/blob/master/LICENSE | 2026-10-30 |
| DFIR-IRIS | dfir-iris/iris-web | LGPL-3.0 | https://github.com/dfir-iris/iris-web/blob/master/LICENSE.txt | 2026-09-23 |

Wazuh, from its LICENSE file: the engine (src/engine/) is AGPL-3.0-or-later; a listed set of shared libraries is "GPL-2.0-only OR AGPL-3.0-or-later"; "Everything else is licensed under the GNU General Public License (version 2)".

Considered and left out
- Velociraptor (Velocidex/velociraptor): GitHub shows NOASSERTION and we did not read its licence; skipped.
- TruffleHog (AGPL-3.0): secret scanner, strong-copyleft; skipped to keep the list simple.
- Authentik (goauthentik/authentik): GitHub shows NOASSERTION (core plus a separately licensed enterprise directory is our understanding, NOT verified); skipped in favour of Keycloak (Apache-2.0).
- Borg (borgbackup/borg): BSD-style LICENSE (header read) but GitHub shows NOASSERTION; restic chosen instead (Windows support is stated in its README).
- osquery: GitHub shows NOASSERTION; its LICENSE says you may choose Apache-2.0 or GPL-2.0. Not needed with Wazuh.
- Offensive or scanning tools: excluded by scope.
