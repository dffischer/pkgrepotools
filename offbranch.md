offbranch(1) -- commit files off the current branch
===================================================

## SYNOPSIS

`offbranch` [`options`] [file[:name] ...]


## DESCRIPTION

This program generates a Git revision from the given files and commits it to a branch in a local git repository.

All this is done without altering the state of the repository or its working tree and thus save to run in a dirty working tree.


## OPTIONS

  - `-h`:
    Shows a summary of the options.

  - `-s` _separator_:
    Change the separator for renaming.

    Choosing an alternate string to tell apart the real file path from the name it is added to the distribution branch allows specifying file names that contain a colon.

    This option is not restricted to a single character, whole strings are also valid separators. If you intend to include weird symbols here, be aware that they are used for prefix and suffix removal using bash parameter expansion and make yourself familiar with the according section in the bash(1) manual page.

  - `-b` _branch_:
    Specify the branch to commit to, instead of _offbranch_. If this branch does not exist yet, it will be created.

All other arguments are treated as further files to include when composing the branch.

The message of the last commit affecting any of these files, viewed from the current HEAD downwards, will be proposed as the description of the commit to be newly generated on the distribution branch. If any of these files are not under version control or unknown to the current branch, they will be silently ignored.

It is possible to add files under a different name with the syntax _path:name_. If any argument contains a colon, the part before it is considered as the name of the file to include with the name taken from the part after the colon. This new name may also be a full path, simulating subdirectories in the distribution branch. If the path contains multiple colons, only the first one is used for separation. All further ones are considered part of the new name.


## BUGS

This project was created by XZS <d.f.fischer@web.de> and [lives at GitHub](http://github.com/dffischer/makepkg-expanded). Bugs can be filed in [the tracker found there](http://github.com/dffischer/makepkg-expanded/issues).


## SEE ALSO

git(1).