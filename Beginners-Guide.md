# The Basics of Chia to Get You Started

Chia is a new type of cryptocurrency that is based on the capacity of pre-stored random-looking data that the user creates and stores in files called _plots_. With Chia a very low resource intensive process checks plot files for _proof of space and time_. This makes Chia very fast and green. It is an improvement over proof of work blockchains, which rely on fast graphic cards and custom machines doing millions of calculations per second and wasting a lot of electricity. Chia also has many improvements to scripting, scripting environment, cryptography, usability, and scalability, and aims to be a simple, secure, and powerful blockchain. 

## How it works

The simplest way to start with Chia is to [install the Chia software](https://github.com/Chia-Network/chia-blockchain/wiki/INSTALL) on the operating system of your choice and run the GUI (Graphical User Interface). The Windows version automatically starts once installed, and the Mac just needs to be opened from the Applications directory. Once the application is opened, it loads and starts to sync with the rest of the network and blockchain. As of this writing, fully syncing can take 4-6 hours and this will increases over time as the blockchain grows. Basically, you are syncing with everyone on the network, downloading the whole blockchain, which includes all transactions ever processed on the network. The Chia blockchain database copy will be stored on your computer. Everyone with Chia installed on their computer also has a copy.

### The Basics of Farming

Once Chia is installed, users will use the Chia application to create plots. Depending on your hardware it can take 4-12+ hours to create a plot. Each approximately 101 [GiBs](https://simple.wikipedia.org/wiki/Gibibyte) in size. The user stores these plots on computer equipment and then farms the created plots for the potential to earn coins. 

Inside each plot is a large number of pre-formulated calculations stored in large table (spreadsheet) of blocks called (hash) cells. You win potential coins by providing the winning pre-formulated (hash) code to allow the transaction to occur. The winning transaction is very quickly done within 2-3 seconds and the user is compensated with coins to facilitate the transaction. Plots have many (hash) cells inside so if 1 is used there is still many left. In other words, you do not need to delete a plot if it wins you coins. Plots can keep earning you coins. Plots are estimated to last over 5 years.

### The CLI - Command-line Interface
The [Chia CLI](https://github.com/Chia-Network/chia-blockchain/wiki/CLI-Commands-Reference) is the more advanced way to use Chia. The CLI can do everything the GUI can do and more (like better utilize your hardware to optimize plotting). It is recommended you start with the GUI to understand the basics of Chia before starting to use the CLI. 

## Setting Expectations 

Once you have completed plots and are farming, you will see "Time to Win Coin" in the farming tab. Whatever time it shows here is just an estimate. It might take 3 to 4 times longer than the time shown. For example, if it shows 2 days, it might take 5 or 8 days, sometimes even more to earn your first coin or partial coin. This is normal as luck plays a role over the short run. As you add more plots, especially after about 50 plots, then the “time to win” gets a little more accurate and closer to the estimated time. 

## How to Get Help
* Read over the documents in [this Wiki](https://github.com/Chia-Network/chia-blockchain/wiki) first. Most of the information you need is here already.
* Get support on the [Chia Keybase chat channels](https://keybase.io/team/chia_network.public). #beginner and #support is where you can get help
* Submit an issue here on Github

## The GUI - Graphical User Interface

### Full Node Tab in the GUI

This shows the blockchain movement. It shows that you are in sync with the blockchain. A copy of the blockchain is stored on your computer. You have a live copy that is continually syncing with everyone else in the network.

- *Blocks* : This is the blockchain working
- *Connections* : Those are the connections to you and other users and their computer (nodes)

### Wallet tab

You will see your Chia coins here as they are won

- *History* : you can see the time/date you earned coins or partial coins

### Plots tab

This is where you create plots. The accepted plot size starts at 101 GiB each. Called a k32 - 101 GiB/109 GB plot.

- *GiB* : is gibibytes and the old school way computers measured space. The new hotness - especially from hard drive manufacturers - is to measure in gigabytes. Since gigabytes are based on 1000 and gibibytes are based on 1024, GB is always 1.074 times larger than GiB.

- *When you plot* : You can think of each plot as a collection of bingo cards, which have a chance of winning blocks, and thus winning you coins.Your computer creates these large 101 [GiB](https://simple.wikipedia.org/wiki/Gibibyte) plot files (approx 108 GB - gigabytes). Inside are large tables (like Excel spreadsheets) where each cell has a random pointer to another cell in the table. This is what the computer is doing, and why it takes so long to create the plot. It is running calculations and putting the “answers” in these millions of cells. The expected life for a 101 GiB - k32 plot to be eligible on mainnet is about 5 to 10 years, for now. There are different sizes of plots. For now, only work with k32 plots. k33 and above are not necessary. 

### Farm tab

This will show you how many plots you have created. On the top it will show how many Chias have been farmed. It also shows how many GiB of plots you have on the network. For example, you have 2 plots of 101 GiB created. Then it shows on Top left “Total size of Plots" as 0.2 [TiB](https://en.wikipedia.org/w/index.php?title=Tebibyte&redirect=no). This means you offer 0.2 TiB much storage of formulas to the Chia network. It is calculated as you have 101 GiB x 2 = 202 GiB. 

- *Latest Block Challenges* : This shows the latest challenges and signage points, which can be thought of as mini lotteries. Every 9 seconds, there is a new signage point, which means there is a new opportunity for you to check your plots and see if you've won. Every other signage point will be a winner for someone on the network so there is a new block created about every 18 seconds.

- *Last Attempted Proof* This is important: It is a 2 step process. **Step 1** plot passes filter test. **Step 2** - Selected Plot is checked for winning hash. What this looks like in the GUI: In the *Last Attempted Proof* section you have a list of five lines. Suppose you have 157 plots – each of the five lines read 0/157. Once in awhile a plot might get selected. If a plot is selected or a 2nd plot is selected that is good news and the number changes to 1/157 or 2/157 maybe 3/157. After passing the filter, each selected plot will go through a "quality lookup," which does approximately 7 reads on your plot, and tells you whether the plots have won. If you won, it does not show any indication as the transaction is done quickly. Your wallet increases.

Once that match shows on the first line, it will move down to lines 2-5, then if another plot passes step 1, it starts at the top also, and moves its way done the filter process. 

Winning is very rare, and on average one person wins in the whole world every 18 seconds. On every signage point (9 seconds), all of your plots are checked to see which ones passes the [plot filter](https://github.com/Chia-Network/chia-blockchain/wiki/FAQ#what-is-the-plot-filter-and-why-didnt-my-plot-pass-it). Approximately 1/512 of all plots will pass the plot filter for each challenge, so here you can see how many of your plots passed. However there are 4,608 chances to win 2 chia every day.

If for some reason those lines stop moving that is another indication you are not in sync with database and need to resync—see below.

# Using the GUI

## Create a plot

1. Click on green button- top right “Add a Plot”

2. Starting size plot is k 32 (101 GiB). You need a temp storage location of at least 239 GiB (256 GB) to create the plot.

3. Chose number of plots — you can select quantity to create.
    1. *Plot to Queue* : Means if (5) is selected it will plot #1, then when finished will start #2
    2. *Plot in Parallel* : means running multiple plots at same time. Make sure you have enough temp storage for combined total.
    3. *Advanced options* : The default values appear for your selected plot size, as a beginner try to leave the defaults
		1. *RAM Max Usage*: More memory slightly increases speed, if you assign too little (less than 4,000 for a k 32) or too much (more than you will have available) the plot might fail during the process.
		2. *Number of Threads*: Default is 2.
		3. *Buckets*: Default is 128, more buckets decreases the amount of RAM required and usually increases the speed of plotting.
		4. *Queue name*: This is useful to mix parallel and series plotting. IE: If you want to do 2 plots at a time, 10 plots in total, you can make 5 plots to Queue name: "My First Queue" and after that add another 5 plots to Queue name: "My Second Queue".

4. Select Temporary Directory: This is where plots are created. About 128 temp files will be created (depending on buckets), then compacted to one plot file. This creation grows to 239 GiB (256 GB) and when finished it will be compressed to a single k 32 plot (101 GiB).
    1. It's recommended to use a SSD drive or NVME drive for this work but make sure you are aware of [SSD Endurance](https://github.com/Chia-Network/chia-blockchain/wiki/SSD-Endurance).

5. Select Permanent directory—once the plot is created—it will go to this location to be farmed to earn chia coins. Storage will fill quickly due to size of plot. Storage can be internal or usb connected drives. Networked drives can work but could congest your local network or be to slow to respond for rewards (should be less than 30 seconds). Plan ahead—storage fills quickly.

6. Click create plot to start process.

#### How Plots are created

Creating a plot is time consuming with an average of 9-20 hours on a normal computer and 4-8 hours on a high end machine. There are 4 phases that does operations in 7 tables.

Phases:
1. **Computing tables 1 to 7:** It creates the buckets (default: 128) as files on your temp directory, when the 7 tables are computed the plot time progress is about **42%**
2. **Back propagation tables 7 to 1:** When the 7 tables are back propagated the plot time progress is about **61%**
3. **Compression of tables 1 to 7 in pairs:** When the 7 tables are compressed the plot time progress is about **98%**
4. **Write checkpoint tables:** Transfers your plot to your permanent drive. It will delete all the files in your temp storage and this completes the progress to **100%**

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

#### Notes
The suggestion is to use an added SSD or NVME storage to create plots and not the primary hard drive (specially for non replaceable NVME like on some Macs or Windows Laptops)
If for some reason a plot fails to complete- it has to be deleted by deleting all of its temp files. Be careful that you don't delete the temp files of another plot that's being plotted.

# Glossary

`Proof`: It is found inside the PLOT. Millions of "excel blocks" with formulas called proofs of space.
The Chia software is designed to work on a lottery system. A key element to winning the lottery of earning coins is that the more plots you have, the more proofs you have, and therefore the higher chances of winning. Someone with 1% of all the plotted space, will win about 1% of all the blocks. There are about 4608 blocks per day, each with 2 chia, so 9216 chia are created per day.

The Chia farming software gets a challenge, lets say 2021. It’s going to look through lookup tables on the front of the plots. Find the closest to 2021 and this is where the time comes into play-- That proof (excel block hash) has a certain quality, and only proofs that are of a certain quality or better are eligible for winning.

What makes Chia different from proof of work blockchains— is the consensus algorithm called proof of space and proof of time. Basically as after the farmer creates a proof of space and a block, other computers called timelords add proofs of time to the block, which is a cryptographic proof that says that a certain amount of time (like 30 seconds) has passed. So instead of the whole world mining at the same time, only a few computers are "mining" for each proof of space that won. Since these are all cryptographic proofs, they cannot be forged or broken, making the consensus extremely secure.

In Chia, the only electricity required is the electricity to create the plots, and to run the hard drives, which is on the order of 10 watts to power, plus CPU power required to run a full node (which is very light). In comparison Blockchains like Bitcoin and Ethereum rely on huge farms of GPUs ( 300W each GPU), or ASICs (hundreds or thousands of watts per machine) to secure the blockchain. You can think of proof of work, as millions of computers "making" lottery tickets by using electricity, but each ticket can only be used once. Chia will use vastly less electricity as each plot will last over 5 years, and the only electricity required is the initial setup (plotting) and 10W for farming a drive.

#### Computer Hardware

`SSD drivers`: Many people are using SSD drives to create plots— or NVME drives. Minimum of 1 Tb each is recommend if you want to plot more than 1 plot at a time.

On an older computer you can purchase a pcie adapter card-to take NVME/SSD drive as the new temp folder. This is internal to the PC. Some have tried to use a usb or firewire attached NVME/ SSD with some success. The first plot is sometimes created with 6 hrs each, but doing multiple plots slows to 8 hrs each. This pertains to creating the temp files.

One item to plan for is storage of plots as they fill storage quickly. As more plots are created, the discussions start to turn to terabytes of data storage not gigs of storage. Many use usb attached external drives for storage, or internal drives or external rack storage with many drives.

**Sync Issues – look on Full Node tab**

Look at the date/time indicated compared to your computer. If there is a 30 min difference and it has not caught up—on the windows menu—click View/Force reload. It will take 5 minutes and should restart the sync mode. It will make you click your key code button again to get in. It does not affect the plotting you are doing.

**Groups to join on the [_chia_network.public_ Keybase](https://keybase.io/team/chia_network.public) chat:**

* \#announcements  -- most important as it tells you of new versions
* \#beginner  -- read this one from the start -- many questions are answered
* \#farming hardware
* \#general
* \#plotting-hardware
* \#random 
* \#testnet 

## Frequently Asked Questions

First there is a [FAQ](https://github.com/Chia-Network/chia-blockchain/wiki/FAQ).

If power goes out what happens. Answer-  In the chia software, the plots that currently being built are now non operational. When you start Chia they will be gone and need to be restarted. One item to do is to go into your temporary storage and delete all those .tmp files it created. This is the one time it will not self delete those files if a new plot is started. Not deleting them may cause that storage device to run out of room when it starts 1 or multiple plots. All your existing plots are "safe" in their existing storage. 

Can I use USB 3.0 cable connected to SSD/NVME running the Temp files. Answer- On Windows - it has not worked well- the communication speed is not fast enough, sometimes the usb turns off, then the plot is not useable. It's possible to run 1 plot, but limiting when trying to process multiple plots. Most are installing PCIe adapters to SSC/NVME and that solves the issue. The mac has very fast communication to do the first plot, - many others are saying that they can do 2 plots but process time increases dramatically. Technology is constantly changing so continue to do research and ask in the chat rooms.

Once a hash is used from a plot-- does it need to be deleted. Answer-- no-- an example is , a plot has 300,000 hashes that the user created. If one is used, there are enough to last an estimated 5 years. 

Farmer vs Harvester: Harvester checks the plots and reports the results to the farmer, farmer then submits the results to the blockchain

### Peak Blockchain and checking if Chia is in sync

*The chia blockchain software* : Every user has copy of the blockchain on their PC and the goal is that everyone is in sync or very close. Click on the Full Mode tab, scroll down to see the connected Nodes/ PC. If the time and date is off by 30 minutes then the software is not synced to blockchain or others. Multiple ways to check-- on Full node -- look at peak height and date/time--- then go below and look at connections and your peak should match the other computers. Also the peak should be close when you click on wallet tab with that wallet peak number. The wallet peak number sometimes is off by 10 numbers. If off by several hundred it's not in sync. 

On the GUI-- on the top menu bar -- click on View then Force Reload. It will take about 5-10 minutes, to re-sync. Your keys will come up again to click to enter Chia. It will not affect your plotting.

### How to tell if Chia is working on Windows GUI

When you are first operating Chia and wondering if the software is working- here is some information to keep you informed.
All of the chia programming is in a file called config.yaml located in c:/Users/ (Your username)/.chia/mainnet/config.yaml

Shut down Chia software before config access.
Open Config with notepad. In the middle  * log_level: WARNING * change wording of WARNING to INFO. 
 Save File and exit. Start up Chia-- give it 20 minutes to run

Can access the log files and read activity, while Chia is running. It's located in c:/Users/ (Your username)/.chia/mainnet/log/debug.log
Log files are very informative. Once a log fills to 20mb another is created. If there are too many you can delete some of them.

Inside what you are looking for are these lines
*07:02:41.663 harvester src.harvester.harvester : INFO     1 plots were eligible for farming f53c496e80... Found 0 proofs. Time: 0.00500 s. Total 8 plots*

This means Chia is working--  The filter system is 2 parts. Chia found that 1 plot passed the (1st) part, now it looks inside to determine if a pre-formulated "proof" will be able to do a transaction in fastest time (2-3 seconds) if it secures one in your plot then you win 1 proof means you won a coin. Many times it will say 0 proofs. But it shows it's working. This is where luck/time comes into play. At the end of that line it will indicate how many plots the software registers.

### What is normal information in a log file

Below is a copy of normal information from a  log file. :
- 
***
*9:32:00.322 full_node full_node_server        : INFO     <- new_signage_point_or_end_of_sub_slot from peer 68b376e5846696df3510822ea527d0899ac6183f261e8858119235cd24903720 193.91.103.92.*-

***

*9:32:00.278 farmer farmer_server              : INFO     <- new_signage_point from peer 62d37909657e183dcd702b66d0e694474f907361f5981eceaba00878e84419c4 127.0.0.1.*
***

*09:32:01.806 full_node full_node_server        : INFO     -> respond_peers to peer 202.185.44.200 e5b7f06ba6ece8698917e0e22971aef8602972de81efe379d693b2baa0dffc24.*
***

*09:32:08.063 full_node full_node_server        : INFO     -> request_signage_point_or_end_of_sub_slot to peer 74.138.106.114 b567363c3a96c13366ef2dbff2e080da77f310875a8beda7c1c07246173c3a06.*
***

*09:32:08.202 harvester harvester_server        : INFO     <- new_signage_point_harvester from peer 5bfd9af9bc76270cf76746255db9a435dca56b9adb37f5d1daec71e3c699c807 192.168.0.44.*
***

*09:32:08.211 harvester src.harvester.harvester : INFO     0 plots were eligible for farming fec1fff66e... Found 0 proofs. Time: 0.00200 s. Total 8 plots.*
***


The last line again shows at that current time of 09:32:08.211- that on this machine, of the 8 plots farming 0 plots were eligible. It still means the software recognized the plots and it's working.


Everyone is very helpful to answer questions. The group does ask for questions to be in the selected chat room. Beginner questions in Beginner etc.

