# How to use git to patch chia-blockchian

Everyone once in a while in our pre-mainnet software, an issue will come up that's not worth generating a whole new release but is worth putting a fix in and letting interested folks apply it. This assumes you already have a working copy of chia-blockchain.

1. From your chia-blockchain directory type `git fetch` and then `git status`. You'll get back something that looks like this though sometimes you'll get much less:
```bash
remote: Enumerating objects: 59, done.
remote: Counting objects: 100% (59/59), done.
remote: Compressing objects: 100% (33/33), done.
remote: Total 59 (delta 34), reused 36 (delta 26), pack-reused 0
Unpacking objects: 100% (59/59), done.
From https://github.com/Chia-Network/chia-blockchain
   b077a7a..42aaf3b  master                 -> origin/master
 * [new branch]      beta-1.1               -> origin/beta-1.1
 * [new branch]      rk-remove-setproctitle -> origin/rk-remove-setproctitle
 * [new tag]         1.0beta1               -> 1.0beta1
```

2. Now you should stop your node/farmer with `chia-stop-all` and exit the virtual environment with `deactivate`.

3. Update your software with `git pull`. You'll get something like:
```bash
Updating b077a7a..42aaf3b
Fast-forward
 INSTALL.md | 4 ++--
 1 file changed, 2 insertions(+), 2 deletions(-)
```
4. Now run the install.sh file with `sh install.sh`.

5. Once that completes successfully, re-enter the virtual environment with `. ./activate` and then start whatever is appropriate for you like `chia-start-farmer &`.