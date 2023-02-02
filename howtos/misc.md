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

# Firefox tweaks

- `xpinstall.signatures.required` -- allow unsigned extensions
- `ui.key.menuAccessKeyFocuses` -- disables the Alt in Firefox
- `browser.audioFeeds.handlers.application` -- /usr/bin/icedove
- `browser.feeds.handlers.application` -- /usr/bin/icedove
- `network.http.sendRefererHeader` -- 2
- `image.animation_mode` -- none

# Compile `greenlet` on alpine

To comple greenlet on alpine, build-base and python3-dev packages must be
installed.

# Find which process uses a mounted device

Run as root:

``` sh
$ fuser -m /dev/sdc1
/dev/sdc1: 538

$ ps auxw|grep 538
donncha 538 0.4 2.7 219212 56792 ? SLl Feb11 11:25 rhythmbox
```
