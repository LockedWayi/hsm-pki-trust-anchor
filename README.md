# hsm-pki-trust-anchor

One public key, on purpose.

This repository holds the **trust anchor** for the key inventory published by
[`multivendor-hsm-pki`](https://github.com/LockedWayi/multivendor-hsm-pki):
the public half of `inventory-signing-key-v1`, which signs that project's
`docs/keys/key-inventory.json`.

## Why it is not in the project it protects

The inventory, its detached signature, and the key that verifies that
signature used to be three files in one tree. That arrangement shows the
three files agree with each other. They would also agree if someone who
could write to the repository rewrote all three together.

Moving the anchor out changes one thing. Changing this file in place needs
write access to this repository, where force-pushes and deletions are
refused. Replacing the anchor with another one needs the consumer to accept
new inputs: a different repository, commit or digest. A consumer who copies
those inputs from the consuming repository's README trusts that repository
for that step.

## What is guaranteed here

- The file changes only through a pull request. No review is required to
  merge one, and there are no status checks. The protection is that
  force-pushes and branch deletion are refused, for the owner too.
- Consumers supply a **commit SHA**, never a branch. A branch is a pointer
  that can be moved; a commit is the bytes.
- Consumers also supply the file's **SHA-256**, so a fetch that returns
  anything else fails closed.

## Rotation

A new anchor version arrives as a **new file** (`inventory-signing-key-v2.pub`),
never as an edit to this one. The previous version stays so signatures made
under it remain verifiable for as long as anything needs to check them.
Overwriting a key in place makes rotation a breaking change, which in
practice means it never happens.

## Verifying what you fetched

```sh
curl -fsSL https://raw.githubusercontent.com/LockedWayi/hsm-pki-trust-anchor/<commit-sha>/inventory-signing-key-v1.pub \
  | sha256sum
```

Compare that digest with the one you hold. If they disagree, stop. Do not
fall back to a copy found anywhere else. An unreachable or unexpected anchor
fails closed.
