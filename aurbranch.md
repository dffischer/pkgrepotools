aurbranch(1) -- maintain a git branch for AUR distibution
=========================================================

## SYNOPSIS

`aurbranch` [`options`] <additional files>


## DESCRIPTION

This program helps to create and update a branch in a local git repository to be pushed to the Arch User Repository.

It automatically generates a .SRCINFO file from a PKGBUILD in the local working directory and commits changes in both on top of a dedicated branch found in the git repository it is run from within. This branch is named like the pkgbase variable as denoted by the PKGBUILD, prefixed with "aur/". If this branch does not exist, it will be created.

All this is done without altering the state of the repository or its working tree and thus save to run in dirty worktrees.


## OPTIONS

  - `-h`:
    Shows a summary of the options.

  - `-p` _buildscript_:
    Select an alternative file to include instead of _PKGBUILD_.

    This file will be stored to the distribution branch under the name _PKGBUILD_, regardless of its original name. It will also be used to generate the .SRCINFO from to store beside it.

    If a file named PKGBUILD is also specified as additional file, it will be ignored in favor of this file. However, it will still be considered when changed files are searched to propose a commit message.

All other arguments are treated as further files to include when composing the branch.

The message of the last commit affecting any of these files, viewed from the current HEAD downwards, will be proposed as the description of the commit to be newly generated on the distribution branch. If any of these files are not under version control or unknown to the current branch, they will be silently ignored.

It is possible to add files under a different name with the syntax _path:name_. If any argument contains a colon, the part before it is considered as the name of the file to include with the name taken from the part after the colon. This new name may also be a full path, simulating subdirectories in the distribution branch. If the path contains multiple colons, only the first one is used for separation. All further ones are considered part of the new name.


## BUGS

This project was created by XZS <d.f.fischer@web.de> and [lives at GitHub](http://github.com/dffischer/makepkg-expanded). Bugs can be filed in [the tracker found there](http://github.com/dffischer/makepkg-expanded/issues).


## SEE ALSO

git(1), mksrcinfo.
