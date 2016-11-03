aurremote(1) -- set up git remote and branch for AUR distibution
================================================================

## SYNOPSIS

`aurremote` <package> [branch]


## DESCRIPTION

This program prepares a local branch to track an AUR package repository.

In the Git repository it was run from within, it will configure a remote pointing to the AUR package repository of the given package and set up the given local branch to track the master of this remote.

The branch name defaults to _aur/package name_ when not specified.


## BUGS

This project was created by XZS <d.f.fischer@web.de> and [lives at GitHub](http://github.com/dffischer/makepkg-expanded). Bugs can be filed in [the tracker found there](http://github.com/dffischer/makepkg-expanded/issues).


## SEE ALSO

aurbranch(1), git(1).
