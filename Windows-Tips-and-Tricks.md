In Windows, you can use the chia CLI from Windows PowerShell, allowing you more flexibility and control. PowerShell is a program where you type commands, press enter, and do things like changing folders, moving files, or running programs, like chia.

# 1. Parallel plotting using PowerShell
```
    cd $env:userprofile\AppData\Local\Chia-Blockchain\app-1.1.5\resources\app.asar.unpacked\daemon\
    start-process .\chia.exe -argumentlist "plots create yourParametersGoHere"
    start-process ....
    ....
```

If `start-process` doesn't work, try `.\chia.exe plots create yourParametersGoHere` instead.

Or add the path `"%USERPROFILE%\AppData\Local\chia-blockchain\app-1.1.5\resources\app.asar.unpacked\daemon"` to the path user variable, this way you can
execute chia commands, only using "chia" in a command window.

To add a delay between your parallel processes, you can put a `sleep <seconds>` between each `chia plots create` command, e.g. `sleep 3600` to delay the next process by an hour.

### A specific example:
```
    cd $env:userprofile\AppData\Local\Chia-Blockchain\app-1.1.5\resources\app.asar.unpacked\daemon\
    start-process ./chia.exe -argumentlist "plots create -k 32 -b 4000 -u 128 -r 4 -t d:\tempdrive1 -2 e:\tempdrive2 -d F:\plots -n 1"
```
The command above makes one plot (specified by `-n 1`), to plot in parallel you need to repeat the command (without closing the first one). Increase the `-n` value for sequential plotting, i.e. once the 1st plot is completed, the next is started.

### Reminder: Manually Add logging when plotting in PowerShell

When plotting with the GUI, the plotter will write logs in \mainnet\plotter for each plot. Many third party tools (such as the excellent Chia Plot Status) rely on the logs for their information reporting. One way to have manual logs when plotting from CLI is to use the built in "tee" command like this: 

```
.\chia.exe plots create -k 32 -n 2 -u 128 -t G:\ChiaTemp -d O:\Chia8O -r 2 -n 5 | tee -filepath $env:userprofile\.chia\mainnet\plotter\plots-5-6-2021-B.txt
```

# 2. Take a good look at log files
Your configuration and logs are found in `~\.chia\mainnet\log` and `~\.chia\mainnet\config`. You can tail your logs with `Get-Content ~\.chia\mainnet\log\debug.log -wait`. To see more of what is going on, set your log level in `config\config.yaml` to INFO from WARNING and restart. You can also use `\.chia.exe configure --set-log-level INFO` from the app directory outlined above and then restart for the changes to take effect.

# 3. Windows (auto) Update can be a pain
Consider going through a Windows Update check and install updates prior to starting a plot process. It can take a while, and updates might initiate a reboot. You can also go into Advanced Options to disable updates for up to 35 days at a time.

# 4. Disable uPnP when running multiple nodes
If you attempt to run more than one node on your local network, having uPnP on on both will cause both nodes significant confusion. You will need to use powershell to disable uPnP on all but one.

### For version 1.0:
```
    cd $env:userprofile\AppData\Local\Chia-Blockchain\app-1.1.5\resources\app.asar.unpacked\daemon\
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

# 7. Leave free space on your drive 
If you end up having your HDD or SSD have any issues that require disk repair, you will need to have free space on the disk that is larger than the largest file. For k32 plots, you will need to leave > 101 GB. This way, if your plots ever have errors (as reported by plot check tool), you will at least be able to try to repair them with CHKDSK /r. However, CHKDSK cannot repair files that are larger than the remaining free space on a drive, and will have an error