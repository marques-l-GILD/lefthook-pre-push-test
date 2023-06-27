# lefthook-pre-push-test

Test repository for [evilmartians/lefthook#508](https://github.com/evilmartians/lefthook/issues/508).

## Context:

The git `pre-push` hook, in addition to the "remote" and "url" command line args, also receives 4 space-separated parameters in STDIN as documented here: https://git-scm.com/docs/githooks#_pre_push

`lefthook` does not appear to forward the info/params received on STDIN to the `pre-push` scripts that it is responsible for triggering.

## Usage

To demonstrate the correct behavior, **fork this repo to your own account** and replace the `.git/hooks/pre-push` installed by lefthook with the [`pre-push.sh` script in this repo](.lefthook/pre-push/pre-push.sh) like so:

```console
$ cp .lefthook/pre-push/pre-push.sh .git/hooks/pre-push && chmod +x .git/hooks/pre-push
```

Then make a dummy commit and push. You should get output like the following:

```console
Received from STDIN:

  - local_ref:  'refs/heads/main'
  - local_oid:  'a1e01593eb65100d3afae04e4a33a42ba9f458c3'
  - remote_ref: 'refs/heads/main'
  - remote_oid: 'dc1c961eed75a8ede754fa0dd02345a01c59de68'

Received as arguments:

  - remote: origin
  - url:    git@github.com:marques-l-GILD/lefthook-pre-push-test.git

```

Now, try reinstalling the pre-push hook from lefthook, make a dummy commit, and try pushing:

```console
$ lefthook add pre-push
$ git commit --allow-empty -m "empty commit for testing"
$ git push

Lefthook v1.4.3
RUNNING HOOK: pre-push
⠦ waiting: pre-push.sh
#          ^^^^^^^^^^^ hangs here waiting for STDIN
```
