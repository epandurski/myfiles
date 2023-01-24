# mysecrets

Files containing encrypted secrets.

## Exporting the private and public GPG keys

To export the keys to the local GPG key-store, run:

```sh
$ gpg --import private.key
$ gpg --import public.key
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
$ ln -s password-store/ ~/.password-store
```
