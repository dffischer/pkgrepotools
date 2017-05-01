aurremote(1) -- set up git remote and branch for AUR distibution
================================================================

## SYNOPSIS

`aurremote` [`options`] <package>


## DESCRIPTION

This program uses offremote(1) to prepare a local branch tracking an AUR package repository.

In the Git repository it was run from within, it will configure

  - a remote pointing to the AUR package repository of the given package and

  - a local branch of the same name to track the master of this remote

both named like the package.

If the local branch does not yet exist, aurremote acts to adopt an existing package. The history will immedialty be pulled to the local branch. If a local branch already is present, it is assumed that the package was not yet created and pulling from there would thus lead to an error. You may fetch the remote manually afterwards.


## OPTIONS

  - `-h`:
    Shows a summary of the options.

  - `-c`:
    Act as suited for a package collection. Instead of simply calling them _aur_, remote and branch name are suffixed include the package name.


## EXIT STATUS

Exit codes from offremote(1) are passed through unaltered.


## BUGS

This project was created by XZS <d.f.fischer@web.de> and [lives at GitHub](http://github.com/dffischer/pkgrepotools). Bugs can be filed in [the tracker found there](http://github.com/dffischer/pkgrepotools/issues).


## SEE ALSO

offremote(1), aurbranch(1), git(1).
