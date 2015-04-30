Makepkg-expanded is readily prepared to gather templates from multiple directories for use in the same build script. However, the underlying _makepkg-template_ program needs to support multiple `--template-dir` options for this to work, as `-t` options to makepkg-expanded are passed down to makepkg-template in this form. However, the current version does not yet support this.

A patch to teach it so exists, but was only recently accepted into the [pacman](https://www.archlinux.org/pacman/) project and has not yet landed in its source repository.


# Patching pacman

So to enable it, the pacman sources have to be patched during installation. Fortunately, this process can be automated completely by building pacman through the [ABS](https://wiki.archlinux.org/index.php/Arch_Build_System) when [customizepkg](https://github.com/ava1ar/customizepkg) is installed. Using [Yaourt](https://github.com/archlinuxfr/yaourt) it is even simpler.


## Manually

1. Install [customizepkg from the AUR](https://aur.archlinux.org/packages/customizepkg) if it is not already installed.

  ```bash
  cd /tmp
  curl https://aur.archlinux.org/packages/cu/customizepkg/customizepkg.tar.gz | tar xz
  cd customizepkg
  makepkg
  pacman -U customizepkg-*.pkg.tar*
  cd /tmp
  rm -r pacman
  ```

  Evidently, the line utilizing pacman needs to be run as root.

2. Download [the script to patch the pacman PKGBUILD](pacman.custom) to `~/.customizepkg` (or even `/etc/customizepkg.d`).

  ```bash
  curl --create-dirs -o ~/.customizepkg/pacman \
    https://raw.githubusercontent.com/dffischer/makepkg-expanded/master/pacman.custom
  ```

3. Retrieve the pacman package files from abs

  ```bash
  cd /tmp
  curl https://projects.archlinux.org/svntogit/packages.git/snapshot/packages/core.tar.xz | tar xz
  cd packages/pacman/trunk
  customizepkg -m
  makepkg
  pacman -U pacman-*.pkg.tar*
  cd /tmp
  rm -r packages
  ```

  Again, the pacman command needs root privileges.


## Through Yaourt

As Yaourt installs from AUR and ABS automatically invoking customizepkg when needed, steps (1) and (3) can be  condensed to one line each.

```bash
yaourt -Sa customizepkg
mkdir ~/.customizepkg
curl --create-dirs -o ~/.customizepkg/pacman \
  https://raw.githubusercontent.com/dffischer/makepkg-expanded/master/pacman.custom
yaourt -S pacman
```
