In Windows, you can use the chia CLI from Windows PowerShell, allowing you more flexibility and control. PowerShell is a program where you type commands, press enter, and do things like changing folders, moving files, or running programs, like chia.

# 1. Parallel plotting using PowerShell
```
    cd %systemdrive%%homepath%\AppData\Local\Chia-Blockchain\app-1.1.2\resources\app.asar.unpacked\daemon\
    start-process .\chia.exe -argumentlist "plots create yourParametersGoHere"
    start-process ....
    ....
```

In some instances it appears the Chia daemon is stored in the following directory. This may be either due to installing as Admin or after version 1.1.0 (to be determined). If you can't find the daemon above, try the path below.
```
    cd %systemdrive%\ProgramData\%USERNAME%\chia-blockchain\app-1.1.0\resources\app.asar.unpacked\daemon\
```

If `start-process` doesn't work, try `.\chia.exe plots create yourParametersGoHere` instead.

Or add the path `"%USERPROFILE%\AppData\Local\chia-blockchain\app-1.1.2\resources\app.asar.unpacked\daemon"` to the path user variable, this way you can
execute chia commands, only using "chia" in a command window.

To add a delay between your parallel processes, you can put a `sleep <seconds>` between each `chia plots create` command, e.g. `sleep 3600` to delay the next process by an hour.

### A specific example:
```
    cd %systemdrive%%homepath%\AppData\Local\Chia-Blockchain\app-1.1.2\resources\app.asar.unpacked\daemon\
    start-process ./chia.exe -argumentlist "plots create -k 32 -b 4000 -u 128 -r 4 -t d:\tempdrive1 -2 e:\tempdrive2 -d F:\plots -n 1"
```
The command above makes one plot (specified by `-n 1`), to plot in parallel you need to repeat the command (without closing the first one). Increase the `-n` value for sequential plotting, i.e. once the 1st plot is completed, the next is started.

# 2. Take a good look at log files
Your configuration and logs are found in `~\.chia\mainnet\log` and `~\.chia\mainnet\config`. You can tail your logs with `Get-Content ~\.chia\mainnet\log\debug.log -wait`. To see more of what is going on, set your log level in `config\config.yaml` to INFO from WARNING and restart. You can also use `\.chia.exe configure --set-log-level INFO` from the app directory outlined above and then restart for the changes to take effect.

# 3. Windows (auto) Update can be a pain
Consider going through a Windows Update check and install updates prior to starting a plot process. It can take a while, and updates might initiate a reboot. You can also go into Advanced Options to disable updates for up to 35 days at a time.

# 4. Disable uPnP when running multiple nodes
If you attempt to run more than one node on your local network, having uPnP on on both will cause both nodes significant confusion. You will need to use powershell to disable uPnP on all but one.

### For version 1.0:
```
    cd %systemdrive%%homepath%\AppData\Local\Chia-Blockchain\app-1.1.2\resources\app.asar.unpacked\daemon\
    ./chia.exe configure --enable-upnp false
```

# 5. Forever "Connecting to wallet"
Sometimes your wallet database can get corrupted. If you get stuck on the "Connecting to wallet" spinner for more than 60 seconds, you will probably want to exit the app, delete your wallet database with Powershell, and then start the app again.

### For version 1.0.x:
```
    del ~\.chia\mainnet\wallet\db\blockchain*
```

# 6. Periodic pause during plotting (PowerShell)
If your PowerShell plotting processes seem to pause, you should disable Quick Edit. Powershell -> Properties -> Options: Disable quick edit.

If you are still seeing this symptom it is almost always a [problem with your RAM](https://www.tomshardware.com/how-to/how-to-test-ram).