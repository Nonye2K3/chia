# Basic information about Chia to get started

Chia is a new type of crypto currency—basic information to get started.   The software is still in development so is evolving

## How it works

You can load Chia software on Windows or Mac. The Windows version automatically starts on installation and the Mac just needs to be opened from the Applications directory - it loads and start to sync with rest of network and blockchain. Fully syncing takes 4-6 hrs.     Basically you are syncing with everyone on the network.  The chia blockchain database will be stored on your pc also

**Full Node Tab**—this shows the block chain movement.  It shows that you are sync with the block chain.  A copy of the block chain is stored on your pc.  You have a live copy that is in sync with everyone else.

- Blocks—that is the block chain working
- Connections—those are the connections to you and other users--  those connections make Chia much faster to process transactions—part of the design

**Wallet tab** – you will see your Chia coins as they are won-- this area is still under development

* History—you can see the time/date you earned coins or partial coins

**Plots tab**-- This is where you create plots.   The accepted plot size starts at 101 gigs each.  Called a 32 k- 101 gig plot.    When you plot—your computer goes and creates these large 101 gig  files.  Inside are excel sheets and each cell has a “hash” formula called transaction proof.  This is what the computer is doing, why it takes so long.  It’s running formula calculations and putting the “answers” in these millions of cells.   That is why it is 101 gigs.  The expected life for a k32 plot to be eligible on mainnet is about 7 to 15 years of usage, for now.

**Farm tab**- this will show you how many plots you have created.  On the top it will show how many Chias have been farmed.  It also shows how much gigs of formulas you have on the network.  If you have 2 plots of 101 gig created. Then it shows on Top left “ Total size of Plots”  .2 TIB, means you offer this much storage of formulas to the chia network.  Means you have 101 gigs x 2 = 202 gigs.  The counts are in terabytes so you need at least 10 plots to pass 1 terabyte to equal 1TIB

* Latest Block Challenges—this shows block chain and order
* “ Last Attempted Proof”_ — ok this is important.    There are 5 lines.  Currently I have 157 plots – so each of the lines read  0/157.   If a plot is selected  or a 2nd plot is selected that is good news and the number change to 1/157 or 2/157 maybe 3/157.   It just means that plot was a close fit and the software is deciding if your hash formulas inside are a match for Chia to process the transaction and have the account earn chia.  A match does not mean money.  Once that match shows on the first line--  it will move down to lines 2-5 , as the block chain moves.   If for some reason those lines top moving--- that is another indication you are not in sync with database and need to resync—see below


\* Note

**Create a plot**

Click on green button- top right “Add a Plot”

- Starting size plot is 101 gigs.  You need a temp storage location of at least 340 gigs to create the plot.
- Chose number of plots—you can select qty to create
- “ Plot to Queue” means if (5) is selected it will plot #1, then when finished will start #2
- Plot in Parallel—means running multiple plots at same time.  Make sure you have enough temp storage for combined total.
- Under “number of plots” – click drop down.  One item to change is “buckets” select qty-128 manually.  It makes the process run slightly faster by about 20 minutes.
- #3 Select Temp Directory—select your temporary directory.  This is where plots are created.  They will go to 330 gigs in size and then when finished will erase all files. They were temp files

- Its recommend to use a SSD drive or NVME drive for this work but make sure you are aware of [SSD Endurance](https://github.com/Chia-Network/chia-blockchain/wiki/SSD-Endurance).

- #3 Select Permanent directory—once the plot is created—it will go to this location to be farmed to earn chia coins.    Storage will fill quickly due to size of plot. Storage can be internal or usb connected drives.  Plan ahead—storage fills quickly
- Click create plot to start process

**When a plot is created.   There are 2 directories.  Temp storage and then permanent file.**
The plot will be created in the temp storage folder- lots of them labled .tmp- over 100 files will be created all labled.tmp-. The temp storage will grow to use a minimum to be created 340 gigs per created plot.    Once the plot is created, software creates (1) file with 101 gigs with a name and end will say .plot (example agljglhjaw[jufpoierh[wjrgpoeiwh.plot )- the software will transfer it to the permanent file and its size will be 101 gigs.  This is what will be farmed to earn chia coins.

**Plotting time**

On for a basic computer like mine- it takes about 10 hours to create 1 plot of 101 gigs.

Once plotting start it will show on the Plot page in an area called “Local Harvester Plots”

Under status—there are 3 dots – click on logs to see the status of that plot.   Look below on this sheet and it explains the steps the plotting process got thru to be created.

**Word definitions**

Proof in the Last Attempted Proof box — that is a Proof of Space and that is what is inside the PLOT.  Millions of "excel blocks" with formulas called proofs of space.

The chia software gets a proof number lets say 2021.  It’s going to look thru people that have the most plots first to find the closest to 2021 and this is where the time comes into play-- that is within 2-3 seconds to do the calculations.   If another block is 2025- but has a time calculation of 5 hours it skips it over.

The chia software is designed to work on a lottery system.  A key element to winning the lottery of earning coins is that the software wants a “winner” to have a lot of plots made so it can go thru all the data quickly to answer the proof.  Proofs are the acceptance of the transaction and then move on.

When Chia is first loaded

The software will sync with everyone found on the Full node tab—takes about 4-6 hrs. to sync.

When you create a plot—it’s very RAM intensive and uses the hard drive— A windows PC with 16 gigs of RAM and normal hard drive --- each 101 gig plot taking around 24-30  hrs. to create.  I now upgraded my pc to 64 gigs and plot time takes 10 hrs. Est.

When a chia transaction occurs – its time stamped  in the front– the software goes and looks for a farmed plot with lot of formulated cells—the larger and more plots available on a computer allows it to process the transaction with all the formulas from the cells.  Once all the verifications are done, another computer called a “timelord” puts another time stamp on it at the end and checks the transaction and sends it to its destination.  

 What makes Chia different from block chain— is the verification called proof of space and proof of time.  Basically as the block chain is going to the transaction process the timestamp in the beginning- time codes it.  Then another computer called a timelord—goes and puts a proof stamp on the transaction to say—every digit that passed thru—was all within this time slot, and similar to a gps location stamp also—so its verified and then the computer puts another time stamp on the end.  So each transaction has 3 verifications—making it extremely hard to crack or make changes, making it very secure

As a transaction goes to a computer with lot of plots—since all the plots have all the calculation formulas already done—the entire transaction is done quickly and moves on.

The chia software gets a proof number let’s say 2021.  It’s going to look thru people that have the most plots first to find the closest to 2021 and this is where the time comes into play--  that is within  2-3 seconds to do the calculations.   If another block is 2025- but has a time calculation of 5 hours it skips it over.

Bitcoin on the other hand- goes to a winning computer—and it stops and does all the transactions—using a lot of electricity and computer resources.

**How Plots are created**

Creating a plot is time consuming.  Average 10-20 hours.  Here are the approx. 7 steps/ tables are created to create a plot.  
Table 1 is quickly created within 2 minutes.

On table 2 you will start to see it create the buckets.    Most of the programmers are telling us to select 128 buckets as it uses the least amount of ram.  Plot size is still 101 gigs.   I have noticed it also helps create the plot faster, then selecting 32 buckets or leaving the selection at 0.

Table 4 takes some time.

After 7 tables are created it starts to “backpropogate” again tables 1-7.  I think this is a double check
Then it start a compression stage of each table manually 1-7

Then it shows it’s completed- 101 gigs but nothing happens.  At this stage it is transferring your 101 gigs of plot to your permanent drive area—takes a few minutes 5-10 minutes.   It will delete all the files in your temp storage and there you go- 10 hrs. later.

**Computer Hardware**

SSD drivers—many people are using SSD drives to create plots— or NVME drives.  Minimum of 1 Tb each is recommend if you want to plot more than 1 plot at a time

I now use a pcie card- with NVME attached as my temp folder.

I had a remote SSD drive as the temp folder drive and the PC did not like it(too slow for Chia).  So I added a PCIE adapter card with NVME drive it now works. I use an old pc.

One item to plan for is storage of plots as they fill storage quickly.  As more are created, the discussions start to turn to terabytes of data storage not gigs of storage

Sync Issues – look on Full Node tab

Look at the date/time indicated compared to your PC.  If there is a 30 min difference and it has not caught up—on the windows menu—click View/Force reload.   It will take 5 minutes and should restart the cynic mode.  It will make you click your key code button again to get in.  It does not affect the plotting you are doing.

It’s different that bitcoin.  I am a beginner -- from what I see-- 70% of Chia work is ram from the temp storage and 30% is CPU.  The plots are very large-- think of them with millions of excel sheets and every block has a formula or "proof"  Each proof needed to be calculated and then put in the cell.  Right now running 16 gig ram-- 1 --32k plot takes 25 hrs.   Upgraded ram to 24 gigs and now plot is 10 hrs.

1/15/2021:

Your PC should have some decent ram.  I am using 24 gig.   Your temp storage should be at least 1tb.     Have a very old pc--  bought for $100.   So what I did was get a PCIE  card that accepts NVME card- Got the PCIE on ebay  $14 and  bought NVME at best buy for $120 and that is my temp storage.  I have a 2tb drive that filled quickly, now on 8tb drive that I should fill in next few days.   so that is why I say storage becomes expensive. 
 
Groups to join on chat
\# announcements   -- most important as it tells you of new versions
\# beginner  -- read this one from the start -- many questions are answered
\# farming hardware
\# general
\# plotting-hardware
\# random
\# Testnet

*\* Important*

When first plotting --   and your first plot is created and now being farmed for chia.  On the farming tab—“Time to Win a coin”— it may say 2 days.   It may take really upto 5 days to earn the first coin or partial coin.  Its normal.  Your computer is being linked up to all the other pc’s.   As you add more plots—especially after about 50 plots, then the “time to Win” gets a little closer to estimated time

Everyone is very helpful to answer questions.  The group does ask for questions to be in the selected chat room.  Beginner questions in Beginner etc

