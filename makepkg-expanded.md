makepkg-expanded(1) -- process template expanded PKGBUILDs
==========================================================

## SYNOPSIS

`makepkg-expanded` [ `options` ]


## DESCRIPTION

This is a wrapper around makepkg-template(1) and makepkg(1) that can be used to build and distribute packages without touching templates in the original PKGBUILD.

It does so by expanding the templates in a temporary file besides the original which is then cleaned and used for further processing. This leaves the template markers as they are while keeping the AUR free from template relics.


## OPTIONS

  - `-h`:
    Shows a summary of the options.

  - `-p` _buildscript_:
    Select an alternative file to process instead of _PKGBUILD_.

    Unlike makepkg, files outside the current working directory will not be rejected. Instead, any processing will be done from within the directory the given file resides in.

    Also unlike makepkg, this option may be given multiple times to batch build many packages with the same settings. However, the next option may be even more fit for this purpose.

  - `-a` [_pattern_]:
    Select all files matching the given pattern from the current working directory and below.

    The pattern is passed to find(1) and thus may use wildcards supported there.

    If no pattern is given, it is assumed to be _PKGBUILD_.

  - `-t` _dir_:
    Specify a directory to look up templates in. These will be passed on to makepkg-template(1) as _--template-dir_ options.

    If makepkg-template(1) supports it, this option may effectively be given multiple times. See [BUGS][] for details. As it is the case with makepkg-templates(1), the directory specified last will take precedence in case a template could be fetched from multiple of them.

    Without any `-t`, the system-wide default is used, which normally is _/usr/share/makepkg-template_. When makepkg-template is invoked from anywhere within a git(1) repository, it will also use templates from a _makepkg-templates_ directory located in the root of the repository, if such exists. Note that an explicit statement of `-t` overwrites these defaults. So if they shall be mixed with more directories, they have to be specified explicitely.

  - `-b` _suffix_:
    Specifies a suffix to prepend to the name of the original build script to compose the file name to store the intermediate expanded content to. They will always end up in the same directory as the original.

    The default is _.expanded_, so without any options, the script actually processed will be called _PKGBUILD.expanded_.

  - `-B`:
    Do not generate a source aurball, just build the given package(s).

    This suppresses the execution of `makepkg -S` so that makepkg(1) will only be invoked one time per package, to build it.

    As all unrecognized options are passed to makepkg(1), creating just the aurball without packaging beforehand can be achieved by passing `-BS`. However, this is discouraged as it would ignore any _pkgver_ function. Building the package first updates the version numbers so that the aurball will then contain current values.

  - `-u`:
    This option is passed to clean-template(1) which is invoked before packaging and creation of the source aurball to leave template markers and mode lines intact. Duplicate templates will still be removed.

    When the `-l` option is also specified, cleaning is skipped completely.

  - `-l`:
    Only remove mode lines and template markers instead when cleaning and leave duplicate templates untouched.

    This completely avoids clean-template invocation and instead replaces it with sed, as mentioned in the clean-template(1) manual page. When the `-u` option is also specified, cleaning is skipped completely.

  - `-E` _command_:
    Command to execute after expanding each PKGBUILD.

    The given snippet will be run through `bash -c` and receive the file names of the expanded and original PKGBUILD as its respective arguments, followed by any options that makepkg-expanded itself did not recognize. It will be run from the directory where the processed PKGBUILD resides in.

    If any execution yields an exit code other than 0, makepkg-expanded will abort instantly with the same exit code.

    The default command first invokes makepkg(1) on the expanded PKGBUILD, followed by makepkg -S, if no `-B` option was given and then uses cp-pkgver(1) to propagate any version updated back to the original if the former two completed successfully. This would be roughly equal to passing 'makepkg -p $1; makepkg -Sp $1; cp-pkgver $1 $2' with this option.

    Using a custom command will disable the passing of any further unrecognized options. But they can instead be included in the command string itself. Note that this option also renders `-B` useless.

All further options are directly passed through to every invocation of makepkg(1), apart from when an `-E` argument is present, in which case further arguments are silently ignored.


## EXIT STATUS

When the program exits with status 0, it is safe to assume that all PKGBUILDs were successfully processed. This means that all `-E` statements exited with code 0 or, in the absence of such, all packages and source tarballs were created.

When makepkg-template(1) fails on any PKGBUILD, the program immediatly halts passing through its exit code.

When the processing fails, the exit code of the code snippet is passed through. By default, failures in makepkg(1) are propagated while cp-pkver(1) may silently fail.


## BUGS

Specifying multiple template directories (the `-t` option) can only be done with a patched pacman installation. Without it, only the last `-t` option has any effect. This also affects the defaults. When run from within a git repository and a local template directory is present there, templates in the system default path will be neglected without the patch. Instructions on how to patch pacman can be found within [the source repository of makepkg-expanded](https://github.com/dffischer/makepkg-expanded/blob/master/patching-pacman.md)

This project was created by XZS <d.f.fischer@web.de> and [lives at GitHub](http://github.com/dffischer/makepkg-expanded). Bugs can be filed in [the tracker found there](http://github.com/dffischer/makepkg-expanded/issues).


## SEE ALSO

makepkg-template(1), makepkg(1), clean-template(1), cp-pkver(1).
