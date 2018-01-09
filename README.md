CAHHome
=======

My dotfiles and shell configuration.

Structure
---------

```
─ $HOME/
    ├── .cahhome/
    │   ├── bin/
    │   ├── generated/
    │   ├── modules/
    │   │   ├── bin/
    │   │   ├── dotfiles/
    │   │   ├── zshrc
    │   │   ├── profile
    │   │   ├── crontab
    │   │   └── update
    │   ├── readmes/
    │   ├── profile
    │   ├── vimrc
    │   ├── zshrc
    │   └── zshrc_update
    ├── .CAHHOME
    ├── .host/
    │   ├── config/
    │   ├── crontab
    │   ├── profile
    │   ├── ssh_config
    │   ├── zshrc_pre
    │   └── zshrc_post
    │
```

### `$HOME/.cahhome` folder

This is where you should clone this repo, and where all of the important bits live.  This is often referred to (here and in the code) as `$CAHHOME`.

#### `$CAHHOME/bin` folder

Executables directly related to managing/executing CAHHome.

This folder will be added to the path.

#### `$CAHHOME/generated` folder

Files that are generated as part of CAHHome operation will be stored here.  That includes files from the *Generated Files* section as well as timestamps to handle periodically executed commands (such as updates and fortunes).

#### `$CAHHOME/modules` folder

Modules allow grouping of similar functionality in a common way.   Modules will be processed in lexicographical order (as returned by `ls -1`), use that fact to model dependencies between modules.

Each module should contain the following information:

##### `.../00Module/bin` folder

Executables related to that module.

Contents of this folder will be symlinked into `$HOME/bin`.

##### `.../00Modules/dotfiles` folder

Configuration files that live in the `$HOME` directory.  Generally speaking contents of this file are symlinked to the `$HOME` directory with a `.` prepended to the name.  There are some exceptions:

- **vimrc** A special-built vimrc file will recursively look through modules to find `dotfiles/vimrc` files and include them.
- **ssh_config** On CAHHome install a `$HOME/.ssh/config` file will be generated by recursively combining all `dotfiles/ssh_config` files from modules.  See the section on Generated Files for more details.
- **gitconfig** On CAHHome install a `$HOME/.gitconfig` file will be generated by recursively combining all `dotfiles/gitconfig` files from modules.  See the section on Generated Files for more details.  TODO: Is this still necessary?  Am I still using pre-git-1.7.0?  If not, combine this into using [include] directives.

##### `.../00Modules/zshrc` file

This is the main script file that will be sourced as part of shell startup.

##### `.../00Modules/profile` file

A file that will be sourced as part of the shell profile initialization.

##### `.../00Modules/crontab` file

On CAHHome install a crontab file is generated from all `dotfiles/crontab` files in the modules.  See the section on Generated Files for more details.

##### `.../00Modules/update` file (optional)

If this module requires special logic to update, put it in here.  If the file does not exist, the default behavior is to to a `git pull` on the module (see the section on updating for more details.)

1. Check for a `.git` folder in the module
2. If that exists, do a `git pull`
3. If it doesn't exist, do nothing

##### `.../00Modules/desired_zsh` file (optional)

This is used to dynamically switch which shell is executing.  Mostly only useful in one instance inside Amazon.

If this file exists and is executable, it must output the path to a zsh executable.  If that is not the currently running zsh executable, then we will exec out to that executable instead.

#### `$CAHHOME/profile` file

Executes all the `profile` files in relevant modules.

This is the file that will be symlinked to `$HOME/.profile`.

#### `$CAHHOME/vimrc` file

Include and execute all the `dotfiles/vimrc` files in relevant modules.

#### `$CAHHOME/zshrc` file

Executes all the `zshrc` files in relevant modules.

#### `$CAHHOME/zshrc_update` file

Checks the last time the CAHHome system was updated and runs an update if necessary.  Then calls out to `$CAHHOME/zshrc` for normal startup.

This is the file that will be symlinked to `$HOME/.zshrc`.

### `$HOME/.CAHHOME` file

This file is intended to be `source`-able by both bash and zsh, and sets up specific environment variables and functions related to CAHHome.  It is created/updated by the installation process.

Important callouts:

- Variable `CAHHOME` is defined as the location of the `.cahhome` directory.
- If they don't exist there already, adds `$HOME/bin` and `$CAHHOME/bin` to the `$PATH`

### `$HOME/.host` folder

Contains specific overrides or configuration on a per-host basis.  Will not be part of any installation, but is referenced by other scripts.

Installation
------------

The installation process is as follows:

1. TODO

### Generated files

Updates
-------

The update process is as follows:

1. `cd $CAHHOME`
2. Do a git pull on the main repo
3. For each module:
    1. If an `update` script exists, run it
    2. If a `.git/` folder exists, do a `git pull`
4. Run an install (see Installation section)