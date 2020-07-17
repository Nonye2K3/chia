# Temporarily patching 1.8 for older (pre-Haswell) processors on Windows

- [Download the patched .pyd](https://download.chia.net/beta-1.8-win64/blspy.cp37-win_amd64.pyd)

- In Powershell `rm ~\AppData\Local\Chia-Blockchain\app-0.1.8\resources\app.asar.unpacked\daemon\blspy.cp37-win_amd64.pyd`

- Then copy the new .pyd into the daemon\ directory - `copy blspy.cp37-win_amd64.pyd ~\AppData\Local\Chia-Blockchain\app-0.1.8\resources\app.asar.unpacked\daemon\`

- Make sure the Chia application is totally shut down and then start it.

