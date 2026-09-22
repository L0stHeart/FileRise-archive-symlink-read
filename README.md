# FileRise: file read via symlink during archive extraction

https://github.com/error311/FileRise/security/advisories/GHSA-mjfc-6wg2-xmcw

Affects FileRise 3.9.1 up to, but not including, 3.25.0. Fixed in 3.25.0.

Medium. CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:N/A:N (6.5).
CWE-59, CWE-61, CWE-552.
No CVE assigned.

A logged-in user can read files outside the storage root. Archive extraction follows a symlink that the extractor writes inside the workspace. This is the `unar` path. The 7-Zip include-list path does not do this, so an install with no `unar` binary is not affected. The official Docker image ships `unar`.

I first sent this as 7.7 with scope changed, and as everything through 3.24.0. The vendor traced it to the 3.9.1 baseline. We both took 6.5, scope unchanged, from 3.9.1.

Reported 2026-07-31 through GitHub private vulnerability reporting. The vendor published the advisory on 2026-08-12.

L0stHeart
