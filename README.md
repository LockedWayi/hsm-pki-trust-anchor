# hsm-pki-trust-anchor

One public key, on purpose.

This repository holds the **trust anchor** for the key inventory published by
[`multivendor-hsm-pki`](https://github.com/LockedWayi/multivendor-hsm-pki):
the public half of `inventory-signing-key-v1`, which signs that project's
`docs/keys/key-inventory.json`.

## Why it is not in the project it protects

The inventory, its detached signature, and the key that verifies that
signature used to be three files in one tree. That arrangement verifies that
the three files agree with each other — which they would also do if someone
who could write to the repository rewrote all three together. The signature
was real; what was missing is that the anchor was not independent of the
thing it authenticates.

Moving the anchor out is the fix. To change what a verifier trusts you now
have to compromise **two** repositories with separate protection, rather than
edit one tree.

## What is guaranteed here

- The file changes only through a pull request that passes this repository's
  required checks. Force-pushes and branch deletion are refused, and that
  applies to the owner too.
- Consumers pin a **commit SHA**, never a branch. A branch is a pointer that
  can be moved; a commit is the bytes themselves.
- Consumers also pin the file's **SHA-256**, so a fetch that returns anything
  else fails closed rather than proceeding with whatever arrived.

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

Compare that digest with the one pinned in the consuming repository. If they
disagree, stop — do not fall back to a copy found anywhere else. An
unreachable or unexpected anchor must fail closed; that is the entire reason
this repository exists.
