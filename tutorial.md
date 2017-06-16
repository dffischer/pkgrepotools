The tools in this suite help to build and prepare for AUR upload even complicated repository setups with additional files, makepkg-templates and rewrites. This tutorial explains some practical situation where they can be of aid.


# AUR Distribution

## Distribution Branch Creation

Suppose a repository that provides a PKGBUILD stored alongside all other project data.

```
examplepackage
├── Makefile
├── PKGBUILD
├── README.md
└── src
    ├── main.c
    ├── Makefile
    └── ...
```

Only the _PKGBUILD_ is relevant for distribution to the AUR. A _.SRCINFO_ is also required for that, but it can be generated from the _PKGBUILD_, so tracking it on the project's main branch would be redundant. To prepare this repository for AUR upload, simply execute [_aurbranch_](aurbranch.md).

```bash
> aurbranch
creating refs/heads/aur
```

It will then ask for a commit message, generate the _.SRCINFO_ from the _PKGBUILD_ and commits them as the first revision of a branch named _aur_. As this branch did not previously exist, it also sets up a remote named exactly like the package (_examplepackage_ in this case) and configure the branch to track the master branch of this remote. So consequently, uploading can be invoked with

```bash
> git push aur
```

Whenever a new release should be distributed to the AUR, just repeat these two commands.


## Adding Files

A few revisions later, an installation script made its way into the repository, which is referred in the _PKGBUILD_ by `install=examplepackage.install`. A desktop entry was also added.

```
examplepackage
├── examplepackage.install
├── examplepackage.desktop
├── Makefile
├── PKGBUILD
├── README.md
└── src
    ├── main.c
    ├── Makefile
    └── ...
```

Both are needed by the _PKGBUILD_ and should thus be included in the AUR branch. To achieve this, they are simply added to the aurbranch command line.

```bash
> aurbranch examplepackage.*
```


As the _PKGBUILD_ gets more complex, automatically generating the _.SRCINFO_ file with _makepkg --printsrcinfo_ program is not sufficient any longer. It has to be edited manually and thus is now included in the main branch.

```
examplepackage
├── examplepackage.install
├── examplepackage.desktop
├── Makefile
├── PKGBUILD
├── README.md
├── src
│   ├── main.c
│   ├── Makefile
│   └── ...
└── .SRCINFO
```

To stop _aurbranch_ from generating it and instead make it use the existing one, add it to the command line, too.

```bash
> aurbranch examplepackage.* .SRCINFO
```


To make it more visible, you may want to keep the file without its leading dot and rename while composing the revision, which you can do with a colon.

```bash
> aurbranch examplepackage.* SRCINFO:.SRCINFO
```


With complex command lines like these, it may be useful to store them in a little script inside your repository. Then your co-maintainers also see how to upload new versions.

```bash
> echo 'aurbranch examplepackage.* SRCINFO:.SRCINFO' > distribute
> chmod +x distribute
> git add distribute
> git commit -m 'add AUR upload directive'
```


## Package Collections

Many maintainer keep related _PKGBUILD_s collected in a central repository. Usually, files specific to a package are kept in a directory named after the corresponding _pkgbase_. Common files reside outside.

```
collection
├── desktop.install
├── one
│   ├── PKGBUILD
│   ├── one.desktop
│   └── desktop.install -> ../desktop.install
├── two
│   ├── PKGBUILD
│   ├── two.desktop
│   └── desktop.install -> ../desktop.install
└── three
    └── PKGBUILD
```

The _aurbranch_ helper is aware of this setup. When run from within a subdirectory of the repository it places all paths as seen from this perspective and creates branches and remotes named _aur/pkgbase_ with the _pkgbase_ extracted from the _PKGBUILD_. So new revisions for the distribution branches _aur/one_, _aur/two_ and _aur/three_ can be composed and sent like so.

```bash
> ( cd one; aurbranch one.desktop one.install )
> ( cd two; aurbranch two.desktop two.install )
> ( cd three; aurbranch )
> git push aur/one ; git push aur/two ; git push aur/three
```


As renaming on the fly can also change paths, the symbolic links are not even needed.

```bash
> ( cd one; aurbranch one.desktop ../desktop.install:desktop.install )
> ( cd two; aurbranch two.desktop ../desktop.install:desktop.install )
> ( cd three; aurbranch )
```


# Templates

## Setup

As _PKGBUILD_s in such collections often share code parts, it can be wise to externalize them in templates. A template marker like `# template input; name=build` takes their place in the _PKGBUILD_.

```
collection
├── build-1.template
├── desktop-1.template
├── desktop.install
├── one
│   ├── PKGBUILD
│   └── one.desktop
├── two
│   ├── PKGBUILD
│   └── two.desktop
└── three
    └── PKGBUILD
```

Before _makepkg_ is able to work with these now, executing `makepkg-template -t ..` has to be called from inside the directories to insert the content of the template into the file again, surrounded by `# template begin ...` and `template end ...` markers. But adding this expanded variant to the repository not only replicates code that is already contained in the templates, but also causes the _PKGBUILD_s to change whenever their templates change. It would be better to just keep the `# template input; ...` lines in the repository as they were, but have a command that can build the package from there.


## Building

This is exactly what [_makepkg-expanded_](makepkg-expanded.md) does. It just needs to be told where to find the templates.

```bash
> ( cd one; makepkg-expanded -t .. )
==> Making package: one ...
```


When moved to a common folder named _makepkg-templates_ in the repository root, even this option can be omitted.

```
collection
├── desktop.install
├── makepkg-templates
│   ├── build-1.template
│   └── desktop-1.template
├── one
│   ├── PKGBUILD
│   └── one.desktop
├── two
│   ├── PKGBUILD
│   └── two.desktop
└── three
    └── PKGBUILD
```


And another flag builds all _PKGBUILD_s that can be found at once with

```bash
makepkg-expanded -a
```


Apart from the package and intermediate files _makepkg_ leaves, this also produces a temporary file _PKGBUILD.expanded_ besides each original _PKGBUILD_s, which you may want to add to your _.gitignore_ as well as the _.SRCINFO_ automatically generated by _aurbranch_.


## Distribution

A _PKGBUILD_ uploaded to the AUR should be self-contained and not rely on any external templates. The _.SRCINFO_ should also be created only from the expanded _PKGBUILD_. So expansion should also happen before throwing in _aurbranch_. Luckily, _makepkg-expanded_ can not only invoke _makepkg_, but run any program, even arbitrary shell code.

```bash
makepkg-expanded -r 'aurbranch -p "$1" *' -a
```

A few things are automatically taken care of by _makepkg-expanded_ here. Firstly, [clean-templates](clean-templates.md) strips the resulting _PKGBUILD_ from template markers and similar redundant content for better readability so that it looks fine for anyone inspecting your package from the AUR web interface. The wild card `*` in snippet passed with the `-r` option will automatically exclude the original _PKGBUILD_, and the `-p` option will direct _aurbranch_ to use the expanded _PKGBUILD_ instead of the original.


You may have noticed that _aurbranch_ proposes a message to describe the commit to the AUR distribution branch with. This will be taken the last commit message affecting any of the files composing the commit. But it does not know that the _PKGBUILD_ it uses here was generated from the templates and thus ignore changes in the templates. It can be told to consider them for the message, but not actually add them, by passing them in with an empty name.

```bash
makepkg-expanded -r 'aurbranch -p "$1" ../makepkg-templates: *' -a
```

Note that their path has to be specified relative to the directories the _PKGBUILD_s reside is, as _aurbranch_ is run from there. Again _makepkg-expanded_ includes automatisms to ease life here. It passes the name of the expanded _PKGBUILD_, the corresponding original and all the template directories to the code snippet as parameters. They just have to be prefixed with a colon, which some bash trickery can do.

```bash
makepkg-expanded -r 'aurbranch -p "$1" ${@/%/:} *' -a
```


There is only one caveat left: When there are any stray untracked files in the directory, the wild card catches them, too. While testing, this is most probably the case for the _package.tar.xz_, the _src_ and _pkg_ directories and any sources left from building the package. So it can be useful to restrict `*` to files tracked by Git. This can be done by filtering it in Git's _ls-files_.

```bash
makepkg-expanded -r 'aurbranch -p "$1" ${@/%/:} $(git ls-files *)' -a
```

This command for the `-r` option is that commonly useful that makepkg-expanded provides `-b` as a shorthand for it.


# Adoption

When adopting a package, it comes with a history to build up on. Especially when integrating a newly adopted package into a collection, it should be pulled into the repository as a distribution branch so that _aurbranch_ can add further commits on top of it. [The _auradopt_ program](auradopt.md) can configure branch and remote. When creating a branch anew, it will also pull in the history from the remote it set up.

```bash
auradopt package-name
```


For more complicated project configurations, you may want to look at [_offbranch_](offbranch.md) which is used by _aurbranch_ internally to commit the files to a branch not checked out.
