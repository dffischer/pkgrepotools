aurbranch(1) -- maintain a git branch for AUR distibution
=========================================================

## SYNOPSIS

`aurbranch` [`options` | additional files ...]


## DESCRIPTION

This program helps to create and update a branch in a local git repository to be pushed to the Arch User Repository.

It uses offbranch(1) to compose a new revision from PKGBUILD, .SRCINFO and given additional files which will be placed on top of a dedicated distribution branch.


## OPTIONS

  - `-h`:
    Shows a summary of the options.

  - `-p` _buildscript_:
    Select an alternative file to include instead of _PKGBUILD_.

    This file will be stored to the distribution branch under the name _PKGBUILD_, regardless of its original name. It will also be used to generate the .SRCINFO from to store beside it.

    If an argument provides a file with the name _PKGBUILD_, it overwrites the choice of this option in regards of storage to the branch. The file chosen with this option will then only be used to generate the .SRCINFO, unless `-i` is also given.

  - `-i` _info_:
    Select a file to include as _.SRCINFO_, instead of automatically generating one from the _PKGBUILD_ using mksrcinfo.

    This file will be stored to the distribution branch under the name _.SRCINFO_, regardless of its original name. Its _pkgbase_ variable will determine the branch name.

    If an argument provides a file with the name _.SRCINFO_, it overwrites the choice of this option in regards of storage to the branch. The file chosen with this option will then only be queried for its _pkgbase_.

When using one of these options with files automatically generated from original _PKGBUILD_ or _.SRCINFO_ tracked by the repository, it is recommended to also specify these prefixed by a colon so offbranch(1) considers it when proposing a commit message.

Apart from these, all options offbranch(1) accepts are also valid to aurbranch, except `-b`. They will be passed through.

All file names are handed over to offbranch(1), accompanied by the PKGBUILD and .SRCINFO. This provides the full range of features such as renaming files and specifying paths to search for a commit message. For more details, see the offbranch(1) manual page, especially the section about "Renaming Files".

All symbolic links that appear in the arguments or files read by the `-l` and `-z` options are resolved and replaced with their link targets. That means that it is even safe to include links pointing outside the repository, as only the content will be commited instead of the plain link reference.


## BRANCH SETUP

If the program is executed in the root of a git repository, the branch will be called _aur_. If it is instead run from a directory deeper within a repository, a PKGBUILD collection repository is assumed and the branch will be named by the pkgbase variable as denoted by the PKGBUILD, prefixed with _aur/_. This way, all distribution branches are recognizable by a common naming scheme.

If the branch does not yet exist, a remote will be set up in addition to its creation. The branch is configured to track the _master_ of the respective package repository. Be wary of using the --all option to git push ever afterwards, because this pushes all branches to the specified remote, or origin, disregarding any of this configuration.


## EXIT STATUS

When offbranch(1) completed successfully, aurbranch will return with code 0. Note that this is also the case when the commit was purposely aborted by an empty commit message or the editor did not even come up because no changes were introduced.

Error codes greater than 7 indicating offbranch(1) failure are directly passed through.

Any remaining error is indicated by an exit code of 1 or greater. This mostly happens when something is wrong with the arguments.


## BUGS

This project was created by XZS <d.f.fischer@web.de> and [lives at GitHub](http://github.com/dffischer/pkgrepotools). Bugs can be filed in [the tracker found there](http://github.com/dffischer/pkgrepotools/issues).


## SEE ALSO

offbranch(1), mksrcinfo, git(1).
