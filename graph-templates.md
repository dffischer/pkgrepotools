graph-templates(1) -- graph template include relations
======================================================

## SYNOPSIS

`graph-template` [`options` | build scripts | templates ...]


## DESCRIPTION

This program will look up which templates the given files include and output this relation in graphiz' dot format.

Build scripts can be recognized by their bold outline. They are told apart from templates by their file extension. Nodes with dashed outline are only found referenced in processed files but not given on the command line themselves. Including them could expand the graph further.

If no files are given, all files names PKGBUILD or ending with .template will be considered instead.


## OPTIONS

  - `-h`:
    Shows a summary of the options.

  - `-l` _file_:
    Read additional file names from a file, one per line.

    To read from the standard input, pass _-_ as a filename.

  - `-z` _file_:
    Read additional file names from a file like with `-l`, but delimit them by null characters.

    The standard input can be written as _-_. To choose only a subset of files to include in the graph among a large project, it may be useful to generate the input with another program, like e.g. `find` with the `-print0` action.


## BUGS

This project was created by XZS <d.f.fischer@web.de> and [lives at GitHub](http://github.com/dffischer/pkgrepotools). Bugs can be filed in [the tracker found there](http://github.com/dffischer/pkgrepotools/issues).


## SEE ALSO

makepkg-template(1), graphiz(7).
