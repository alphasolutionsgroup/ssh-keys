# ssh-keys

The one list of SSH public keys that may log in to our infrastructure.
`authorized_keys` is what every new server trusts: fleet pulls this repo and
gives the file to each VM it builds, and hand-built servers should copy it too.

Anyone who can change this file can get onto every server built from it, so
`main` only changes through a pull request. That leaves a record of who added
or removed each key and when.

## The file

- One key per line, in the usual `authorized_keys` format.
- A `#` comment above each key says whose it is and which device it's on.
  Blank lines and comments are ignored.
- Only public keys (`.pub`) go here. Never a private key.

## Adding a key

1. On the device: `cat ~/.ssh/id_ed25519.pub` (make one with
   `ssh-keygen -t ed25519 -C you@device` if there isn't one).
2. On a branch, add a comment line and the key, open a pull request naming the
   ticket, and merge it.
3. New servers get it from then on. Existing servers don't: add it to them
   separately.

## Removing a key

Remove the line by pull request, the same way. That stops new servers trusting
it. Servers already built still do, so take it out of their
`~/.ssh/authorized_keys` too. When a device is lost or someone leaves, do both
straight away.
