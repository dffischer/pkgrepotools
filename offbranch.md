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

  - `-n` _number_:
    Increase the number of commit messages to propose. They are searched for backwards through the history, newest first.

  - `-m` _message_:
    Pass a message to use for the commit created.

    The editor is still brought up. This can be avoided by additionally setting the environment variable GIT_EDITOR to some no-operation like : .

    If this option is given multiple times, only the last one is effective. It will overwrite any `-n` option.

  - `-l` _file_:
    Read additional file names from a file, one per line.

    Files can be renamed or only considered for the commit message as if they would be encountered as command line arguments.

    This option is most useful when the output of file-listing commands is piped to it. For example, connecting find(1) to it makes it possible to add a filtered list of filenames. Inserting an xargs(1) to the pipeline would serve the same aim, but could splinter into multiple calls to offbranch, leading to multiple commits, which is normally not the intended result.

    Reading from the standard input is possible by explicitly passing /dev/stdin, but be aware that you most probably want this input to be connected to a terminal for the commit message editor to receive. To use a file list while still interactively reviewing the commit use Process Substitution if supported by your shell or fall back to a pipe.

All other arguments are treated as further files to include when composing the branch. Directories will be descended into as if all files recursively found in there were specified.

The message of the last commit affecting any of these files, viewed from the current HEAD downwards, will be proposed as the description of the commit to be newly generated on the distribution branch. If any of these files are not under version control or unknown to the current branch, they will be silently ignored.


### Renaming Files

It is possible to add files under a different name with the syntax _path:name_. If any argument contains a colon, the part before it is considered as the name of the file to include with the name taken from the part after the colon. This new name may also be a full path, simulating subdirectories in the distribution branch. If the path contains multiple colons, only the first one is used for separation. All further ones are considered part of the new name.

If no name is given, but the path ends with the separator, the file is not included, but considered when looking for a commit message to propose. This can be used when a file is generated from other files under version control, but not tracked itself. The files used to generate can then be passed like this to exclude them from the branch but use them to explain changes in the generated result.

When looking for a commit message, files not tracked by the current git repository are silently ignored.


## EXIT STATUS

Exit codes below 8 indicate successful termination, possibly as requested by user interaction. The numbers in braces will be returned on the respective events, when

  - a commit was successfully created from the given file and commit message (0),

  - a branch was created by its first commit (2),

  - the user aborted the process through an empty commit message (1) and also

  - when no commit was necessary to begin with because nothing changed (3).

Exit codes higher than 7 are not changed by the -q flag and indicate errors in program invocation.

  - When offbranch is called with invalid parameters, including nonexistent options or missing files, it will exit with status 8.

  - Running it outside of a git repository will lead to exit code 9.


## BUGS

Files residing in repositories other than the one including the current working directory are not scanned for commit messages to propose. This could be useful for example when branch contents are to be composed from data scattered across multiple repositories or utilizes templates from elsewhere. To implement this, simply calling git log to find the last relevant message is not enough any more. The files would have to be assorted by repository, then the last message would have to be collected from all of them, each including a timestamp to sort them by, so that the most recent one can be used.

This project was created by XZS <d.f.fischer@web.de> and [lives at GitHub](http://github.com/dffischer/pkgrepotools). Bugs can be filed in [the tracker found there](http://github.com/dffischer/pkgrepotools/issues).


## SEE ALSO

git(1).
