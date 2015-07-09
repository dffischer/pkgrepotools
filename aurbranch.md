aurbranch(1) -- maintain a git branch for AUR distibution
=========================================================

## SYNOPSIS

`aurbranch`


## DESCRIPTION

This program helps to create and update a branch in a local git repository to be pushed to the Arch User Repository.

It automatically generates a .SRCINFO file from a PKGBUILD in the local working directory and commits changes in both on top of a dedicated branch found in the git repository it is run from within. This branch is named like the pkgbase variable as denoted by the PKGBUILD, prefixed with "aur/". If this branch does not exist, it will be created.

All this is done without altering the state of the repository or its working tree and thus save to run in dirty worktrees.


## BUGS

This project was created by XZS <d.f.fischer@web.de> and [lives at GitHub](http://github.com/dffischer/makepkg-expanded). Bugs can be filed in [the tracker found there](http://github.com/dffischer/makepkg-expanded/issues).


## SEE ALSO

git(1), mksrcinfo.
