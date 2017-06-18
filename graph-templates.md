graph-templates(1) -- graph template include relations
======================================================

## SYNOPSIS

`graph-template` [`options` | build scripts | templates ...]


## DESCRIPTION

This program will look up which templates the given files include and output this relation in graphiz' dot format.

Templates, recognized by their file extension are colored gray.

If no files are given, all files names PKGBUILD or ending with .template will be considered instead.


## OPTIONS

  - `-h`:
    Shows a summary of the options.


## BUGS

This project was created by XZS <d.f.fischer@web.de> and [lives at GitHub](http://github.com/dffischer/pkgrepotools). Bugs can be filed in [the tracker found there](http://github.com/dffischer/pkgrepotools/issues).


## SEE ALSO

makepkg-template(1), graphiz(7).
