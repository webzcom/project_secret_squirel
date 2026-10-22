# PHP2Shell (CVE-2026-109618 / CVE-2026-109619) Mitigation Checklist

As of Oct 21, 2026

## At a glance

Any server running PHP below 8.5.12, 8.4.27, 8.3.36 or 8.2.35 that serves web requests needs attention now. Both CVEs (nicknamed "PHP2Shell") are unauthenticated remote code execution in the PHP engine itself, rated CVSS v4.0 9.2, fixed 21 Oct 2026. A public proof-of-concept has been claimed; no in-the-wild exploitation was reported as of 21 Oct.

| CVE | Advisory | Flaw | What goes wrong |
| --- | --- | --- | --- |
| CVE-2026-109618 | [GHSA-w8hw-w9fx-j79w](https://github.com/php/php-src/security/advisories/GHSA-w8hw-w9fx-j79w) | RCE via out-of-memory (OOM) corruption | An OOM during request handling exits via longjmp() while request memory is half-initialized; request shutdown then trusts that invalid state. Multipart body parsing can trigger OOM within normal limits. |
| CVE-2026-109619 | [GHSA-6wmf-4rv5-p3rp](https://github.com/php/php-src/security/advisories/GHSA-6wmf-4rv5-p3rp) | RCE via stale global state (use-after-free) | A fatal error or OOM leaves request-owned global state pointing at freed memory; the next request on the same worker uses it before initialization resets it. |

Only a PHP update fully fixes either flaw. Config changes in the interim-mitigation section narrow specific attack paths but do not make a server safe.

## Affected versions and modules

Every supported branch below the patched release is vulnerable to both CVEs; researchers report the bug class reaches back to PHP 5.4.

| PHP branch | Vulnerable | Fixed in | Notes |
| --- | --- | --- | --- |
| 8.5 | < 8.5.12 | 8.5.12 | Also exposed via `fatal_error_backtraces` (the confirmed CVE-2026-109619 exploit path) |
| 8.4 | < 8.4.27 | 8.4.27 | |
| 8.3 | < 8.3.36 | 8.3.36 | |
| 8.2 | < 8.2.35 | 8.2.35 | Branch goes EOL 31 Dec 2026 |
| 8.1, 8.0, 7.4 (EOL) | All | No upstream release | PHP published minimal source backport branches; otherwise use a distro/LTS vendor build or upgrade |
| 5.4 – 7.3 (EOL) | Reported affected | None | No release or backport; contain and upgrade |
| 5.3 (EOL) | Unconfirmed; treat as affected | None | See the PHP 5.3 section below |
| 8.6 RC | Pre-release | Check RC4+ | Not for production |

**Distro packages:** Debian, Ubuntu, RHEL/AlmaLinux, Alpine and others often backport fixes without changing the upstream version string. A server reporting e.g. 8.1.2 may be patched (or not). Judge by the package version against your vendor's advisory (e.g. Ubuntu USN-8948, RHSA-2026:87080), not `php -v` alone. PHP also warns that `rpms.remirepo.net` will not have same-day updates.

**Modules / SAPIs at risk** (any PHP serving web requests):

| SAPI / runtime | Risk | Why |
| --- | --- | --- |
| Apache mod_php (libphp) | Highest | Both CVEs exploitable before any application code runs; full RCE demonstrated |
| PHP-FPM (nginx, Apache proxy_fcgi) | High | CVE-2026-109619 RCE validated via session upload progress; CVE-2026-109618 susceptible by source analysis |
| FrankenPHP | High | Calls request shutdown unconditionally, like mod_php |
| LiteSpeed LSAPI | Medium–High | Modeled on FPM; application-specific exploits likely |
| php-cgi | At risk | Named by PHP as affected |
| PHP CLI only (cron, workers, CI) | Lower | No web request path; still patch with the rest |

**Engine components involved:** multipart/form-data body parsing (OOM trigger), the Zend memory manager OOM handler, the session extension's `session.upload_progress` handler (enabled by default, no `session_start()` needed), fatal-error backtrace state (8.5), and INI diagnostics raised before request startup (`.htaccess` / `php_value` / `PHP_VALUE` overrides).

## PHP 5.3 hosts

Treat any PHP 5.3 server as exposed and unpatchable: no fix exists or is coming, so containment now and migration off 5.3 are the only remediation.

**What is known about 5.3**

| Path | On PHP 5.3 | Basis |
| --- | --- | --- |
| CVE-2026-109618 OOM corruption | Assume vulnerable | PHP gives no lower bound. The longjmp-based OOM bailout predates 5.3; this is our assessment, not a PHP statement. NCSC advises assuming all versions may be affected. |
| CVE-2026-109619 via session upload progress | Not present | Upload progress arrived in PHP 5.4.0; Sansec dates the vulnerable code to 5.4.0. |
| CVE-2026-109619 via fatal error backtraces | Not present | `fatal_error_backtraces` exists only in 8.5. |
| CVE-2026-109619 other stale-state paths | Unknown | PHP says other global/extension state may be reachable depending on app and extensions. |
| Upstream or backport fix | None | PHP's oldest backport branch is 7.4; 5.3 has been EOL since Aug 2014. |

5.3 already carries years of unpatched critical CVEs, so "not confirmed vulnerable" is not a reason to wait.

**Checklist for 5.3 servers**

- [ ] Confirm the version: `php -v` reports 5.3.x. Common sources are CentOS/RHEL 6 (`php-5.3.3`) and Ubuntu 12.04 (`5.3.10`).
- [ ] If the OS is under a paid extended-support contract, ask that vendor in writing whether they will ship a fix. Don't assume one.
- [ ] **Contain today:** remove direct internet exposure (VPN, IP allowlist), or front it with a reverse proxy/WAF that blocks `multipart/form-data` where not needed and caps body size, field count, multipart parts, headers per part and nesting depth.
- [ ] Set `memory_limit=-1` admin-level (`php_admin_value`) so apps can't lower it; set a low `post_max_size`; set `max_input_vars` (5.3.9+) and `max_input_nesting_level` low.
- [ ] Limit cross-request state: FPM `pm.max_requests=1`, or Apache prefork `MaxRequestsPerChild 1` for mod_php. Expect a real CPU cost.
- [ ] Don't count on `session.upload_progress.enabled=0` or `fatal_error_backtraces=0`; those settings don't exist on 5.3.
- [ ] Run the compromise hunt in section 4 on every 5.3 host, given its exposure history.
- [ ] Set a migration date. 5.3 → 8.x is a code migration (e.g. `mysql_*` functions removed in 7.0), so scope app changes now. Interim option: isolate the app in a locked-down container or VM with no outbound access.

## 1. Inventory and validate: does this server need attention?

A server needs attention if it serves web requests through PHP and **any** PHP runtime on it lacks the 21 Oct 2026 fix. Run these on every host, VM and container image.

- [ ] **Find every PHP install and runtime**, not just the default `php` on PATH:

```bash
ls -1 /usr/bin/php* /usr/sbin/php-fpm* /usr/local/bin/php* 2>/dev/null
ps -eo pid,user,lstart,cmd | grep -E 'php-fpm|php-cgi|lsphp|frankenphp|httpd|apache2' | grep -v grep
```

- [ ] **Identify the SAPI in use.** mod_php is the top priority.

```bash
apachectl -M 2>/dev/null | grep -i php      # php_module / php7_module = mod_php
systemctl list-units --type=service | grep -Ei 'php|fpm|litespeed|frankenphp'
```

- [ ] **Check the version of each runtime** (CLI and FPM can differ):

```bash
php -v
for f in /usr/sbin/php-fpm*; do $f -v | head -1; done
```

- [ ] **Check the package build, not just the version string** (distros backport):

```bash
# Debian / Ubuntu
dpkg -l | grep -E '^ii\s+(php|libapache2-mod-php)'
apt changelog php8.3-common 2>/dev/null | grep -m2 -E 'CVE-2026-10961[89]'
# RHEL / Alma / Rocky / Fedora
rpm -qa 'php*'
rpm -q --changelog php-common | grep -m2 -E 'CVE-2026-10961[89]'
# Alpine
apk list -I 2>/dev/null | grep php
```

No CVE line in the changelog and version below the fixed release = vulnerable.

- [ ] **Containers and images:** check running containers and the base images in your registry and Dockerfiles.

```bash
docker ps --format '{{.ID}} {{.Image}}' | while read id img; do echo "$img: $(docker exec $id php -r 'echo PHP_VERSION, " ", PHP_SAPI;' 2>/dev/null)"; done
grep -rhoE '^FROM\s+\S*php\S*' --include=Dockerfile* . | sort -u
```

- [ ] **Windows / IIS:** run `php -v` in every PHP directory referenced by IIS FastCGI handler mappings.
- [ ] **Hosted / panels:** Plesk, cPanel and similar ship their own PHP builds per version; check each enabled handler. Plesk's Linux builds dated 21 Oct 2026 carry the fix.
- [ ] **Verify what the web server actually runs** (catches stale workers and wrong handlers). Drop a temporary file behind auth or on localhost, request it, then delete it:

```php
<?php echo PHP_VERSION, ' ', PHP_SAPI, ' ', ini_get('session.upload_progress.enabled');
```

- [ ] **Record exposure factors** for prioritization (run with the FPM or Apache config, not only CLI):

```bash
php-fpm8.3 -i 2>/dev/null | grep -E 'memory_limit|post_max_size|session.upload_progress.enabled|fatal_error_backtraces'
grep -rnE 'php_(admin_)?(value|flag)' /etc/apache2 /etc/httpd /etc/php*/ 2>/dev/null
grep -rl --include=.htaccess -E 'php_(value|flag)' /var/www 2>/dev/null
grep -rn 'PHP_VALUE\|PHP_ADMIN_VALUE' /etc/nginx 2>/dev/null
```

Per-request INI overrides (`.htaccess`, `php_value`, nginx `PHP_VALUE`) that raise warnings are a confirmed early trigger for CVE-2026-109619.

- [ ] **Optional:** run your vulnerability scanner's new checks (e.g. Qualys QIDs 389102 / 39033, or Composer-based GHSA checks) to catch hosts the inventory missed.

**Priority order:** internet-facing mod_php → internet-facing FPM/FrankenPHP/LSAPI → internal web PHP → CLI-only.

## 2. Patch and verify

Update to PHP 8.5.12, 8.4.27, 8.3.36 or 8.2.35 (or your vendor's fixed build), then restart every PHP process. A patched binary does nothing until old workers are gone.

- [ ] Apply the vendor package update (`apt upgrade`, `dnf update php*`, `apk upgrade`) or build from source. On 8.1 and older, use a vendor/LTS backport or PHP's source backport branches, or upgrade the branch.
- [ ] If you use `rpms.remirepo.net`, switch to distro packages or source builds; PHP says it won't have same-day updates.
- [ ] Rebuild and redeploy container images from patched base images; don't just patch running containers.
- [ ] Restart every runtime: `systemctl restart php*-fpm apache2|httpd`, FrankenPHP, LiteSpeed (`lswsctrl restart`), IIS app pools.
- [ ] Confirm no worker predates the patch: compare process start time with package install time.

```bash
ps -eo lstart,cmd | grep -E 'php-fpm|apache2|httpd' | grep -v grep
ls -l --time-style=full-iso /var/lib/dpkg/info/php*-common.list 2>/dev/null   # or: rpm -qi php-common | grep 'Install Date'
```

- [ ] Re-run the web probe from section 1 and confirm the fixed version and expected SAPI; then delete the probe file.
- [ ] Re-run the package changelog check and confirm the CVE lines are present.
- [ ] Smoke-test file uploads and large POSTs: the fix replaces longjmp OOM with a soft OOM at the next safe point plus a hard `_exit()` at a higher threshold, so memory-heavy requests may fail differently than before.
- [ ] Even after patching, consider keeping `session.upload_progress.enabled=0` if you don't use it; PHP suggests this feature will draw further attacker attention.

## 3. Interim mitigations (only if a patch must wait)

These break specific confirmed attack paths but do not make a server safe; schedule the patch regardless. Set each in `php.ini`, the FPM pool (`php_admin_value`) or Apache config, never `ini_set()` or `.user.ini`, then restart every worker.

**CVE-2026-109618 (OOM corruption)**

- [ ] Set `memory_limit=-1` so applications can't override it (`php_admin_value[memory_limit] = -1`), and confirm no pool, directory or app restores a finite limit. Removes the controllable OOM, not every allocation failure.
- [ ] Lower `post_max_size`: PHP suggests 8M without uploads, 32M with; keep `upload_max_filesize` below it.
- [ ] Enforce limits at the web server/proxy/WAF on decoded body size, input fields, multipart parts, headers per part and nesting depth, including requests without a usable `Content-Length`. Reduce `max_input_nesting_level` and `max_input_vars`.
- [ ] If an app doesn't accept `multipart/form-data`, block it at the frontend.
- [ ] Use a container/cgroup memory limit (`memory.max`) as a backstop, ideally per worker. Do **not** use `RLIMIT_AS` / `RLIMIT_DATA`; they trigger the vulnerable path.

**CVE-2026-109619 (stale global state)**

- [ ] Set `session.upload_progress.enabled=0` (blocks the confirmed session paths, including the validated FPM path).
- [ ] On PHP 8.5, set `fatal_error_backtraces=0` (blocks the confirmed mod_php path).
- [ ] Remove invalid or deprecated per-request INI overrides that emit warnings before startup (`.htaccess`, `php_value`, nginx `PHP_VALUE`). Examples: `auto_detect_line_endings=1`, legacy `assert.*`, explicit `mbstring.internal_encoding` / `iconv.*_encoding`, malformed `memory_limit`. Hiding errors with `error_reporting`, `@` or `display_errors=Off` does not help.
- [ ] Fix attacker-reachable fatal errors and `Maximum execution time exceeded` on upload, import, error-page and callback paths.
- [ ] Where performance allows, `pm.max_requests=1` (FPM) stops state carrying over between requests.
- [ ] For an audit, temporarily enable `log_errors=On`, `error_reporting=E_ALL` and review logs for warnings before scripts run, cleanup diagnostics and fatals. Clean logs are not proof of safety.

## 4. Hunt for compromise

Details leaked on 20 Oct 2026 before the fix, so check exposed servers from that date forward, even after patching. Successful exploitation runs code as the web worker user (e.g. `www-data`, `apache`, `nginx`).

- [ ] Search PHP and web logs for bursts of `Allowed memory size ... exhausted`, `Out of memory`, fatal errors or `Maximum execution time exceeded`, especially on POST/multipart requests from a single source.

```bash
grep -hE 'Allowed memory size|Out of memory|PHP Fatal error|Maximum execution time' /var/log/php*/*.log /var/log/apache2/error.log /var/log/httpd/error_log /var/log/nginx/error.log 2>/dev/null | tail -200
```

- [ ] Look for worker crashes/restarts: FPM `child ... exited on signal 11 (SIGSEGV)`, Apache `segmentation fault`, unusual worker churn.
- [ ] List processes spawned by the web user that aren't PHP or the web server (shells, `curl`/`wget`, miners, reverse shells):

```bash
ps -u www-data,apache,nginx -o pid,ppid,lstart,cmd 2>/dev/null | grep -vE 'php-fpm|apache2|httpd|nginx'
```

- [ ] Find recently written PHP files in web roots and temp dirs (webshells):

```bash
find /var/www /tmp /var/tmp /dev/shm -type f \( -name '*.php' -o -name '*.phtml' \) -newermt '2026-10-20' 2>/dev/null
```

- [ ] Check for new cron entries, SSH keys, or systemd units owned by or writable by the web user.
- [ ] Review outbound connections from web servers for long-lived sessions or traffic to low-reputation hosts.
- [ ] Any hit: isolate the host, preserve logs and memory, rotate secrets the web user could read (`.env`, DB credentials, API keys), then rebuild rather than clean.

## Sources

- [PHP.net security advisory, 21 Oct 2026](https://www.php.net/archive/2026.php#2026-10-21-5)
- [GHSA-w8hw-w9fx-j79w: RCE via OOM corruption (CVE-2026-109618)](https://github.com/php/php-src/security/advisories/GHSA-w8hw-w9fx-j79w)
- [GHSA-6wmf-4rv5-p3rp: RCE via stale global state (CVE-2026-109619)](https://github.com/php/php-src/security/advisories/GHSA-6wmf-4rv5-p3rp)
- [Rapid7 ETR on both CVEs](https://www.rapid7.com/blog/post/etr-cve-2026-109618-and-cve-2026-109619-critical-unauthenticated-remote-code-execution-in-php/)
- [Sophos: PoC released, monitoring guidance](https://www.sophos.com/en-us/blog/proof-of-concept-exploit-released-for-php-vulnerabilities-109618-109619)
- [Qualys detections and distro advisories](https://blog.qualys.com/vulnerabilities-threat-research/2026/10/20/php-announces-critical-security-release-for-two-vulnerabilities)
- [AlmaLinux patched packages](https://almalinux.org/blog/2026-10-21-php2shell-cve-2026-109618-cve-2026-109619/)
- [Plesk PHP builds](https://support.plesk.com/hc/en-us/articles/44397729587351-Vulnerability-CVE-2026-109618-and-CVE-2026-109619-Remote-code-execution-in-PHP-shipped-with-Plesk)
- [Google Cloud / Wiz: affected range back to 5.4](https://cloud.google.com/blog/products/identity-security/responding-to-core-php-vulnerabilities-cve-2026-109618-cve-2026-109619)
- [Sansec: vulnerable code dates to PHP 5.4.0](https://sansec.io/research/php2shell)
- [NCSC: assume all PHP versions may be affected](https://www.ncsc.gov.uk/news/critical-security-issue-affecting-php)
- [watchTowr FAQ: PHP 5 onward reported, no 5.x/7.x patches](https://watchtowr.com/intelligence/php-cve-2026-109618-faq/)
