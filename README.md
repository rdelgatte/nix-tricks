# Nix - Tips & Tricks

[Nix](https://nixos.org/) is a package manager just like Apt, NPM, Brew... but it goes way beyond as it brings key features like:

- full reproducibility (same input = same output)
- "local" packages, meaning you don't need to install globally
- describe in a Nix file everything you need to build or run an application, and Nix configures everything locally:
  - system dependencies like `node` or `PostgreSQL`
  - build systems like `Maven` or `Stack`
  - language dependencies like `JDK` versions
  - environment variables
  - etc.

## Install

```shell
sh <(curl -L https://nixos.org/nix/install)
```

Running this command is going to prompt messages you need to read carefully so the install is done successfully (installation directory, multi/single user install, ...).

Once the operation is over, you may have to restart your terminal so you can run `nix --version` and get it up and ready.

Installing nix comes with `nix-env` which helps in dealing with nix environments (to query, install or upgrade packages).

### Niv

[niv](https://github.com/nmattia/niv) eases Nix projects dependencies handling (adding, updating or removing) through a single file, `nix/sources.json`.

```shell
nix-env -iA niv
```

While `nixpkgs` provides a single entry point for a huge list of packages, some are not part of it, and thus additional dependencies must be managed.

Handling the `nixpkgs` and other repositories by hand may be tricky as whenever you want to add or update a dependency, you need to find a recent Git hash, change it, prefetch the new tar.gz, find the sha256, change it.

Niv makes this a one liner `niv update`.

- To update all dependencies, run `niv update` (or `niv update hello` to update the `hello` dependency only)
- To add a dependency to repository `https://github.com/hello`, run `niv add hello`
- To remove a dependency, run `niv drop hello`

### Optional - Install Direnv

Optionally, installing `direnv` helps in loading `nix-shell` when landing in the project directory: [`direnv`](https://direnv.net/docs/installation.html):

```shell
brew install direnv
```

You need to hook `direnv` to your bash configuration (e.g. `~/.zshrc`) file:

```shell
eval "$(direnv hook zsh)"
```

### Update `nixpkgs`

As packages versions defined in `nixpkgs` may not be up-to-date / recent, it is useful to update to ``nixpkgs`-unstable`.

```shell
niv update `nixpkgs` -b `nixpkgs`-unstable
```

### `nix repl`

Nix also provides a REPL (`nix repl`) to explore the Nix language but also to find any derivations from your default nix packages collection or any specific one (`nix repl '<`nixpkgs`>'` - `<`nixpkgs`>` is probably different for each of our machines).

- Load the same dependencies available in `shell.nix`:

```
nix-repl> :l nix/default.nix
Added 12348 variables.
```

- Find a specific derivation (using TAB after starting to type):

```
nix-repl> haskell
haskell          haskell-ci       haskellPackages
```

- Accessing a derivation:

```
nix-repl> ormolu
«derivation /nix/store/1bfj81j8i9ha8j1ifsdrx7hsrkxpmfhx-ormolu-0.1.2.0.drv»

nix-repl> haskell.packages.ghc8103.haskell-language-server
«derivation /nix/store/h2sfrfhq9r7wf5si2i6hg1qhmmcbydks-haskell-language-server-1.0.0.0.drv»
```

- Pretty-printing a derivation (to JSON):

```
nix-repl> postgresql_11
«derivation /nix/store/bz4yiyfxbg4jagirzj6k9alzwx1l6385-postgresql-11.9.drv»
# Outside of Nix REPL
$ nix show-derivation /nix/store/bz4yiyfxbg4jagirzj6k9alzwx1l6385-postgresql-11.9.drv

{
  "/nix/store/bz4yiyfxbg4jagirzj6k9alzwx1l6385-postgresql-11.9.drv": {
    "outputs": {
      "doc": {
        "path": "/nix/store/nw4c4g1xqk95ckr25x1l0p2mhdaq5wd2-postgresql-11.9-doc"
      },
      "lib": {
        "path": "/nix/store/vcqlz1wa5n4ajjz05zjyj1jr8byv7l5q-postgresql-11.9-lib"
      },
      "man": {
        "path": "/nix/store/rrgszm5jpxb3c7pgyl01mlxmghxp2lkd-postgresql-11.9-man"
      },
      "out": {
        "path": "/nix/store/c7kvwyv1z6r4a2k9cmm30zrmz7551h33-postgresql-11.9"
      }
    },
    "inputSrcs": [
      "/nix/store/75a44m59dwpwwgm7pcwwh9hmlx17gq63-specify_pkglibdir_at_runtime.patch",
      "/nix/store/7s83qirx82lfwhym8zc4dnpi4hfbrd22-less-is-more-96.patch",
      "/nix/store/9krlzvny65gdc8s7kpb6lkx8cd02c25b-default-builder.sh",
      "/nix/store/i24yzg71m9ca1q459dvs1bqhz6ry7s7p-socketdir-in-run.patch",
      "/nix/store/jn9shys2nd8qjfsn4px8a9wky6askj8m-hardcode-pgxs-path-96.patch",
      "/nix/store/zpsqrnfpmmw840vr6xgsmynp6k4lk3ki-findstring.patch",
      "/nix/store/zv4waczq5k45138d2sdy7dyidas90lhh-disable-resolve_symlinks-94.patch"
    ],
    "inputDrvs": {
      "/nix/store/24sqxl1l5ams3vjngadxxihh394j72g5-libxml2-2.9.10.drv": [
        "dev"
      ],
      "/nix/store/4qry96ap0kpkjwjlsyc8p3m3hh6pg5pv-bash-4.4-p23.drv": [
        "out"
      ],
      "/nix/store/8m5x7q4s1lsar4p61zzir7hc2910bsym-readline-6.3p08.drv": [
        "dev"
      ],
      "/nix/store/ajlklk2853whqffjf15y7rckncz8fzzk-icu4c-67.1.drv": [
        "dev"
      ],
      "/nix/store/djzqwadfbv770dx6wfbw4by7aqn4mwvr-glibc-2.31.drv": [
        "bin"
      ],
      "/nix/store/f3fqq9wwslbkb3xks7bchn56869ibkkb-hook.drv": [
        "out"
      ],
      "/nix/store/fdd2vaircsn9zswkdkg9cvjrmh97yq4b-postgresql-11.9.tar.bz2.drv": [
        "out"
      ],
      "/nix/store/idgv8h799k0wvfs3w8lvgq9qpx27xrjp-systemd-246.drv": [
        "dev"
      ],
      "/nix/store/ja8rby976wn7qgbqn4k9mkgdc42m6wi2-zlib-1.2.11.drv": [
        "dev"
      ],
      "/nix/store/nizihiiy8gcwn61sfd538vq0bf3ll5ll-stdenv-linux.drv": [
        "out"
      ],
      "/nix/store/nnmhsc9bcxw3vxi9pa9zv4l793lkks5c-libossp-uuid-1.6.2.drv": [
        "out"
      ],
      "/nix/store/p3d0li4iw9yn317lz9n5gsafabij3rib-gcc-wrapper-9.3.0.drv": [
        "out"
      ],
      "/nix/store/phdgi399an7jn8x2lb3bqx21spgdllp9-openssl-1.1.1g.drv": [
        "dev"
      ],
      "/nix/store/pqwvlcvw7y40670q21gd3vzk6fv6gy0r-tzdata-2019c.drv": [
        "out"
      ],
      "/nix/store/r80z731phckd2bbiank9qr6s9vpfilza-pkg-config-wrapper-0.29.2.drv": [
        "out"
      ]
    },
    "platform": "x86_64-linux",
    "builder": "/nix/store/6ffp6xbr6j2ryqw2dafbbvv55ggq16iv-bash-4.4-p23/bin/bash",
    "args": [
      "-e",
      "/nix/store/9krlzvny65gdc8s7kpb6lkx8cd02c25b-default-builder.sh"
    ],
    "env": {
      "LC_ALL": "C",
      "NIX_CFLAGS_COMPILE": "-I/nix/store/f69s8010a0imbwqbp4rk73d15ybb26ps-libxml2-2.9.10-dev/include/libxml2",
      "buildFlags": "world",
      "buildInputs": "/nix/store/17sjssmqv2gxqxp8qnhwn4rpc0s60fs5-zlib-1.2.11-dev /nix/store/z86ipkbqb06xg9fkjfflj9qqwfh9mykm-readline-6.3p08-dev /nix/store/ixnfsvfklpkwrds7qrks4ff6xxyv5m8n-openssl-1.1.1g-dev /nix/store/f69s8010a0imbwqbp4rk73d15ybb26ps-libxml2-2.9.10-dev /nix/store/pczfnmvqchd3g0zadbh5sbsbj1qi1v50-hook /nix/store/ic0pky86h7q0xjh8w3pv7lxiyjfpf926-icu4c-67.1-dev /nix/store/sn2hlscnrzqma768if06cc2fmrdka9wq-systemd-246-dev /nix/store/pd23sa619yjm4xi6klhhlibkr9xki9nv-libossp-uuid-1.6.2",
      "builder": "/nix/store/6ffp6xbr6j2ryqw2dafbbvv55ggq16iv-bash-4.4-p23/bin/bash",
      "checkTarget": "check",
      "configureFlags": "--with-openssl --with-libxml --sysconfdir=/etc --libdir=$(lib)/lib --with-system-tzdata=/nix/store/ilbwv6r37l626g7fg5s1g93k70zq3rp9-tzdata-2019c/share/zoneinfo --with-systemd --with-ossp-uuid --with-icu",
      "depsBuildBuild": "",
      "depsBuildBuildPropagated": "",
      "depsBuildTarget": "",
      "depsBuildTargetPropagated": "",
      "depsHostHost": "",
      "depsHostHostPropagated": "",
      "depsTargetTarget": "",
      "depsTargetTargetPropagated": "",
      "disallowedReferences": "/nix/store/mimzy2k99xq842k59zl3f2z2s34rcq5b-gcc-wrapper-9.3.0",
      "doCheck": "1",
      "doInstallCheck": "",
      "doc": "/nix/store/nw4c4g1xqk95ckr25x1l0p2mhdaq5wd2-postgresql-11.9-doc",
      "enableParallelBuilding": "1",
      "enableParallelChecking": "1",
      "installTargets": "install-world",
      "lib": "/nix/store/vcqlz1wa5n4ajjz05zjyj1jr8byv7l5q-postgresql-11.9-lib",
      "man": "/nix/store/rrgszm5jpxb3c7pgyl01mlxmghxp2lkd-postgresql-11.9-man",
      "name": "postgresql-11.9",
      "nativeBuildInputs": "/nix/store/0w3s9m519vnh8bvjn1vipj8cz7c7iigd-pkg-config-wrapper-0.29.2",
      "out": "/nix/store/c7kvwyv1z6r4a2k9cmm30zrmz7551h33-postgresql-11.9",
      "outputs": "out lib doc man",
      "patches": "/nix/store/zv4waczq5k45138d2sdy7dyidas90lhh-disable-resolve_symlinks-94.patch /nix/store/7s83qirx82lfwhym8zc4dnpi4hfbrd22-less-is-more-96.patch /nix/store/jn9shys2nd8qjfsn4px8a9wky6askj8m-hardcode-pgxs-path-96.patch /nix/store/75a44m59dwpwwgm7pcwwh9hmlx17gq63-specify_pkglibdir_at_runtime.patch /nix/store/zpsqrnfpmmw840vr6xgsmynp6k4lk3ki-findstring.patch /nix/store/i24yzg71m9ca1q459dvs1bqhz6ry7s7p-socketdir-in-run.patch",
      "pname": "postgresql",
      "postConfigure": "# Hardcode the path to pgxs so pg_config returns the path in $out\nsubstituteInPlace \"src/common/config_info.c\" --replace HARDCODED_PGXS_PATH $out/lib\n",
      "postFixup": "# initdb needs access to \"locale\" command from glibc.\nwrapProgram $out/bin/initdb --prefix PATH \":\" /nix/store/r5xqfk8p7c79yqphy25rbvm71yfvrzh4-glibc-2.31-bin/bin\n",
      "postInstall": "moveToOutput \"lib/pgxs\" \"$out\" # looks strange, but not deleting it\nmoveToOutput \"lib/libpgcommon-.a\" \"$out\"\nmoveToOutput \"lib/libpgport-.a\" \"$out\"\nmoveToOutput \"lib/libecpg-\" \"$out\"\n\n# Prevent a retained dependency on gcc-wrapper.\nsubstituteInPlace \"$out/lib/pgxs/src/Makefile.global\" --replace /nix/store/mimzy2k99xq842k59zl3f2z2s34rcq5b-gcc-wrapper-9.3.0/bin/ld ld\n\nif [ -z \"${dontDisableStatic:-}\" ]; then\n  # Remove static libraries in case dynamic are available.\n  for i in $out/lib/-.a $lib/lib/-.a; do\n    name=\"$(basename \"$i\")\"\n    ext=\".so\"\n    if [ -e \"$lib/lib/${name%.a}$ext\" ] || [ -e \"${i%.a}$ext\" ]; then\n      rm \"$i\"\n    fi\n  done\nfi\n",
      "preConfigure": "CC=cc",
      "propagatedBuildInputs": "",
      "propagatedNativeBuildInputs": "",
      "setOutputFlags": "",
      "src": "/nix/store/dxvg4a78inacfjy0493g4bhbnzj8ld2n-postgresql-11.9.tar.bz2",
      "stdenv": "/nix/store/7ylf64vl6747sf45daaw707d4k97wn13-stdenv-linux",
      "strictDeps": "",
      "system": "x86_64-linux",
      "version": "11.9"
    }
  }
}
```

## Init Nix for a project

```shell
mkdir new-project
cd new-project
nix-env init
```

### Shell.nix

Create file `shell.nix` at the directory root:

```nix
let
  pkgs = import <nixpkgs> {};
in
pkgs.mkShell {
  buildInputs = [

  ];
}
```

This is the file that describes everything you need to build and execute the project (system requirements, build tools, ...):

- `buildInputs` describing all executables that must be available in the path (compiler, formatter, database tools...)
- the environment variables

## Add dependency

https://github.com/NixOS/nixpkgs

## Upgrades
