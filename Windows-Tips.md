1. To plot in parallel in Windows you will need to use powershell.
    ```
    cd C:\Users\yourUserName\AppData\Local\Chia-Blockchain\app-1.0.0\resources\app.asar.unpacked\daemon\
    start-process .\chia.exe -argumentlist "plots create yourParametersGoHere"
    start-process ....
    ....
    ```
    A specific example:
    ```
    cd C:\Users\yourUserName\AppData\Local\Chia-Blockchain\app-1.0.0\resources\app.asar.unpacked\daemon\
    start-process ./chia.exe -argumentlist "plots create -k 32 -b 4000 -u 128 -r 4 -t d:\tempdrive1 -2 e:\tempdrive2 -d F:\plots -n 1"
    ```
    The command above makes one plot (specified by `-n 1`), to plot in parallel you need to open a new powershell and repeat the command (without closing the first one). 

2. Your configuration and logs are found in `~\.chia\mainnet\log` and `~\.chia\mainnet\config`. You can tail your logs with `Get-Content ~\.chia\mainnet\log\debug.log -wait`. To see more of what is going on, set your log level in `config\config.yaml` to INFO from WARNING and restart. You can also use `\.chia.exe configure --set-log-level INFO from the app directory outlined above and then restart for the changes to take effect.

3. Consider going through a Windows Update check and install updates prior to starting a plot process. It can take a while, and updates might initiate a reboot.

4. If you attempt to run more than one node on your local network, having uPnP on on both will cause both nodes significant confusion. You will need to use powershell to disable uPnP on all but one. An example for RC 9:
    ```
    cd C:\Users\yourUserName\AppData\Local\Chia-Blockchain\app-1.0.0\resources\app.asar.unpacked\daemon\
    ./chia.exe configure --upnp-enable False
    ```

5. Sometimes your wallet database can get corrupted. If you get stuck on the "Connecting to wallet" spinner for more than 60 seconds, you will probably want to exit the app, delete your wallet database with powershell, and then start the app again.
    ```
    # For 1.0:
    cd ~\.chia\mainnet\wallet\db
    del *.db
    ```

6. If your Powershell plotting processes seem to pause you should disable Quick Edit. Powershell -> Properties -> Options: Disable quick edit. If you are still seeing this symptom it is almost always a [problem with your RAM](https://www.tomshardware.com/how-to/how-to-test-ram).