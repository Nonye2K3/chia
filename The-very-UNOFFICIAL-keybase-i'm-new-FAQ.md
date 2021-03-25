Based on questions and answers from the beginner keybase chat who are NOT employees or have any say, your mileage may vary.

What is chia?
A blockchain like Bitcoin or Etherium but better.
Much less electricity
More decentralized
Simpler smart contracts

When does it's blockchain mainnet start?
It started farming rewards on Mar 19 2021

How do I get chia?
The 1.0.0 or 1.0.1 software is running and anyone who makes a plot file can farm it and have a chance at winning chia reward tokens.
Transactions are due to start at a certain block about 6 weeks from launch, or adjusted soon by a soft fork update (not breaking the chain).

How do I get started?
You need to download the software, make .plot files of size k32 to fill up any free space you have and then leave them 'farming'.
[Quick Start Guide](https://github.com/Chia-Network/chia-blockchain/wiki/Quick-Start-Guide)
Dont worry about other K sizes, K 32 is minimum, larger doesn't have an advantage.
[k sizes](https://github.com/Chia-Network/chia-blockchain/wiki/k-sizes)

Can I just buy chia? Until transactions start you can't securely spend chia. Any deals before then are off-chain and no guarantees about safety, just a person to person handshake worth of trust. 

When will it be on exchanges? The people who run the Chia Networks company have said 
"Please check out the Business Whitepaper and the Zooms - https://www.chia.net/2021/02/10/chia-businesss-whitepaper.html"
Until transactions start, nobody can transfer any chia with the security of the blockchain and it can't be listed on exchanges.

If you join KeyBase.io chia_network.public then don't @ people directly and join #general to talk about Chia, join #beginner for noob questions, join #support for problems with the software.

Plot issues?
to plot you need 357gb of temporary space, faster the better. This is the command option -t or temp space.
Hard drives are slower than sata ssd are slower than m.2 or nvme drives are slower than datacenter pcie nvme ssds.
you need final storage for all the plots you want to farm long term, slow usb drives are okay.

Don't try to plot over USB, it's too slow (if you're an expert usb3 gen2 is looked at?)

A GPU wont help you, this isn't mining like other cryptos, it's making saving challenges that could win to a file and compressing it then leaving it to farm at an idle long term. 
"plotting is very CPU<>RAM bus intensive and use large data elements making the GPU struggle to do anything with it and especially struggle with the memory speed"

Plotting takes CPU, RAM and temp space resources, farming long term takes very little resources.

Don't use your boot drive to plot, consumer SSD wears out fast, plotting reads and writes a LOT and you can kill drives by using them to plot.

Don't use cloud services (as a beginner, otherwise do what you want, i'm not a cop), it's not worth the costs to move the files.

Only wrote 1835008 of 4194288 bytes at offset 1421863632 to "/Volumes/mydrive/chia_folder/plot-k32-2021-03-24-13-27-##########.plot.p1.t6.sort_bucket_019.tmp"with length 1423704064. Error 1. Retrying in five minutes.
what does this mean?
It means you ran out of space or drive disconnected or something like that. Plot has probably failed, fix the issue and start again.

A failed plot does NOT clean up itself. You need to delete the temp files before starting up again.

A failed plot, interrupted plot can't be resumed (for now, maybe someday). Delete the temp files and start over.

Can I use a raspberry pi? 
Not to make plots, it's really too slow. People are using Raspberry Pi and to farm though. 

Windows CLI questions?
[windows tips and tricks](https://github.com/Chia-Network/chia-blockchain/wiki/Windows-Tips-&-Tricks)
it's hiding in C:\users\%username%\AppData\Local\chia-blockchain\app-<version>\resources\app.asar.unpacked\daemon
then .\chia -h

