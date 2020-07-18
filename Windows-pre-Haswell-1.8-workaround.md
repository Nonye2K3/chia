# Patching 1.8 for older (pre-Haswell) processors

- Make sure the Chia application is totally shut down.
- You will probably need an Administrator Powershell
- [Download the patched .pyd](https://download.chia.net/beta-1.8-win64/blspy.cp37-win_amd64.pyd)
- In Powershell `rm ~\AppData\Local\Chia-Blockchain\app-0.1.8\resources\app.asar.unpacked\daemon\blspy.cp37-win_amd64.pyd`
- Copy the new .pyd into the daemon\ directory - `copy blspy.cp37-win_amd64.pyd ~\AppData\Local\Chia-Blockchain\app-0.1.8\resources\app.asar.unpacked\daemon\`
- Start the Chia application and proceed as normal.
- This will be fixed 1.9.

# Patching 1.8 for Pentium Skylake processors

- You will only ever need one of these patches or the other - never both.
- Make sure the Chia application is totally shut down.
- `cd ~\AppData\Local\Chia-Blockchain\app-0.1.8\resources\app.asar.unpacked\daemon`
- `mv mpir.dll mpirold.dll`
- `mv mpir_gc.dll mpir.dll`
- Start the Chia application and proceed as normal.
- This will be fixed 1.9.
