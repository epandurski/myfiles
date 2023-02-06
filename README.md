# myfiles

Files that I want to have on each computer that I use. Some of them contain
encrypted secrets. The file `private.asc` contains my private GPG key,
protected by a strong passphrase.

## Exporting the private and public GPG keys

To export the keys from the local GPG key-store, run:

```sh
$ gpg --list-keys
$ gpg --export -a "Evgeni Pandurski" > public.asc
$ gpg --export-secret-key -a "Evgeni Pandurski" > private.asc
```

## Importing the private and public GPG keys

To import the keys to the local GPG key-store, run:

```sh
$ gpg --import public.asc
$ gpg --import private.asc
```

To elevate the trust level of the newly exported key (most probably to
"ultimate"), run:

```sh
$ gpg --edit-key 1596E1ACC6D3FE56CB71792F84B44B1E45D42FA3 trust
```

To change the passphrase that protects the private key, run:

```sh
$ gpg --edit-key 1596E1ACC6D3FE56CB71792F84B44B1E45D42FA3 passwd
```

## Setting up the password store

To use the password store, the standard Unix password manager (`pass`) must
be installed. To install `pass`, run `apt install pass`. To set up the
password store, run:

```sh
$ ln -s ~/src/myfiles/password-store/ ~/.password-store
```

## Copy configuration files in user's home directory

Copy the contents of the `dot_files` directory to `~\`. If a file with the
same name already exists in the home directory, use `diff` to figure out how
to merge the changes.
