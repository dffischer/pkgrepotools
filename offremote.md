offremote(1) -- track a branch of a foreign repository
======================================================

## SYNOPSIS

`offremote` [`options`] <url>


## DESCRIPTION

This program prepares a remote associating a exactly one local branch with a remote branch.

It is mostly used to port possibly modified parts of the local histroy to a remote repository not usually pushed to using tools like offbranch(1) or incorporating remote history into local branches.


## OPTIONS

  - `-h`:
    Shows a summary of the options.

  - `-n` _name_:
    Specify the remote and branch name, instead of the name of the remote repository.


## BUGS

This project was created by XZS <d.f.fischer@web.de> and [lives at GitHub](http://github.com/dffischer/pkgrepotools). Bugs can be filed in [the tracker found there](http://github.com/dffischer/pkgrepotools/issues).


## SEE ALSO

offbranch(1), git(1).
