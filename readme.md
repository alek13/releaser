# GitHub Releaser

## Install
 - clone repo: `git clone git@github.com:alek13/releaser.git` or [download](https://github.com/alek13/releaser/archive/master.zip) scripts
 - add the folder into `$PATH` var

## Configure
 - in root directory of your project create file `.gh-release`
 - add config vars in format `var=value`

## Config variables

variable      | opt              | default                  | description
------------- | ---------------- | ------------------------ | --------------------------------------------------------
remote        | required         | tries to detect `origin` | Name of remote to push. Typically `origin`.
changeLogFile | optional         | detects `CHANGELOG.md` or `changelog.md` | Name of change log file.
token         | required for `gh-release create` |          | Access token.
submodulesLog | optional         | `0`                      | If enabled (==`1`), `gh-release prepare` also adds a changelog entry with commit messages of every submodule that changed since the last tag.

## Usage

Run `gh-release` in the root of your project to see **available commands**:

```bash
$ gh-release 

gh-release - make release easier

Usage:
    gh-release <command> { [options] [arguments] | --help }

Commands:
    prepare     Collect commit messages and prepend to ChangLog file
    version     Creates bump-commit & specified tag
    create      Creates release on GitHub
```

### Then just run commands step-by-step:

- [First `gh-release prepare`](#gh-release-prepare)
- [Then `gh-release version`](#gh-release-version)
- [Finally `gh-release create`](#gh-release-create)

#### `gh-release prepare`

This command will:
1. - collect commit messages from latest tag to current latest commit (or to specified commit in '-c' option)
   - show collected messages
   - _ask continue with prepend to changelog file_

2. - prepend collected messages to changelog file

After running this command you can review and prettify the changelog file.

```bash
$ gh-release prepare --help

gh-release prepare - Collect commit messages and prepend to ChangLog file.
    It takes all commits from latest tag to current latest commit (or to specified commit in '-c' option).
    Before continue with 'gh-release version' you can edit your prepared ChangLog file.

Usage:
    gh-release prepare [{-c|--commit} <commit_hash>] [{-v|--version|-t|--tag} <tag>]

Options:
    -c|--commit            <commit_hash>       [default: latest ] The commit to take to.
    -v|--version|-t|--tag  <tag>               [default: YYYY.MM] Specified tag will be used to generate link in ChangeLog file.

```

---

#### `gh-release version`

> **Note**: This command should be used after `gh-release prepare` and after your own edits of ChangLog file.

This command will:
1. - show info about version and commit message
   - _ask continue_

2. - add changelog file to git
   - show git status and diff

3. - _ask continue with committing_
   - commit with specified message
   
4. - show remote to push to
   - _ask continue with pushing_
   - push to remote

```bash
$ gh-release version --help

gh-release version - Creates bump-commit & specified tag.
    Use this command after 'gh-release prepare' and after your own edits of ChangLog file.

Usage:
    gh-release version [{-c|--commit} <commit_hash>] [{-m|--message} <commit_message>] [{-v|--version|-t|--tag} <tag>]

Options:
    -c|--commit            <commit_hash>        [default: latest ] The commit on which the tag will be set.

    -m|--message           <commit_message>     [default: 'bump version. Release %s' - if NO commit was specified]
                                                [     or: 'add %s changelog'         - if commit was specified   ]
                                                                   The message of bump commit.
                                                                   You can use %s for insert version into message.

    -v|--version|-t|--tag  <tag>                [default: YYYY.MM] Version tag to create.

```

---

#### `gh-release create`

Just creates release on GitHub as draft.

In release description will be:
- title "Change Log"
- changelog
- links:
  - "View full changes" - to changelog file
  - "View commits" - to commits between latest and previous tags

After that you can edit release on GitHub or just publish it.

```bash
$ gh-release create --help

gh-release create - Creates release on GitHub.
    It takes last two tags and create template on GitHub.
    After edit release on GitHub (add ChangLog) and publish it.

Usage:
    gh-release create
```
