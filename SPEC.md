# Lesson 01 specification

## STUDIO-001 · Launch a tested calculator on an existing server

**Status:** Launched and verified; PR and CI linked below.
**Own issue:** [#1](https://github.com/mohammedhammoudeh8/my-server-studio/issues/1) · **Branch:** `studio-001`

## Agreed result

Launch the supplied published calculator image through my existing Traefik
network on a new HTTPS hostname, preserving my existing services (Traefik plus
my Gridiron prod and QA sites). No new proxy, updater or substitute image.

- **Public hostname:** https://calc.mohammedhammoudeh.duckdns.org
- **New launch or adopted deployment, and reason:** New launch. `doctor` reported `launch-new`: no existing route used this hostname.
- **Server / network / image:** my DigitalOcean droplet as non-root user `mohamad` (Docker group, no sudo) · network `hosting_default` · pinned image from [calculator-release.json](studio/calculator-release.json)
- **Gap found and fixed before launch:** my Traefik HTTPS entrypoint had no default certificate resolver (each site set `certresolver=le` itself), so the studio route would not have received a trusted certificate. Added `--entrypoints.websecure.http.tls.certresolver=le` through my Gridiron repo's qa pipeline (commit 71cb5dd). Existing routes were unaffected.
- **Known access gap:** NJIT Wi-Fi resets HTTPS connections to the droplet during the TLS handshake, even by IP. The site loads over cellular and passed the server-side HTTPS check.

## Evidence

| Evidence | Actual result / link |
|---|---|
| Hosted image/commit, trusted HTTPS, valid and rejected API cases | `verify` passed: health/identity passed, 6 x 7 returned 200 with 42, division by zero 400, numeric string 422, trusted_public_https true. [studio/evidence.json](studio/evidence.json) |
| Preservation of unrelated services | 3 existing containers checked, unchanged (identity, state, published ports) |
| Local prepared checks | `check --fast` on Windows **failed**: pytest could not collect `tests/test_restrict_dashboard.py` because `signal.SIGHUP` does not exist on Windows, and Pylint reported a failure that was not investigated locally. Full local E2E not run (no local Docker). |
| Full Black/Pylint/pytest/E2E | GitHub CI on the PR (Linux): see PR below |
| My prediction, check interpretation, browser observation, keep decision | [LEARNING.md](LEARNING.md), my own words, revised after coach feedback where noted |
| PR, CI, merge, issue closure | Pending |

CI does not deploy this server. A green check, closed issue or AI assertion alone
does not establish the full result.

## Optional instructor history

The records below are background, not steps to repeat.

## HOST-001 · Keep the Traefik dashboard private

[Historical requirement and evidence](book/changes/hosting-spec-history.md#host-001--keep-the-traefik-dashboard-private).

## HOST-002 · Controlled host maintenance

[Historical requirement and evidence](book/changes/hosting-spec-history.md#host-002--controlled-host-maintenance).

## HOST-003 · Companion image security maintenance

[Historical requirement and evidence](book/changes/hosting-spec-history.md#host-003--companion-image-security-maintenance).