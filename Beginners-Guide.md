# Basic information about Chia to get started

Chia is a new type of cryptocurrency that is based on capacity of pre-stored random looking data that the user creates and stores in files called plots, making the blockchain consensus extremely fast/green. It is an improvement over proof of work blockchains, which rely on fast graphic cards and custom machines doing millions of calculations per second, and wasting a lot of electricity. Chia also has many improvements to scripting, scripting environment, cryptography, usability, and scalability, and aims to be a simple, secure and powerful blockchain. 

## How it works

You can load Chia software on Windows, Mac or Linux. The Windows version automatically starts on installation and the Mac just needs to be opened from the Applications directory - it loads and starts to sync with rest of network and blockchain. Fully syncing can take 4-6 hrs, although this increases over time. Basically you are syncing with everyone on the network, downloading the whole blockchain, which includes all transactions ever processed on the network. The chia blockchain database will be stored on your computer.

### Full Node Tab

This shows the blockchain movement. It shows that you are in sync with the blockchain. A copy of the blockchain is stored on your pc. You have a live copy that is in sync with everyone else.

- *Blocks* : This is the blockchain working
- *Connections* : Those are the connections to you and other users

### Wallet tab

You will see your Chia coins as they are won

- *History* : you can see the time/date you earned coins or partial coins

### Plots tab

This is where you create plots. The accepted plot size starts at 101 GiB each. Called a k32 - 101 GiB/109 GB plot.

- *GiB* : is gibibytes and the old school way computers measured space. The new hotness - especially from hard drive manufacturers - is to measure in gigabytes. Since gigabytes are based on 1000 and gibibytes are based on 1024, GB is always 1.074 times larger than GiB.

- *When you plot* : Your computer creates these large 101 GiB files. Inside are large tables (kind of like excel sheets) where each cell has a random pointer to another cell in the table. This is what the computer is doing, and why it takes so long to create the plot. It is running calculations and putting the “answers” in these millions of cells. The expected life for a k32 plot to be eligible on mainnet is about 7 to 15 years, for now. You can think of each plot as a collection of bingo cards, which have a chance of winning blocks.

### Farm tab

This will show you how many plots you have created. On the top it will show how many Chias have been farmed.  It also shows how many gigs of plots you have on the network. If you have 2 plots of 101 GiB created. Then it shows on Top left “Total size of Plots” .2 TIB, means you offer this much storage of formulas to the chia network. It is calculated as you have 101 GiB x 2 = 202 GiB. 

- *Latest Block Challenges* : This shows the latest challenges and signage points, which can be thought of as mini lotteries. Every 9 seconds, there is a new signage point, which means there is a new opportunity for you to check your plots and see if you've won. Every other signage point will be a winner for someone on the network so there is a new block created about every 18 seconds.

- *Last Attempted Proof* : This is important. There are 5 lines. Currently if you have 157 plots – so each of the lines read  0/157.
On every signage point (9 seconds), all of your plots are checked to see which ones passes the [plot filter](https://github.com/Chia-Network/chia-blockchain/wiki/FAQ#what-is-the-plot-filter-and-why-didnt-my-plot-pass-it). Approximately 1/512 of all plots will pass the plot filter for each challenge, so here you can see how many of your plots passed. If a plot is selected or a 2nd plot is selected that is good news and the number changes to 1/157 or 2/157 maybe 3/157. However a plot passing the plot filter does not mean that you have won a block. After passing the filter, each plot will go through a "quality lookup", which does approximately 7 reads on your plot, and tells you whether the plots have won. Winning is very rare, and on average one person wins in the whole world, every 18 seconds. If you won, the whole plot is looked up on disk, and you will see the chia farmed on the Wallet screen. However there are 4,608 chances to win 2 chia every day.

Once that match shows on the first line, it will move down to lines 2-5 , as the blockchain moves. If for some reason those lines stop moving that is another indication you are not in sync with database and need to resync—see below.

## Create a plot

1. Click on green button- top right “Add a Plot”

2. Starting size plot is k 32 (101 GiB). You need a temp storage location of at least 332 GiB (357 GB) to create the plot.

3. Chose number of plots — you can select quantity to create.
    1. *Plot to Queue* : Means if (5) is selected it will plot #1, then when finished will start #2
    2. *Plot in Parallel* : means running multiple plots at same time. Make sure you have enough temp storage for combined total.
    3. *Advanced options* : The default values appear for your selected plot size, as a beginner try to leave the defaults
		1. *RAM Max Usage*: More memory slightly increases speed, if you assign too little (less than 4,000 for a k 32) or too much (more than you will have available) the plot might fail during the process.
		2. *Number of Threads*: Default is 2.
		3. *Buckets*: Default is 128, more buckets decreases the amount of RAM required and usually increases the speed of plotting.
		4. *Queue name*: This is useful to mix parallel and series plotting. IE: If you want to do 2 plots at a time, 10 plots in total, you can make 5 plots to Queue name: "My First Queue" and after that add another 5 plots to Queue name: "My Second Queue".

4. Select Temporary Directory: This is where plots are created. About 128 temp files will be created (depending on buckets), then compacted to one plot file. This creation grows to 332 GiB (357 GB) and when finished it will be compressed to a single k 32 plot (101 GiB).
    1. Its recommend to use a SSD drive or NVME drive for this work but make sure you are aware of [SSD Endurance](https://github.com/Chia-Network/chia-blockchain/wiki/SSD-Endurance).

5. Select Permanent directory—once the plot is created—it will go to this location to be farmed to earn chia coins. Storage will fill quickly due to size of plot. Storage can be internal or usb connected drives. Networked drives can work but could congest your local network or be to slow to respond for rewards (should be less than 30 seconds). Plan ahead—storage fills quickly.

6. Click create plot to start process.

#### How Plots are created and 7 steps Process

Creating a plot is time consuming.  Average 9-20 hours on a normal computer, and 4-8 hours on a high end machine. There are 4 phases that does operations in 7 tables.

Phases:
1. **Computing tables 1 to 7:** It creates the buckets (default: 128) as files on your temp directory, when the 7 tables are computed the plot time progress is about **42 %**
2. **Back propagation tables 7 to 1:** When the 7 tables are back propagated the plot time progress is about **61 %**
3. **Compression of tables 1 to 7 in pairs:** When the 7 tables are compressed the plot time progress is about **98 %**
4. **Write checkpoint tables:** Transfers your plot to your permanent drive. It will delete all the files in your temp storage and this completes the progress to **100 %**

| Phase | Step                       | % Progress |
| :---: | :------------------------- | ---------: |
| 1     | Computing table 1          |         1% |
| 1     | Computing table 2          |         6% |
| 1     | Computing table 3          |        12% |
| 1     | Computing table 4          |        20% |
| 1     | Computing table 5          |        28% |
| 1     | Computing table 6          |        36% |
| 1     | Computing table 7          |        42% |
| 2     | Backpropagating on table 7 |        43% |
| 2     | Backpropagating on table 6 |        48% |
| 2     | Backpropagating on table 5 |        51% |
| 2     | Backpropagating on table 4 |        55% |
| 2     | Backpropagating on table 3 |        58% |
| 2     | Backpropagating on table 2 |        61% |
| 3     | Compressing tables 1 and 2 |        66% |
| 3     | Compressing tables 2 and 3 |        73% |
| 3     | Compressing tables 3 and 4 |        79% |
| 3     | Compressing tables 4 and 5 |        85% |
| 3     | Compressing tables 5 and 6 |        92% |
| 3     | Compressing tables 6 and 7 |        98% |
| 4     | Write checkpoint tables    |       100% |

Notes-
The suggestion is to use an added SSD or NVME storage to create plots and not the primary hard drive (specially for non replaceable NVME like on some Macs or Windows Laptops)
If for some reason a plot fails to complete it has to be deleted by deleting all of its temp files. Be careful that you don't delete the temp files of another plot that's being plotted.

# Glossary

*Proof* : It is found inside the PLOT.  Millions of "excel blocks" with formulas called proofs of space.
The chia software is designed to work on a lottery system.  A key element to winning the lottery of earning coins is that the more plots you have, the more proofs you have, and therefore the higher chances of winning.  Someone with 1% of all the plotted space, will win about 1% of all the blocks. There are about 4608 blocks per day, each with 2 chia, so 9216 chia are created per day.

The chia farming software gets a challenge, lets say 2021.  It’s going to look through lookup tables on the front of the plots.  Find the closest to 2021 and this is where the time comes into play-- That proof (excel block hash) has a certain quality, and only proofs that are of a certain quality or better are eligible for winning.

What makes Chia different from proof of work blockchains— is the consensus algorithm called proof of space and proof of time.  Basically as after the farmer creates a proof of space and a block,  other computers called timelords add proofs of time to the block, which is a cryptographic proof that says that a certain amount of time (like 30 seconds) has passed. So instead of the whole world mining at the same time, only a few computers are "mining" for each proof of space that won.  Since these are all cryptographic proofs, they cannot be forged or broken, making the consensus extremely secure.

In Chia, the only electricity required is the electricity to create the plots, and to run the hard drives, which is on the order of 10 watts to power, plus CPU power required to run a full node (which is very light).  In comparison Blockchains like Bitcoin and Ethereum rely on huge farms of GPUs ( 300W each GPU), or ASICs (hundreds or thousands of watts per machine) to secure the blockchain. You can think of proof of work, as millions of computers "making" lottery tickets by using electricity, but each ticket can only be used once. Chia will use vastly less electricity as each plot will last over 5 years, and the only electricity required is the initial setup (plotting) and 10W for farming a drive.

**Computer Hardware**

*SSD drivers* : Many people are using SSD drives to create plots— or NVME drives.  Minimum of 1 Tb each is recommend if you want to plot more than 1 plot at a time.

On an older computer you can purchase a pcie adapter card-to take NVME/SSD drive as the new temp folder.   This is internal to the PC.  Some have tried to use a usb or firewire attached NVME/ SSD with some success.  The first plot is sometimes created with 6 hrs each, but doing multiple plots slows to 8 hrs each. This pertains to creating the temp files.

One item to plan for is storage of plots as they fill storage quickly.  As more  plots are created, the discussions start to turn to terabytes of data storage not gigs of storage.  Many use usb attached external drives for storage, or internal drives or external rack storage with many drives.

**Sync Issues – look on Full Node tab**

Look at the date/time indicated compared to your computer.  If there is a 30 min difference and it has not caught up—on the windows menu—click View/Force reload.   It will take 5 minutes and should restart the sync mode.  It will make you click your key code button again to get in.  It does not affect the plotting you are doing.

**Groups to join on Keybase chat:**

* \#announcements   -- most important as it tells you of new versions
* \#beginner  -- read this one from the start -- many questions are answered
* \#farming hardware
* \#general
* \#plotting-hardware
* \#random 
* \#testnet 

*\* Important*

When first plotting --   and your first plot is created and now being farmed for chia.  On the farming tab—“Time to Win a coin”— it may say 2 days.   It may take really up to 5 days to earn the first coin or partial coin.  It is normal as luck plays a role over the short run. Your computer is being linked up to all the other pc’s.   As you add more plots—especially after about 50 plots, then the “time to Win” gets a little closer to estimated time.

## Frequently Asked Questions

First there is a [FAQ](https://github.com/Chia-Network/chia-blockchain/wiki/FAQ).

If power goes out what happens.   Answer-  In the chia software, the plots that currently being built  are now non operational.  When you start Chia they will be gone and need to be restarted.  One item to do is to go into your temporary storage and delete all those .tmp files it created.  This is the one time it will not self delete those files if a new plot is started.  Not deleting them may cause that storage device to run out of room when it starts 1 or multiple plots.   All your existing plots are "safe" in their existing storage. 

Can I use USB 3.0  cable connected to SSD/NVME running the Temp files.  Answer- On Windows - it has not worked well- the communication speed is not fast enough, sometimes the usb turns off, then the plot is not useable. It's possible to run 1 plot, but limiting when trying to process multiple plots.  Most are installing PCIe adapters to SSC/NVME and that solves the issue.  The mac has very fast communication to do the first plot, - many others are saying that they can do 2 plots but process time increases dramatically.  Technology is constantly changing so continue to do research and ask in the chat rooms.

Once a hash is used from a plot-- does it need to be deleted.  Answer-- no-- an example is , a plot has 300,000 hashes that the user created.  If one is used, there are enough to last an estimated 5 years. 

### New version RC4

The new RC4 version is a new blockchain-- prior coins are erased.  It's all test chia coins right now anyway.   It may take 4-10 hrs to sync-- just let it run-- there may be multiple other revisions coming example 1 per week until final.  When RC4 is loaded-- a new feature is that it waits to download a large file from other PC's then starts to sync-- so that is why the sync will take some time.

### Peak Blockchain and checking if Chia is in sync

*The chia blockchain software* : Every user has copy of the blockchain on their PC and the goal is that everyone is in sync or very close. Click on the Full Mode tab. If the time and date is off by 30 minutes then the software is not synced to blockchain or others.  Multiple ways to check-- on Full node -- look at peak height and date/time--- then go below and look at connections and your peak should match the other computers.   Also the peak should be close when you click on wallet tab with that wallet peak number. The wallet peak number sometimes is off by 10 numbers. If off by several hundred its not in sync. On the GUI-- on the top menu bar -- click on View then Force Reload. It will take about 5-10 minutes.  It will not affect your plotting.

Everyone is very helpful to answer questions. The group does ask for questions to be in the selected chat room. Beginner questions in Beginner etc.

