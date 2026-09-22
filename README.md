# FileRise archive extraction follows symlinks

https://github.com/error311/FileRise/security/advisories/GHSA-mjfc-6wg2-xmcw

Severity: medium (CVSS 6.5, `CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:N/A:N`). CWE-59, CWE-61, CWE-552. No CVE.

FileRise 3.9.1 up to but not including 3.25.0 is affected. Fixed in 3.25.0.

A logged-in user can read files outside the storage root. Extraction follows a symlink written inside the workspace. This is the unar path. The 7-Zip include-list path does not do it, so an install with no unar binary is outside this bug. The official Docker image ships unar, and that is the build I tested.

Dot-prefixed basenames never make the allow-list, so they are out of reach. Other names are not. That includes the application user database, the ACL state, the site configuration, and files outside the tree such as the host account list.

I first sent this as 7.7 with scope changed, and I wrote the range as everything through 3.24.0. The lower bound was wrong. The vendor traced it to 3.9.1 and we both took 6.5, scope unchanged.

3.25.0 keeps the original workspace as the extraction boundary. I retested that release. Ordinary extraction still works. This read does not. The vendor was going to request a CVE from GitHub once the version bounds were settled. I agreed not to file a second request. As of 22 September 2026 the advisory still has no CVE.

I tested a local `error311/filerise-docker` container, v3.24.0 (commit 765eccc) and then v3.25.0. Nothing else.

Reported privately on 31 July 2026. The vendor published the advisory on 12 August 2026.

L0stHeart
