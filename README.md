what is this?
-------------

this is where I store my changes to the `cgit` package in debian
jessie (currently v0.10.2).

why?
----

I wanted to add some ugly hacks and backport some new features to the
cgit debian package. Neither belong upstream, so I've posted them here.

which hacks?
------------

I've put each change in a separate branch:

 1. `github`, which adds a github tab to each cgit page
 2. `deb-ignore-hide`, a backport of `repo.{hide,ignore}` for
    `/etc/cgitrc`. To resolve a merge conflict, it includes Nicolas
    Dandrimont's `fix_status_code_for_unknown_repos` patch from the
    debian package.

how do i use it?
----------------

again, this is for debian jessie because i'm selfish.
i learned most of this from https://wiki.debian.org/BuildingTutorial,
which might help if my directions are confusing.

 1. clone this absolutely wicked repo and make the patches:

        $ git clone https://code.austinjadams.com/cgit
        $ git diff jessie..github >github
        $ git diff jessie..deb-ignore-hide >ignore-hide

 2. download the source, probably in a different directory (make sure
    you have `apt-get update`d after adding the proper `deb-src` line in
    `/etc/apt/sources.list` or a file in `/etc/apt/sources.list.d`):

        $ apt-get source cgit
        $ cd cgit-XXXXX

 3. add the patches and if you're using the `deb-ignore-hide` patch,
    comment the consequently redundant
    `fix_status_code_for_unknown_repos` line:

        $ cd debian/patches/
        $ cp $the_other_directory/{github,ignore-hide} .
        $ ed series
        # The patch files of the -p1 format included in the debian/patches directory
        # are applied to the upstream source in the order listed below.
        # This is manually managed by users with dquilt (quilt(1) wrapper) etc.
        debianize_makefile
        hardening
        github
        ignore-hide
        #fix_status_code_for_unknown_repos
        $ cd ../../

 4. bump the package version:

        $ dch -n

 5. build it:

        $ debuild -b -uc -us

 6. install your new package:

        # dpkg -i ../cgit_*.deb

    you're done! nice work

how do i configure this?
------------------------

in `/etc/cgitrc` like anything else:

    # enable github tab
    enable_github_tab=1
    github_user=ausbin

    ...

    # example of repo.hide
    repo.url=foo
    repo.desc=show off repo.hide
    repo.path=/home/jane/foo.git
    repo.hide=1

see `cgitrc(5)` for more information.
