# pkgrepotools

Tired of all the clutter _makepkg-template_ causes in your PKGBUILD? Use makepkg-expanded to build them leaving the template markers exactly as you wrote them. It will
- build a package without any change to the PKGBUILD,
- only propagating changes caused by a `pkgver` function and
- batch build all your packages in one go, also preparing them for AUR upload.
- It also is aware of git and can use a directory of repository-wide templates.

For more details and options, see [the manual page](makepkg-expanded.md).

Many functionalities are also available as separate or additional tools and can ease life dealing with PKGBUILDs and the AUR even without templates.
- [cp-pkgver](cp-pkgver.md) copies version variables from one PKGBUILD to another. It can be used by makepkg-expanded to propagate changes from the expanded variant back to the original script.
- [clean-templates](clean-templates.md) removes template markers, mode lines and duplicate templates to make the result looks more like an ordinary PKGBUILD. It is automatically invoked my makepkg-expanded by default.
- [graph-templates](graph-templates.md) helps to keep an overview of templates, build scripts and their relations. A [gvpr](http://www.graphviz.org/pdf/gvpr.1.pdf) [script](reachable.gvpr) is also included to restrict the graph to reachable nodes, which represent actually used templates.
- [aurbranch](aurbranch.md) commits the PKGBUILD and related files to a dedicated branch suitable to push to the AUR. It can not only be used directly in [the `-r` option of makepkg-expanded](makepkg-expanded.md#OPTIONS), but is generally usable when a slightly changed variant of a repository should be pushed to the AUR.
- [auradopt](aurremote.md) helps to prepare such a branch with an existing history.
- [offbranch](offbranch.md) is a tool to allow commits to a git branch without checking it out. It provides the Git-centric functionalities _aurbranch_ uses, but is completely agnostic of any AUR or pacman related specialities as is
- [offremote](offremote.md) which prepares remotes tracking exactly one branch of another repository. It is normally invoked automatically by _aurbranch_ when it creates a new distribution branch and used by aurremote.

You can learn about how to use all of these in concert with [this tutorial](tutorial.md).


## Installation

As it is probably only useful in [Arch Linux](https://archlinux.org/), anyways, [go grab it from the AUR](http://aur.archlinux.org/packages/pkgrepotools-git/).

To use the [PKGBUILD](PKGBUILD) as it is kept in this repository, [the makepkg-template for git](https://github.com/dffischer/git-makepkg-template) has to be installed.


## Dependencies

To use _makepkg-expanded_, only pacman is needed, as it includes _makepkg-template_. For _aurbranch_ to automatically generate _.SRCINFO_ files, _makepkg_ is needed, which is also included in pacman.

The repository-oriented tools _offbranch_, _aurbranch_ and _aurremote_ need git.

The output of _graph-templates_ can be processed further using _graphviz_, which can also execute _reachable.gvpr_.

The rest, _cp-pkgver_ and _clean-templates_, function even just with bash and sed.
