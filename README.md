This file contains instructions for installing Stack on NixOS 20 with minimal dependencies on NixOS, allowing Stack to install an arbitrary version of GHC in an isolated location rather than relying on a GHC version in nixpkgs.

Make sure that you have the `gmp`, `ncurses5`, `zlib`, and `zlib.dev` packages installed in the Nix store.

Run:

```bash
sudo mkdir /lib64
sudo ln -s `nix eval --raw nixpkgs.glibc`/lib/ld-linux-x86-64.so.2 /lib64/ld-linux-x86-64.so.2
curl -sSL https://get.haskellstack.org/ | sh
```

Add the following lines to your .bashrc (or another shell script if you prefer, as long as you source it before you `stack build`):

```bash
GMP_PATH=`nix eval --raw nixpkgs.gmp`/lib
CURSES_PATH=`nix eval --raw nixpkgs.ncurses5`/lib
ZLIB_PATH=`nix eval --raw nixpkgs.zlib`/lib
ZLIB_INCLUDE_PATH=`nix eval --raw nixpkgs.zlib.dev/`/include
export LD_LIBRARY_PATH=$GMP_PATH:$CURSES_PATH:$ZLIB_PATH:$LD_LIBRARY_PATH
export LIBRARY_PATH=$GMP_PATH:$CURSES_PATH:$ZLIB_PATH:$LIBRARY_PATH
export C_INCLUDE_PATH=$ZLIB_INCLUDE_PATH:$C_INCLUDE_PATH
```

Run:

```bash
source ~/.bashrc
```

Add the following to `~/.stack/config.yaml`:

```yaml
ghc-build: standard
nix:
  enable: false
```
