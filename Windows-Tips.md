1. To plot parallel in Windows you will need to use powershell. A generic example for Beta 16:
> ```
> cd C:\Users\yourUserName\AppData\Local\Chia-Blockchain\app-0.1.16\resources\app.asar.unpacked\daemon\
> start-process ./chia.exe -argumentlist "plots create yourParametersGoHere"
> start-process ....
> ....
> ```
>
> A specific example, again using Beta 16:
> ```
> cd C:\Users\yourUserName\AppData\Local\Chia-Blockchain\app-0.1.16\resources\app.asar.unpacked\daemon\
> start-process ./chia.exe -argumentlist "plots create -k 32 -b 4508 -u 128 -r 4 -t d:\tempdrive1 -2 e:\tempdrive2 -d F:\plots -n 1"
> ```
> If you are using Beta 16 you would change the `Chia-Blockchain\app-0.1.16\resources\` section to `Chia-Blockchain\app-0.1.16\resources\` in the command above.

2. Windows and Python do not get along regarding reporting CPU % so you have to ignore it on Windows.

3. You configuration and logs are found in `~\.chia\VERSION\`. You can tail your logs with `Get-Content ~\.chia\VERSION\log\debug.log -wait`. To see more of what is going on, set your log level in `conf\config.yaml` to INFO from WARNING and restart.

4. Consider going through a Windows Update check and install updates prior to starting a plot process. It can take a while, and updates might initiate a reboot.