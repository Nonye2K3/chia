# How to use git to patch chia-blockchian

Everyone once in a while in our pre-mainnet software, an issue will come up that's not worth generating a whole new release but is worth putting a fix in and letting interested folks apply it. This assumes you already have a working copy of chia-blockchain.

1. From your chia-blockchain directory type `git fetch` and then `git status`. You'll get back something that looks like this though sometimes you'll get much less:
```bash
remote: Enumerating objects: 60, done.
remote: Counting objects: 100% (60/60), done.
remote: Compressing objects: 100% (34/34), done.
remote: Total 60 (delta 34), reused 36 (delta 26), pack-reused 0
Unpacking objects: 100% (60/60), done.
From https://github.com/Chia-Network/chia-blockchain
   b077a7a..1212590  master                 -> origin/master
 * [new branch]      beta-1.1               -> origin/beta-1.1
 * [new branch]      rk-remove-setproctitle -> origin/rk-remove-setproctitle
```

2. Now you should stop your node/farmer with `chia-stop-all` and exit the virtual environment with `deactivate`.

3. Update your software with `git pull`. You'll get something like:
```bash
Updating b077a7a..1212590
Fast-forward
 INSTALL.md                     | 4 ++--
 README.md                      | 2 +-
 src/full_node/blockchain.py    | 3 +++
 src/full_node/full_node.py     | 3 +--
 src/wallet/websocket_server.py | 2 +-
 5 files changed, 8 insertions(+), 6 deletions(-)
```
4. Now run the install.sh file with `sh install.sh`.

5. Once that completes successfully, re-enter the virtual environment with `. ./activate` and then start whatever is appropriate for you like `chia-start-farmer &`.