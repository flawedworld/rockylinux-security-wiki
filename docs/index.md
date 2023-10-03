# SIG/Security Wiki

The Security SIG repositories provide extra security-related packages and security-hardened override packages (replacing those from the main distribution) for Rocky Linux and other Enterprise Linux (EL) distributions.

## Responsibilities

Developing and maintaining various security related packages that are not in upstream EL. Identifying, developing, and maintaining security hardening changes relative to upstream EL packages. Occasionally including/backporting additional security fixes that are not yet in upstream EL packages. Contributing to the respective upstreams where practical.

## Repo Installation

```
dnf install rocky-release-security
```

## Packages

### Extra packages (for EL8 and EL9)

- [lkrg](https://lkrg.org) (Linux Kernel Runtime Guard)
- [passwdqc](https://www.openwall.com/passwdqc/) (Password/passphrase strength checking and policy enforcement)

### Override packages (currently only for EL9)

- glibc (adds many security-hardening changes originating from Owl and ALT Linux on top of EL package)
- openssh (fewer shared libraries exposed in sshd processes while otherwise fully matching EL package's functionality)

The changes are described in more detail in the package changelogs.
More packages/changes are planned, including override packages also for EL8.

#### Known-effective vulnerability mitigations and fixes

`glibc-2.34-60.el9_2.security.0.2` (specifically the `.0.2` version!) includes mitigations sufficient to avoid security exposure of [CVE-2023-4911](https://www.openwall.com/lists/oss-security/2023/10/03/2) and a backport of upstream glibc fix of [CVE-2023-4527](https://www.openwall.com/lists/oss-security/2023/09/25/1) that was not yet in upstream EL.

The inclusion of additional security fixes will be "reverted" if and when those get included in upstream EL packages that we rebase our changes on.

## Source code

Just like for other Rocky Linux SIGs, the source trees for Security SIG packages are maintained in [per-package git repositories](https://git.rockylinux.org/sig/security/src). Each repository contains branches `r8` and/or `r9` corresponding to target EL version.

## Contributing

If anyone else wants to join this effort - in any capacity including development, maintenance, testing, documentation, user support, spreading the word, or something else - please join the Mattermost channel below and let us know!

We also welcome well-reasoned suggestions/feedback/preferences on direction we should take (e.g., only making changes on top of EL's vs. offering newer upstream versions), what else to package, and what other changes to include.

## Meetings / Communications

We hang out in our [Security Mattermost channel](https://chat.rockylinux.org/rocky-linux/channels/security).

## Members

Some of the people particularly active with setting up this SIG so far:

| Name           | Mattermost Name |
|----------------|-----------------|
| Neil Hanlon    | @neil           |
| Scott Shinn    | @atomicturtle   |
| Solar Designer | @solardiz       |