# CAHHome

My dotfiles and shell configuration.

## Structure

```
-- $HOME/
   |
   +-- .cahhome/
   |   |
   |   +-- bin/
   |   |   |
   |   |   ...
   |   |
   |   ...
   |
   +-- .CAHHOME
   |
   ...
```

### `$HOME/.cahhome` folder

This is where you should clone this repo, and where all of the important bits live.  This is often referred to (here and in the code) as `$CAHHOME`.

#### `$CAHHOME/bin` folder

Executables directly related to managing/executing CAHHome.

### `$HOME/.CAHHOME` file

This file is intended to be `source`-able by both bash and zsh, and sets up specific environment variables and functions related to CAHHome.  It is created/updated by the installation process.

Important callouts:

- Variable `CAHHOME` is defined as the location of the `.cahhome` directory.
