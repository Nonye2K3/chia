1. To plot parallel in Windows you will need to use powershell. A generic example for Beta 22:
```
cd C:\Users\yourUserName\AppData\Local\Chia-Blockchain\app-0.1.22\resources\app.asar.unpacked\daemon\
start-process ./chia.exe -argumentlist "plots create yourParametersGoHere"
start-process ....
....
```

A specific example, again using Beta 22:

```
cd C:\Users\yourUserName\AppData\Local\Chia-Blockchain\app-0.1.22\resources\app.asar.unpacked\daemon\
start-process ./chia.exe -argumentlist "plots create -k 32 -b 4000 -u 128 -r 4 -t d:\tempdrive1 -2 e:\tempdrive2 -d F:\plots -n 1"
```
If you are using Beta 16 you would change the `Chia-Blockchain\app-0.1.22\resources\` section to `Chia-Blockchain\app-0.1.16\resources\` in the command above.

2. Windows and Python used to not get along regarding reporting CPU % on Windows but starting with Beta 18 this has been fixed.

3. You configuration and logs are found in `~\.chia\VERSION\log` so `~\.chia\beta-1.0b22\log` in Beta 22. You can tail your logs with `Get-Content ~\.chia\VERSION\log\debug.log -wait`. To see more of what is going on, set your log level in `conf\config.yaml` to INFO from WARNING and restart. You can also use `\.chia.exe configure --set-log-level INFO from the app directory outlined above and then restart for the changes to take effect.

4. Consider going through a Windows Update check and install updates prior to starting a plot process. It can take a while, and updates might initiate a reboot.

5. If you attempt to run more than one node on your local network, having uPnP on on both will cause both nodes significant confusion. You will need to use powershell to disable uPnP on all but one. An example for Beta 22:
```
cd C:\Users\yourUserName\AppData\Local\Chia-Blockchain\app-0.1.22\resources\app.asar.unpacked\daemon\
./chia.exe configure --upnp-enable False
```

6. Sometimes your wallet database can get corrupted. If you get stuck on the "Connecting to wallet" spinner for more than 60 seconds, you will probably want to exit the app, delete your wallet database with powershell, and then start the app again.
```
# For Beta 22:
cd ~\.chia\beta-1.0b22\wallet\db
del *.db
```
