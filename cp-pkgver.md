cp-pkgver(1) -- copy pkgver between PKGBUILDs
=============================================

## SYNOPSIS

`cp-pkver` [-h] <source> <destination>


## DESCRIPTION

Copies the value of the `pkgver` and `pkgrel` variables from one PKGBUILD to another.

When both fields already match in the given files, the access time of <destination> will not change.


## EXIT STATUS

The program exits with status 0, after both PKGBUILDs have the same version fields. This is the case on successful copy or when no update was necessary to begin with.

An exit code of 1 indicates that something was wrong with the arguments.

When <destination> can not be written, 2 is returned.


## BUGS

This project was created by XZS <d.f.fischer@web.de> and [lives at GitHub](http://github.com/dffischer/pkgrepotools). Bugs can be filed in [the tracker found there](http://github.com/dffischer/pkgrepotools/issues).


## SEE ALSO

PKGBUILD(1).
