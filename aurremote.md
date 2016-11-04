aurremote(1) -- set up git remote and branch for AUR distibution
================================================================

## SYNOPSIS

`aurremote` <package> [branch]


## DESCRIPTION

This program prepares a local branch to track an AUR package repository.

In the Git repository it was run from within, it will configure a remote pointing to the AUR package repository of the given package and set up the given local branch to track the master of this remote.

The branch name defaults to _aur/package name_ when not specified.

If the local branch does not yet exist, aurremote acts to adopt an existing package. The history will immedialty be pulled to the local branch. If a local branch already is present, it is assumed that the package was not yet created and pulling from there would thus lead to an error. You may fetch the remote manually afterwards. It has the exact same name as the package.


## BUGS

This project was created by XZS <d.f.fischer@web.de> and [lives at GitHub](http://github.com/dffischer/makepkg-expanded). Bugs can be filed in [the tracker found there](http://github.com/dffischer/makepkg-expanded/issues).


## SEE ALSO

aurbranch(1), git(1).
