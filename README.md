# What is it?

`osslsigncode` (available at [GitHub](https://github.com/mtrojnar/osslsigncode))
is a fantastic tool for giving Windows binaries you've cross-compiled on Linux
or MacOS valid Microsoft-approved code signatures. You no longer have to move
your binaries into a Windows dev environment to sign them there.

Unfortunately, `osslsigncode` is also complex, with command lines that are
error-prone. It also doesn’t integrate well with build environments for that
same reason.

`signcode` helps solve that.

# Installation and use

The `signcode` Python script goes wherever you like (I prefer `$HOME/.local/bin`).
Then:

```shell
$ mkdir -p $HOME/.config/signcode
$ touch $HOME/.config/signcode/signcode.toml
$ chmod 600 $HOME/.config/signcode/signcode.toml
$ vim $HOME/.config/signcode/signcode.toml
```

Here you enter your system-wide configuration data. (Don’t worry, they can
all be overridden by per-project files that follow the same format.)

```toml
osslsigncode = "/usr/bin/osslsigncode"
certificate = "/path/to/code/signing/certificate.pfx"
passphrase = "certificate passphrase goes here"
timestamp-service = "http://timestamp.sectigo.com"
digest-algo = "sha256"
```

Now, back in your source tree, compose another `signtool.toml` file and
check it into Git:

```toml
app-name = "Fooblitsky"
app-website = "https://www.example.com"
```

At this point you’re good to go. At the end of your release build, add a
step:

```shell
$ signtool -c /path/to/my/project/config.toml ./my_cool_app.exe
```

… and presto, you’re now signing your release builds!
