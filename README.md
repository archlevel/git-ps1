git-supp is a package of supplemental scripts and enhancements for Git. They
are intended to solve common issues or reduce repetitiveness for common
day-to-day Git tasks.

The name "git-supp" is not the name of a project. Rather, it is simply a name
given to the repository.


# Git PS1
git-ps1 is a Git-augmented PS1 for BASH. It uses colors and various identifiers
to display information about the status of the current branch, including the
branch name, staged and unstaged changes, untracked files, commits ahead of the
tracking branch and more.

## Setup
To use, modify your PS1 variable (e.g. in ~/.bashrc) to include the script:
  PS1="$PS1\$($( cat git-ps1.sh ))"

This script produces output in a number of different colors. To ensure the
prompt operates correctly, non-printable characters must be escaped (\[\]).
However, these characters are not recognized when output from an external
script. Therefore, the script must be inserted directly into the PS1 string,
where echoing the escape sequence will produce the intended result.

## Configuration
All configuration is done via `GITPS1_*` environment variables.

Indicators (set to '0' to disable)

* `GITPS1_IND_STAGED`      - Staged changes
* `GITPS1_IND_UNSTAGED`    - Unstaged changes
* `GITPS1_IND_UNTRACKED`   - Untracked files
* `GITPS1_IND_AHEAD`       - Ahead of tracking branch
* `GITPS1_IND_AHEAD_COUNT` - Whether to display number of commits ahead (e.g. @5)
* `GITPS1_IND_STATE`       - Whether to display state string (see git-supp)

Colors:

* `GITPS1_COLOR_DEFAULT`   - Default color, used to display brackets and branch
* `GITPS1_COLOR_FASTFWD`   - Color used for fast-forward indicator and used to
                             display brackets and hash when not on a branch
* `GITPS1_COLOR_STAGED`    - Color used to for staged changes indicator
* `GITPS1_COLOR_UNTRACKED` - Color used for untracked files indicator
* `GITPS1_COLOR_UNSTAGED`  - Color used for unstaged changes indicator
* `GITPS1_COLOR_AHEAD`     - Color used for ahead indicator (ahead of tracking)
* `GITPS1_COLOR_STATE`     - Color used for state string (see git-supp)


# shortmaps / BASH Completion
The `bash_completion` file contains BASH completion for custom commands and
"shortmappings", which provide single-character aliases to common Git commands.

## Setup
Source the `bash_completion` file (e.g. place in `.bashrc` or in
`/etc/bash_completion.d/` on Debian systems), with the path to the provided
`shortmaps` file as the only argument:

```
$ . bash_completion ./shortmaps
```

You may also add your own mappings to `~/.git-ps1-shortmaps`.

## Configuration
The file format is as follows:

```
KEY COMPLETION :CMD
KEY COMPLETION CMD
```

If `CMD` contains a `:` prefix, the command will be prefixed with `git`.


# git state
Adds the concept of "states" to branches. The state, which is represented as a
string, can be assigned to a branch and will be prepended to any commit on that
branch. Distinct states can be assigned to separate branches.

This concept is intended to aid in the following scenarios:

* Branches are often used to identify a certain feature or fix. However, once
  the branch is deleted, the only remaining identifying information is the merge
  commit. States allow a specific string (e.g. the bug number) to be prepended
  to each commit message automatically, which may be otherwise forgotten or
  infrequent.
* During large refactorings, one may need to commit during an unstable state in
  order to prevent one massive commit. However, this complicates operations
  like `git bisect`. One could use states to clearly mark each commit as
  unstable until the process is complete.
* The state can be used in conjunction with git-ps1 in order to clearly state
  the current state of the branch.

## Usage
```sh
$ git state foo     # sets the state to "foo"
$ git state         # retrieve the current state
foo
$ git state --clear # clear the state
```

Remember - states are tied to the current branch:

```sh
$ git state foo
$ git state
foo
$ git checkout -b newbranch
Switched to branch 'newbranch'
$ git state # no state
$ git state bar
$ git state
bar
$ git checkout master
Switched to branch 'master'
$ git state
foo
```

## Setup
Add the repository's `bin/` directory to your `PATH` environment variable, or
copy the script into your `PATH`.

