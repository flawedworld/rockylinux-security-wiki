# Extra package: hardened_malloc

## EL9

- Version `hardened_malloc-12-2.el9_2.security.x86_64`
- Based on upstream version `12`
- No plans to support older Rocky Linux versions due to glibc being too old

### Package summary

This package ships the "normal" and "light" configurations of the [GrapheneOS](https://grapheneos.org) [hardened_malloc](https://github.com/GrapheneOS/hardened_malloc) project. The official README.md in the upstream project documents security properties and explains the differences between the regular and light variants.

### Usage in Rocky Linux

It is strongly reccomended to read all documentation here before deploying this package on your infrastructure.

In order to support the large amount of mappings caused by guard slabs and large allocation guard guard regions, the `vm.max_map_count` sysctl is increased as part of package installation to `1048576` in `/etc/sysctl.d/hardened_malloc.conf`. The package ships 2 builds of hardened_malloc, the regular variant, which is located at `/usr/lib64/libhardened_malloc.so` and can be preloaded using the `hardened_malloc_preload.sh` script, and the light variant, which is located at `/usr/lib64/libhardened_malloc-light.so` and can be preloaded using the `hardened_malloc_light_preload.sh` script. The preload scripts adds the relevant library to `LD_PRELOAD` and then loads the desired binary, as shown in the following example: `hardened_malloc_preload.sh uname`. Users may choose to set an OS-wide `LD_PRELOAD` with hardened_malloc. This can be done by adding the desired library, for example, `/usr/lib64/libhardened_malloc.so`, into your `/etc/ld.so.preload`. Be aware that applications where `AT_SECURE` is set, this approach will not work. 

It is suggested that if you wish to deploy hardened_malloc systemwide, that you deploy it in your `LD_PRELOAD` with the normal variant globally, and then for applications which are peformance sensitive, or which do not work, try them individually with the light variant using the preload script or by setting `LD_PRELOAD` within a systemd service namespace, and then if that does not resolve your issue, disabling it by running the program in it's own systemd service namespace.

### Bugs uncovered by hardened_malloc

As with all infrastructure changes, ensure you test in your staging environment extensively before deploying into production. Many packages and projects suffer from memory corruption bugs, which hardened_malloc uncovers, which when running under glibc, are not encountered during operation. Some applications may crash during usage, completely break, or break when running with certain configurations. Bugs in packages are typically the result of upstream project bugs, and should be reported there. In some cases these bugs are fixed in later versions in the upstream project, in which case the bug is an issue with Rocky Linux, and should be reported to Rocky Linux, so that the patch may be included.

| Package name    | Latest version tested                             | Normal variant | Light variant |
|-----------------|---------------------------------------------------|----------------|---------------|
| php             | php-8.0.30-1.el9_2.x86_64                         | Broken         | Broken        |
| php             | php-8.1.14-1.module+el9.2.0+15232+36037ab0.x86_64 | Broken         | Broken        |
| sssd            | sssd-2.8.2-3.el9_2.x86_64                         | Broken         | Broken        |

### Potential for issues with EDR

By nature of relying on `LD_PRELOAD`, if you have EDR software on your server, it may falsely send alerts when using hardened_malloc. If it doesn't, your EDR is probably terrible or misconfigured.

### Change log

```
* Wed Nov  8 2023 flawedworld <flawedworld@flawed.world> 12-2
- Set CONFIG_NATIVE to false
- Mark libraries as executable (change to 755 permissions)
- Add hardened_malloc_light_preload.sh
- Fix arm64 building

* Sat Oct 28 2023 flawedworld <flawedworld@flawed.world> 12-1
- Initial packaging for hardened_malloc version 12, co-authored-by
  Scott Shinn (atomicturtle) and Solar Designer
```
