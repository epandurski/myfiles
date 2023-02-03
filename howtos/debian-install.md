# Netinstall via Wifi

Most Wifi chips require a non-free firmware, which is not shipped with the
Debian "netinstall" image. The firmware should be loaded from removable
media, such as an USB stick.

The installer will tell you what the missing firmware files are. For
example: `iwlwifi-7260-9.ucode` and `iwlwifi-7260-8.ucode`. First, find the
required firmware files. To that end:

- Go to the Debian site.

- Click the Debian Packages link, to continue to the Debian Packages page.

- Under the Packages header, select the "Search the contents of packages"
  link.
  
- In the Keyword field, enter: iwlwifi.

- Under Display, select the option labelled packages that contain files
  whose names contain the keyword.
  
- Leave the Distribution value unchanged at stable.

- Set the Architecture value to 64-bit PC (amd64).

- Click the Search button.

- In the list of search results that appears next, look for the two files
  that are needed (iwlwifi-7260-8.ucode and iwlwifi-7260-9.ucode for
  example).
  
- Under the Download firmware-iwlwifi header, click the all architecture
  link to continue.
  
- On the next page, click the link to the site from which I wanted to
  download the file.

The Debian package firmware-iwlwifi_XXXX_all.deb will be downloaded.

Next, it is time to extract the contents of the downloaded packages. Open a
command-line terminal window, and go to the directory where the packages
were downloaded. Run:

```sh
$ dpkg-deb --extract firmware-iwlwifi_0.43_all.deb .
```

This step will create two new sub-directories: `lib` and `usr`. The `lib`
directory, in turn, contains a further sub-directory `firmware`, that holds
all firmware files extracted from both the package. Copy the entire contents
of the firmware sub-directory to a USB stick that was formatted with a
(V)FAT filesystem.

The directory listing of the USB stick, then, will look like this:

```
drwx------ 2 luvr luvr  16384 May 15 13:58 intel
-rw-r--r-- 1 luvr luvr 337520 Jun 16  2014 iwlwifi-1000-5.ucode
-rw-r--r-- 1 luvr luvr 337572 Jun 16  2014 iwlwifi-100-5.ucode
-rw-r--r-- 1 luvr luvr 689680 Jun 16  2014 iwlwifi-105-6.ucode
-rw-r--r-- 1 luvr luvr 701228 Jun 16  2014 iwlwifi-135-6.ucode
-rw-r--r-- 1 luvr luvr 695876 Jun 16  2014 iwlwifi-2000-6.ucode
-rw-r--r-- 1 luvr luvr 707392 Jun 16  2014 iwlwifi-2030-6.ucode
-rw-r--r-- 1 luvr luvr 670484 Jun 16  2014 iwlwifi-3160-7.ucode
-rw-r--r-- 1 luvr luvr 667284 Jun 16  2014 iwlwifi-3160-8.ucode
-rw-r--r-- 1 luvr luvr 666792 Jun 16  2014 iwlwifi-3160-9.ucode
-rw-r--r-- 1 luvr luvr 150100 Jun 16  2014 iwlwifi-3945-2.ucode
-rw-r--r-- 1 luvr luvr 187972 Jun 16  2014 iwlwifi-4965-2.ucode
-rw-r--r-- 1 luvr luvr 353240 Jun 16  2014 iwlwifi-5000-2.ucode
-rw-r--r-- 1 luvr luvr 340696 Jun 16  2014 iwlwifi-5000-5.ucode
-rw-r--r-- 1 luvr luvr 337400 Jun 16  2014 iwlwifi-5150-2.ucode
-rw-r--r-- 1 luvr luvr 454608 Jun 16  2014 iwlwifi-6000-4.ucode
-rw-r--r-- 1 luvr luvr 444128 Jun 16  2014 iwlwifi-6000g2a-5.ucode
-rw-r--r-- 1 luvr luvr 677296 Jun 16  2014 iwlwifi-6000g2a-6.ucode
-rw-r--r-- 1 luvr luvr 679436 Jun 16  2014 iwlwifi-6000g2b-6.ucode
-rw-r--r-- 1 luvr luvr 463692 Jun 16  2014 iwlwifi-6050-4.ucode
-rw-r--r-- 1 luvr luvr 469780 Jun 16  2014 iwlwifi-6050-5.ucode
-rw-r--r-- 1 luvr luvr 683236 Jun 16  2014 iwlwifi-7260-7.ucode
-rw-r--r-- 1 luvr luvr 679780 Jun 16  2014 iwlwifi-7260-8.ucode
-rw-r--r-- 1 luvr luvr 679380 Jun 16  2014 iwlwifi-7260-9.ucode
-rw-r--r-- 1 luvr luvr 690452 Jun 16  2014 iwlwifi-7265-8.ucode
-rw-r--r-- 1 luvr luvr 691960 Jun 16  2014 iwlwifi-7265-9.ucode
drwx------ 2 luvr luvr  16384 May 15 13:58 RTL8192E
drwx------ 2 luvr luvr  16384 May 15 13:58 RTL8192SU
drwx------ 2 luvr luvr  16384 May 15 13:58 rtl_nic
drwx------ 2 luvr luvr  16384 May 15 13:58 rtlwifi
```

Note, in particular, that the USB stick includes the contents of the
firmware directory, not the directory itself!

Finally, the time had come to retry the Debian installation. I simply booted
the computer off the Debian "netinstall" medium. Choose graphical install,
and when it complains again about the missing firmware files, I take the
following steps:

1. Insert the USB stick.

2. Press the `Ctrl+Alt+F2` key combination, to switch to the second virtual
   terminal.

3. Run the `blkid` command, to find out under which device name the USB
   stick was known: `/dev/sdc`. Its FAT filesystem, which contained the
   firmware files, for example `/dev/sdc1`.

4. Mount the FAT filesystem under the `/lib/firmware` mountpoint, of if it
   already exists, mount it somewhere else, and copy the files there.

    ```
    # mkdir /lib/firmware
    # mount /dev/sdc1 /lib/firmware
    ```

5. Press `Ctrl+Alt+F5` to continue the installation process.

# Debian post-installation actions

## Add "contrib" and "non-free" package sources

After a clean Debian install, the first line of the file
`/etc/apt/sources.list` will be something like this:

```
deb http://ftp.bg.debian.org/debian bullseye main
```

Add "contrib" and "non-free" to it:

```
deb http://ftp.bg.debian.org/debian bullseye main contrib non-free
```

## Add an user as a sudo-er

```sh
$ su -
# usermod -aG sudo evgeni
```

After this, a reboot or at least a re-login will be necessary.

## Configure console and keyboard

To configure Cyrillic console fonts (optional):

```sh
$ sudo dpkg-reconfigure console-setup
```

To use "Caps Lock" as "Ctrl":

```sh
$ sudo dpkg-reconfigure keyboard-configuration
```

Then add `XKBOPTIONS="ctrl:swapcaps"` or `XKBOPTIONS="ctrl:nocaps"` in
`/etc/default/keyboard`.

## Install important packages

Aptitude is not installed by default. Run `apt install aptitude` to install
it. Then run `sudo aptitude`, and install the following packages:

- pass
- monkeysphere
- gtk2-engines
- chromium
- transmission
- audacious
- geeqie
- apt-listbugs
- ntp
- gbgoffice
- vlc
- mpv
- pandoc
- baobab
- texlive-full
- gvfs-backends
- gsmartcontrol
- dconf-editor
- xchm
- fbreader
- sqlitebrowser
- meld
- build-essential
- software-properties-gtk
- aspell-bg
- myspell-bg
- rst2pdf
- dos2unix
- gettext
- direnv
- autoconf
- git
- git-gui
- mercurial
- subversion
- isync (mbsync)
- mu4e
- xsel
- xclip
- curl
- ddd
- gtranslator
- arc-theme
- gdebi
- python3-venv
- pavucontrol
- xboard
- eboard
- scid
- stockfish
- polyglot
- emacs
- emacs-common-non-dfsg

### Very useful non-free packages

- ttf-mscorefonts-installer
- firmware-linux
- unrar

### Other potentially useful packages, which normally are not needed

- virtialenvwrapper
- exuberant-ctags
- handbrake
- openshot
- silversearcher-ag
- brasero
- googleearth-package (run make-googleearth-package afterwards)

## Install the GPG keys

Follow the `README.md` file in the "mysecrets" repository.

## Add important configuration files in user's home directory

Copy the contents of the `dot_files` directory to `~\`. If a file with the
same name already exists in the home directory, use `diff` to figure out how
to merge the changes.

## Fine-tune system's "swappines"

To see the current swappiness, run:

```sh
$cat /proc/sys/vm/swappiness
```

To change the swappiness, add `vm.swappiness = 10` at the end of
`/etc/sysctrl.conf`.

## Stop anoying beebs

If the computer "beebs" annoyingly (for example at invalid commands in
Emacs), blacklist the `pcspkr` device in `/etc/modprobe.d/blacklist`, or set
speaker's volume to 0 in `alsamixer`.

## Open a file with Emacs, even if no Emacs server is running

```sh
$ emacsclient -n -a emacs FILE
```

This is especially useful when instructing the file manager, to associate a
given file type with Emacs.

## SSH agents

By default, Xfce 4.10 will try to load gpg-agent or ssh-agent, in that
order, during session initialization. To disable this, create an xfconf key
using the following command:

```sh
$ xfconf-query -c xfce4-session -p /startup/ssh-agent/enabled -n -t bool -s false
```

To force using ssh-agent even if gpg-agent is installed, run the following
instead:

```sh
$ xfconf-query -c xfce4-session -p /startup/ssh-agent/type -n -t string -s ssh-agent
```

## Configure `mbsync` and `mu`

A `~/.mbsyncrc` configuration file must be created (it can be copied from
the `dot_files` directory). Then run the following commands:

```sh
$ cd ~
$ mkdir Mail
$ mbsync -a
$ mu init --maildir=~/Mail --my-address=<MY_EMAIL_ADDRESS>
$ mu index
```

Here `<MY_EMAIL_ADDRESS>` should be substituted with my email address.

## Add the `chrome-pass` extension

https://github.com/hsanson/chrome-pass

To work with Brave, after running `pip3 install --user chrome-pass==0.3.0`,
which creates a `nativePass` python file in `~/.local/bin`, this file should
be edited like this:

``` diff
***************
*** 192,197 ****
--- 192,206 ----
      sys.exit(1)


+ def native_path_brave():
+     if sys.platform == "linux" or sys.platform == "linux2":
+         return os.path.expanduser(
+             '~') + "/.config/BraveSoftware/Brave-Browser/NativeMessagingHosts/"
+
+     sys.stderr.write("Only linux is supported")
+     sys.exit(1)
+
+
  # Installs the Native Host Application manifest for this script into Chrome.
  def install(nativePath, extension_id):
      if sys.platform == "win32":
***************
*** 235,243 ****
--- 244,254 ----
          if len(sys.argv) > 2:
              install(native_path_chrome(), sys.argv[2])
              install(native_path_chromium(), sys.argv[2])
+             install(native_path_brave(), sys.argv[2])
          else:
              install(native_path_chrome(), EXTENSION_ID)
              install(native_path_chromium(), EXTENSION_ID)
+             install(native_path_brave(), EXTENSION_ID)
      elif sys.argv[1] == "pass":
          if len(sys.argv) > 2:
              print_pass(sys.argv[2])
```

## Run `xboard` with Stockfish

```sh
$ xboard -fcp stockfish -fUCI
```

XBoard's configuration is in the `~/.xboardrc` file.

