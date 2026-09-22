# FileRise: file read via symlink during archive extraction

https://github.com/error311/FileRise/security/advisories/GHSA-mjfc-6wg2-xmcw

FileRise is a self-hosted PHP file manager (error311/FileRise). I reviewed v3.24.0, commit `765eccc`, and sent this through GitHub private vulnerability reporting on 2026-07-31. The vendor published the advisory on 2026-08-12. Fixed in 3.25.0. No CVE has been assigned.

A logged-in user can read files outside the storage root. Archive extraction follows a symlink that the extractor writes inside the workspace. This is specific to the `unar` path. The 7-Zip include-list path does not do it, so an install with no `unar` binary is outside this bug. The official Docker image ships `unar`, and that is the configuration I tested.

What can be read is whatever the entry-name allow-list will still accept. Dot-prefixed basenames are dropped before they get that far. Other names are not: the application user database, the ACL file, site configuration, and files outside the app tree such as the host account list.

I first scored it 7.7 with scope changed, and I wrote the range as everything through 3.24.0. That lower bound was wrong. The vendor traced the behavior to the v3.7.0 import and then narrowed this particular bug to 3.9.1. Scope stays on the application. The published score is the one we both accepted.

| | |
|---|---|
| Affected | 3.9.1 ≤ version < 3.25.0 |
| Fixed | 3.25.0 |
| Severity | Medium, 6.5 |
| Vector | CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:N/A:N |
| CWE | CWE-59, CWE-61, CWE-552 |
| Privileges | Any logged-in user |
| CVE | Not assigned |

3.25.0 keeps the original workspace as the extraction boundary, so a symlink cannot retarget the root. I retested that release. Ordinary extraction still works. This read does not.

The vendor said they would request the CVE from GitHub once the version bounds were final. I agreed not to file a second request anywhere else. As of 2026-09-22 the advisory still has no CVE id.

Testing was a local Docker container, `error311/filerise-docker`, first at v3.24.0 and again at v3.25.0. Nothing else was touched.

L0stHeart
https://github.com/L0stHeart
