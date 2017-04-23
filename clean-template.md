clean-template(1) -- build script beautifier
============================================

## SYNOPSIS

`clean-template` [ `options` ] [_`file`_]


## DESCRIPTION

This program cleans every PKGBUILD(5) it is passed from some of the chaos makepkg-template(1) is likely to leave in it and makes it look more like a PKGBUILD written from slate. It does so by

- removing Vim mode lines,

  If they are preceeded by an empty line, it is also removed.

- removing all template markers and

- suppressing duplicate templates.

  Only the first occurrence of each template, identified by its name, will be left intact. All further occurrences will be removed, including any templates it includes in turn.


## OPTIONS

  If no file is given, clean-template reads from the standard input.

  - `-h`:
    Shows a summary of the options.

  - `-m`:
    Only remove duplicates, leave mode lines and markers of kept templates intact.

  Note that there is no option to just remove template markers and modelines while leaving duplicates alone. This is beacause the same task can be performed more efficiently by the one-liner `sed '/# vim/d;x;$G;/# template/d;1d'`.


## EXIT STATUS

The program always exits with status 0 on a well-written PKGBUILD.

If a template marker is opened buy not closed, indicated by a "template start" without a corresponding "template end", this is indicated by exit code 1. In this case, correct cleaning of the script can not be assumed.


## BUGS

Not all such broken templates lead to errors. They only cause an error if they are part of or delimit a template that is to be removed in the curse of deduplication. Thus, clean-template cannot be used to check template marker validity. This is intentional to speed up processing.

This project was created by XZS <d.f.fischer@web.de> and [lives at GitHub](http://github.com/dffischer/pkgrepotools). Bugs can be filed in [the tracker found there](http://github.com/dffischer/pkgrepotools/issues).


## SEE ALSO

makepkg-template(1), makepkg(1).
