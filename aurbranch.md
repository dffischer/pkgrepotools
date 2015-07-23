aurbranch(1) -- maintain a git branch for AUR distibution
=========================================================

## SYNOPSIS

`aurbranch` [`options`] [additional files ...]


## DESCRIPTION

This program helps to create and update a branch in a local git repository to be pushed to the Arch User Repository.

It uses offbranch(1) to compose a new revision from PKGBUILD, .SRCINFO and given additional files which will be placed on top of a branch named by the pkgbase variable as denoted by the PKGBUILD, prefixed with _aur/_.


## OPTIONS

  - `-h`:
    Shows a summary of the options.

  - `-p` _buildscript_:
    Select an alternative file to include instead of _PKGBUILD_.

    This file will be stored to the distribution branch under the name _PKGBUILD_, regardless of its original name. It will also be used to generate the .SRCINFO from to store beside it.

    If a file named PKGBUILD is also specified as additional file, it will be ignored in favor of this file. However, it will still be considered when changed files are searched to propose a commit message.

All file names are directly passed to offbranch(1), accompanied by the PKGBUILD and .SRCINFO.


## BUGS

This project was created by XZS <d.f.fischer@web.de> and [lives at GitHub](http://github.com/dffischer/makepkg-expanded). Bugs can be filed in [the tracker found there](http://github.com/dffischer/makepkg-expanded/issues).


## SEE ALSO

offbranch(1), mksrcinfo.
