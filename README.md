# makepkg-expanded

Tired of all the clutter _makepkg-template_ causes in your PKGBUILD? Use makepkg-expanded to build them leaving the template markers exactly as you wrote them. It will
- build a package without any change to the PKGBUILD,
- only propagating changes caused by a `pkgver` function and
- produce a source tarball stripped from all template markers, keeping it clean and readable for the [AUR](https://aur.archlinux.org/),
- batch build all your packages in one go, also preparing them for AUR upload.
- It also is aware of git and can use a directory of repository-wide templates.

For more details and options, see [the manual page](makepkg-expanded.md).
