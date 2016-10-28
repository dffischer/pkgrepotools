aurbranch(1) -- maintain a git branch for AUR distibution
=========================================================

## SYNOPSIS

`aurbranch` [`options`] [additional files ...]


## DESCRIPTION

This program helps to create and update a branch in a local git repository to be pushed to the Arch User Repository.

It uses offbranch(1) to compose a new revision from PKGBUILD, .SRCINFO and given additional files which will be placed on top of a dedicated distribution branch.

If the program is executed in the root of a git repository, the branch will be called _aur_. If it is instead run from a directory deeper within a repository, a PKGBUILD collection repository is assumed and the branch will be named by the pkgbase variable as denoted by the PKGBUILD, prefixed with _aur/_. This way, all distribution branches are recognizable by a common naming scheme.


## OPTIONS

  - `-h`:
    Shows a summary of the options.

  - `-p` _buildscript_:
    Select an alternative file to include instead of _PKGBUILD_.

    This file will be stored to the distribution branch under the name _PKGBUILD_, regardless of its original name. It will also be used to generate the .SRCINFO from to store beside it.

    If the original PKGBUILD is tracked by the repository, it is recommended to also specify it prefixed by a colon so offbranch(1) considers it when proposing a commit message.

  - `-s` _separator_:
    Change the separator for renaming.

    This option is passed through unaltered to offbranch(1).

All file names are directly passed to offbranch(1), accompanied by the PKGBUILD and .SRCINFO. This provides the full range of features such as renaming files and specifying paths to search for a commit message. For more details, see the offbranch(1) manual page, especially the section about "Renaming Files".

All symbolic links that appear in the arguments are resolved and replaced with their link targets. That means that it is even safe to include links pointing outside the repository, as only the content will be commited instead of the plain link reference.

If a .SRCINFO file is present as an argument, aurbranch will use it directly. If it is not explicitly specified, a .SRCINFO will be generated from the PKGBUILD and overwrite a possibly present file of the same name in the current working directory.


## EXIT STATUS

Exit codes are passed through unaltered from offbranch(1). The its manual page for a more detailed description of their meaning.


## BUGS

This project was created by XZS <d.f.fischer@web.de> and [lives at GitHub](http://github.com/dffischer/makepkg-expanded). Bugs can be filed in [the tracker found there](http://github.com/dffischer/makepkg-expanded/issues).


## SEE ALSO

offbranch(1), mksrcinfo, git(1).
