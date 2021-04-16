Why you may need buildroot-dl-mini (for buildroot users)
--------------------------

The buildroot-dl-mini allows you to build rootfs offline, without external dependencies redownloading.
It is not always possible to follow last versions of external developer
repositories from different independent places.
When external developers update or remove their projects, products or service down, 
you often have build failure,
so you can not use your code or product anymore, until you update external dependencies,
and then modify your code to support changes on external dependencies.
When you use many external dependencies of independent developers (as buildroot does),
it may take all your live to only support changes on external dependencies
in your project (like in buildroot), or you must have 5-50 paid programmers and admins,
which will do this task for you.

You may ask then why rebuild code from source again and again.
It is not only to support different architectures.
Life changes, so you often want to modify
subset of features and settings in your embedded project.

Or you may want to patch or modify part of programs and libraries
in buildroot dependencies by yourself. It is big difference between
small modifications by youself (you team)
and following big default changes by 100-1000+ independent developers.

How to use buildroot-dl-mini offline
--------------------------

Previously you downloaded all wishes to /your/path/clonned/buildroot-dl-mini/dl/

```
$ cd /your/path/to/buildroot-dl-mini/ #go to you dir buildroot-dl-mini
$ rm -rf output/ #remove old stuff to avoiding build conflicts
$ less .config #view your buildroot config to be sure in your desires: target arch, target bin, target variant, JLEVEL, C library (musl, uclibc-ng, glibc), gcc compiler version, binutils, kernel headers, preferred shell, output rootfs images ...
$ make menuconfig #remove problematic Toolchain options (C library, Kernel, C++, Fortran ...) or unwanted Target Packages
$ make help | grep -iE config #read at least part of help
$ make
```

How to prepair your custom buildroot-dl-mini (online needed)
--------------------------

1. You may download all wishes to /your/path/clonned/buildroot-dl-mini/dl/
, but it is simpler to allow buildroot to do this for you.
2. Backup your configs somewhere (put .config files in other place).
3. Get original buildroot source (and be sure that ./dl/ and ./output/ are clean).
4. Restore your configs to buildroot (or you may use configs from buildroot-dl-mini).
5. Select all disabled Toolchain options (you must do it manually), e.g. by `make menuconfig`
6. To select all Packages you may run `make allyespackageconfig` (mismatched with `make allpackageyesconfig`). This step is not recommended, only if you are advanced buildroot user and understand.
7. **Do not wait for full build finish, break manually after 1 minute by Ctrl+C.**
8. Deselect unwanted Toolchain options (e.g. C++ support, Fortran support, OpenMP support) on `make menuconfig`.
9. Deselect unwanted Target Packages (e.g. mail/mutt, net/dnsmasq, net/lynx, editors/uemacs, viewers/mc, fs/udftools).
10. Deselect unwanted Bootloaders, Kernels, Host utilities, Filesystem images (leave only your prefered).
11. Configure also Linux kernel and Busybox as you wish (backup configs separately or use special defines in buildroot).
12. Need one success build with external dependecies downloading: `make`.
13. Remove ./output/ folder (if don not need images build result currently).
14. Backup your created and customized **buildroot-dl-mini** as you wish.

Original buildroot README:
--------------------------

Buildroot is a simple, efficient and easy-to-use tool to generate embedded
Linux systems through cross-compilation.

The documentation can be found in docs/manual. You can generate a text
document with 'make manual-text' and read output/docs/manual/manual.text.
Online documentation can be found at http://buildroot.org/docs.html

To build and use the buildroot stuff, do the following:

1) run 'make menuconfig'
2) select the target architecture and the packages you wish to compile
3) run 'make'
4) wait while it compiles
5) find the kernel, bootloader, root filesystem, etc. in output/images

You do not need to be root to build or run buildroot.  Have fun!

Buildroot comes with a basic configuration for a number of boards. Run
'make list-defconfigs' to view the list of provided configurations.

Please feed suggestions, bug reports, insults, and bribes back to the
buildroot mailing list: buildroot@buildroot.org
You can also find us on #buildroot on Freenode IRC.

If you would like to contribute patches, please read
https://buildroot.org/manual.html#submitting-patches
