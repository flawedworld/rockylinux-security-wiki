# SIG/Security Wiki

The Security SIG repositories provide extra security-related packages and security-hardened override packages (replacing those from the main distribution) for Rocky Linux and other EL distributions.

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