graph-templates(1) -- graph template include relations
======================================================

## SYNOPSIS

`graph-template` [-h] [files]


## DESCRIPTION

This program will look up which templates the given files include and output this relation in graphiz' dot format.

Templates, recognized by their file extension are colored gray.

If no files are given, all files names PKGBUILD or ending with .template will be considered instead.


## BUGS

This project was created by XZS <d.f.fischer@web.de> and [lives at GitHub](http://github.com/dffischer/makepkg-expanded). Bugs can be filed in [the tracker found there](http://github.com/dffischer/makepkg-expanded/issues).


## SEE ALSO

makepkg-template(1), graphiz(7).
