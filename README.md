This is where I store my additions to cgit. I haven't sent them
upstream because they're ugly hacks.

Beware: I plan to rebase each feature branch at the time of every
upstream release.

github-tab
----------

This patch, stored in the `github-tab` branch, adds a github tab to each
cgit page.

The configuration is pretty simple:

    enable_github_tab=1
    github_user=ausbin

See `cgitrc(5)` for more information.
