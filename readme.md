
## Install
 - clone repo: `git clone git@github.com:alek13/releaser.git` or [download](https://github.com/alek13/releaser/archive/master.zip) scripts
 - add the folder into `$PATH` var

## Configure
 - in root directory of your project create file `.gh-release`
 - add config vars in format `var=value`

## Config variables

variable | opt | default | description
-------- | --- | ------- | -----------
remote        | required | tries to detect `origin` | Name of remote to push. Typically `origin`.
changeLogFile | optional | detects `CHANGELOG.md` or `changelog.md` | Name of change log file.
token         | required |   | Access token.
