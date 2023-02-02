# Extract all .zip files to directories

``` sh
$ find -iregex '.*\.zip$' -type f -exec unzip '{}' -d '{}.ETXRACTED' \;
$ find . -iname '*.zip' -execdir unzip "{}" -d "{}.extracted" \;
$ find . -name "*.zip" | while read filename; do unzip -o -d "`basename -s .zip "$filename"`" "$filename"; done;
```

# Purge removed debian packages

``` sh
$ dpkg -l | awk '/^rc/ { print $2 }'
$ apt-get purge $(dpkg -l | awk '/^rc/ { print $2 }')
```

# Start alcibiades with xboard

``` sh
$ xboard -fcp "polyglot -noini -log true -ec /home/evgeni/src/alcibiades/target/release/alcibiades"
```

# Allow 1600x900 for the display projector
``` sh
$ xrandr --addmode DP-2 1600x900
```

# Allow unsigned extensions in Firefox

`xpinstall.signatures.required = false`

# Compile `greenlet` on alpine

To comple greenlet on alpine, build-base and python3-dev packages must be
installed.
