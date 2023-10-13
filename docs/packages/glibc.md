# Override package: glibc

## EL9

- Version `2.34-60.7.el9_2.security.0.3`
- Based on `2.34-60.el9_2.7`

### Changes summary

- Distrust and/or unset many more environment variables used by current and previous glibc versions when running SUID/SGID/setcap (Owl via ALT Linux)
- When `syslog(3)`/`vsyslog(3)` is called by a SUID/SGID/setcap program without a preceding call to `openlog(3)`, don't blindly trust `__progname` for the syslog ident (Owl via ALT Linux)
- In `syslog(3)/vsyslog(3)` use `asctime_r(3)+localtime_r(3)` instead of `strftime_r()` so that month names don't depend on current locale settings (Owl via ALT Linux)
- In `asprintf(3)/vasprintf(3)` reset the pointer to NULL on error, like BSDs do, so that the caller wouldn't access memory over an uninitialized or stale pointer (ALT Linux)
- In `fread(3)/fwrite(3)` check for potential integer overflow (ALT Linux)
- In `tmpfile(3)` use the `TMPDIR` environment variable (when not running SUID/SGID/setcap) (ALT Linux)

#### Known-effective vulnerability mitigations and fixes

`2.34-60.el9_2.security.0.2` included mitigations sufficient to avoid security exposure of [CVE-2023-4911](https://www.openwall.com/lists/oss-security/2023/10/03/2) and a backport of upstream glibc fix of [CVE-2023-4527](https://www.openwall.com/lists/oss-security/2023/09/25/1) that was not yet in upstream EL. In the update to `2.34-60.7.el9_2.security.0.3`, we retained the mitigations while rebasing on upstream EL's package with upstream fixes for these vulnerabilities (and more).

In general, inclusion of additional security fixes will be "reverted" if and when those get included in upstream EL packages that we rebase our changes on.

### Change log

```
* Fri Oct  6 2023 Solar Designer <solar@openwall.com> - 2.34-60.7.el9.security.0.3
- Rebase on 2.34-60.7, drop "our" CVE-2023-4527 patch in favor of RH's

* Mon Sep 25 2023 Florian Weimer <fweimer@redhat.com> - 2.34-60.7
- Fix memory leak regression in getaddrinfo (RHEL-2425)

* Tue Sep 19 2023 Carlos O'Donell <carlos@redhat.com> - 2.34-60.6
- CVE-2023-4911 glibc: buffer overflow in ld.so leading to privilege escalation (RHEL-2999)

* Tue Sep 19 2023 Carlos O'Donell <carlos@redhat.com> - 2.34-60.5
- Revert: Always call destructors in reverse constructor order (RHEL-3385)

* Mon Sep 18 2023 Siddhesh Poyarekar <siddhesh@redhat.com> - 2.34-60.4
- CVE-2023-4806 glibc: potential use-after-free in getaddrinfo (RHEL-2425)

* Fri Sep 15 2023 Siddhesh Poyarekar <siddhesh@redhat.com> - 2.34-60.3
- CVE-2023-4813: potential use-after-free in gaih_inet (RHEL-2437)

* Fri Sep 15 2023 Carlos O'Donell <carlos@redhat.com> - 2.34-60.2
- CVE-2023-4527: Stack read overflow in getaddrinfo in no-aaaa mode (#2234715)

* Wed Sep 13 2023 Florian Weimer <fweimer@redhat.com> - 2.34-60.1
- Always call destructors in reverse constructor order (RHEL-3385)

* Mon Oct  2 2023 Solar Designer <solar@openwall.com> - 2.34-60.el9.security.0.2
- Add glibc-owl-alt-sanitize-env.patch stitched from several ALT Linux commits
  as none of their revisions matched this package's set of backports as-is
- Add glibc-upstream-no-aaaa-CVE-2023-4527.patch based on upstream commit
  bd77dd7e73e3530203be1c52c8a29d08270cb25d fixing
  CVE-2023-4527: Stack read overflow with large TCP responses in no-aaaa mode

* Tue Sep 26 2023 Solar Designer <solar@openwall.com> - 2.34-60.el9.security.0.1
- Revise the texinfo documentation edit of glibc-2.34-alt-asprintf.patch via
  glibc-2.34-rocky-asprintf.patch

* Sat Sep 23 2023 Solar Designer <solar@openwall.com> - 2.34-60.el9.security.0.0
- Add some of the patches from ALT Linux as of when they were at 2.34:
  https://git.altlinux.org/gears/g/glibc.git
  git show 5fa32fb0f8509f4b2b1105d71b45966dfbadc099 > glibc-2.34-alt-tmpfile.patch
  git show f97e5d60a6a4c9cb64e3b9ee6f5113969cf07d87 > glibc-2.34-alt-asprintf.patch
  git show cd45d0f74560325cc48aedb9f56881270ab3dfab > glibc-2.34-alt-libio-bound.patch
  git show 436eb1017c04aee3a553c2868d00a4b046e5e394 > glibc-2.34-owl-alt-syslog-ident.patch
  git show 03a86c234873723c26b7e387c498c1332c223968 > glibc-2.34-mjt-owl-alt-syslog-timestamp.patch
```
