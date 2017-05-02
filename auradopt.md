auradopt(1) -- adopt an AUR package as a branch to an existing git repository
=============================================================================

## SYNOPSIS

`auradopt` [`options`] <package>


## DESCRIPTION

This program uses offremote(1) to prepare a local branch tracking an AUR package repository.

In the Git repository it was run from within, it will configure

  - a remote pointing to the AUR package repository of the given package and

  - a local branch of the same name to track the master of this remote

both named like the package, and then immedialty pull remote history to the local branch.


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
