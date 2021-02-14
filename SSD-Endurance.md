## Estimated SSD wear out, endurance table

There are various approaches to picking a great plotting SSD, and a lot will depend on the physical system it is going into for form factor and interface compatibility (NVMe/PCIe, SATA, or SAS). The one thing in common will be that you need high endurance, due to the fact that it take almost ~1.8TB of writes using the `-e` flag or 1.6TB without that option to create a single K=32 plot.

Endurance is how much data can be written to the SSD before it wears out. In Chia this is important because a plotting SSD will generally be at 100% duty cycle and writing all day. 

A mixed use or high endurance data center or enterprise SSD is the best choice for plotting. Used SSDs with plenty of endurance can be found for a good value on eBay, Craigslist, or similar.

Consumer NVMe SSDs are generally not recommended due to the lower endurance, and they often employ caching algorithms to faster media (SLC, or single level cell) for great bursty performance. They do not perform well under heavy workload sustained IO.
There are very high performance consumer NVMe SSDs that will offer great plotting performance, but the lower rated endurance in TBW will result in a faster wearout.

working model can be found [here](https://drive.google.com/file/d/1mNUYRWeJUaijEZXupwP5k6IuATZGj1FB/view?usp=sharing)

| Vendor  | Model                  | Form Factor | Interface  | Class                | $ASP      | $/GB  | User Capacity (GB): | usable GiB in OS | Spec sheet rated TBW | total plots, worst, (TiB) | total plots, best, (TiB) | $/TiB, worst | $/TiB, best |
|---------|------------------------|-------------|------------|----------------------|-----------|-------|---------------------|------------------|----------------------|-------------------------------------------------------|------------------------------------------------------|-------------------------------------|---------------------------------|
| Intel   | P3700                  | U.2 & AIC   | NVMe       | enterprise mixed use | $250.00   | $0.16 | 1600                | 1455.5           | 43800                | 1953                                                  | 2930                                                 | $0.13                               | $0.09                           |
| Intel   | P3600                  | U.2 & AIC   | NVMe       | enterprise mixed use | $140.00   | $0.09 | 1600                | 1455.5           | 8760                 | 391                                                   | 977                                                  | $0.36                               | $0.14                           |
| Intel   | P4600                  | U.2 & AIC   | NVMe       | enterprise mixed use | $352.00   | $0.11 | 3200                | 2910.9           | 18200                | 1356                                                  | 2441                                                 | $0.26                               | $0.14                           |
| Intel   | P3600                  | U.2 & AIC   | NVMe       | enterprise mixed use | $116.00   | $0.10 | 1200                | 1091.6           | 6570                 | 297                                                   | 742                                                  | $0.39                               | $0.16                           |
| Intel   | P3600                  | U.2 & AIC   | NVMe       | enterprise mixed use | $115.00   | $0.10 | 1200                | 1091.6           | 6570                 | 293                                                   | 732                                                  | $0.39                               | $0.16                           |
| Intel   | P4600                  | U.2 & AIC   | NVMe       | enterprise mixed use | $176.00   | $0.11 | 1600                | 1455.5           | 8990                 | 407                                                   | 732                                                  | $0.43                               | $0.24                           |
| Intel   | P4600                  | U.2 & AIC   | NVMe       | enterprise mixed use | $220.00   | $0.11 | 2000                | 1819.3           | 11080                | 509                                                   | 916                                                  | $0.43                               | $0.24                           |
| Intel   | P3700                  | U.2 & AIC   | NVMe       | enterprise mixed use | $120.00   | $0.30 | 400                 | 363.9            | 7300                 | 332                                                   | 498                                                  | $0.36                               | $0.24                           |
| Micron  | 5300 Max               | SATA 2.5in  | SATA       | enterprise SATA      | $307.20   | $0.16 | 1920                | 1746.6           | 17520                | 557                                                   | 1270                                                 | $0.55                               | $0.24                           |
| Micron  | 5300 Max               | SATA 2.5in  | SATA       | enterprise SATA      | $614.40   | $0.16 | 3840                | 3493.1           | 24528                | 1114                                                  | 2539                                                 | $0.55                               | $0.24                           |
| Intel   | S4610                  | SATA 2.5in  | SATA       | enterprise mixed use | $144.00   | $0.15 | 960                 | 873.3            | 5800                 | 266                                                   | 586                                                  | $0.54                               | $0.25                           |
| Micron  | 5300 Pro               | SATA 2.5in  | SATA       | enterprise SATA      | $249.60   | $0.13 | 1920                | 1746.6           | 5256                 | 200                                                   | 1000                                                 | $1.25                               | $0.25                           |
| Micron  | 5300 Pro               | SATA 2.5in  | SATA       | enterprise SATA      | $499.20   | $0.13 | 3840                | 3493.1           | 8410                 | 400                                                   | 2000                                                 | $1.25                               | $0.25                           |
| Samsung | PM1725b                | U.2 & AIC   | NVMe       | enterprise mixed use | $267.00   | $0.17 | 1600                | 1455.5           | 8760                 | 400                                                   | 1000                                                 | $0.67                               | $0.27                           |
| Micron  | 9300 Pro               | U.2         | NVMe       | enterprise           | $1,152.00 | $0.15 | 7680                | 6986.3           | 16800                | 727                                                   | 4000                                                 | $1.58                               | $0.29                           |
| Micron  | 9300 Pro               | U.2         | NVMe       | enterprise           | $576.00   | $0.15 | 3840                | 3493.1           | 8400                 | 364                                                   | 2000                                                 | $1.58                               | $0.29                           |
| Intel   | P4610                  | U.2         | NVMe       | enterprise mixed use | $300.00   | $0.19 | 1600                | 1455.5           | 10613                | 430                                                   | 1031                                                 | $0.70                               | $0.29                           |
| DRAM    | DDR3                   | DIM         | DDR3       | memory               | $768.00   | $1.50 | 512                 | 465.8            |                      | 2500                                                  | 2500                                                 | $0.31                               | $0.31                           |
| Samsung | PM983                  | U.2 and M.2 | NVMe       | data center          | $110.00   | $0.11 | 960                 | 873.3            | 1366.56              | 63                                                    | 350                                                  | $1.76                               | $0.31                           |
| Samsung | PM983                  | U.2 and M.2 | NVMe       | data center          | $225.00   | $0.12 | 1920                | 1746.6           | 2733.12              | 125                                                   | 700                                                  | $1.80                               | $0.32                           |
| Intel   | S3710                  | SATA 2.5in  | SATA       | enterprise mixed use | $68.00    | $0.17 | 400                 | 363.9            | 8300                 | 137                                                   | 205                                                  | $0.50                               | $0.33                           |
| Corsair | CSSD-F960GBMP510       | M.2         | NVMe       | client               | $120.00   | $0.13 | 960                 | 873.3            | 1700                 | 76                                                    | 350                                                  | $1.57                               | $0.34                           |
| Toshiba | PX04SVQ                | 2.5in SAS   | SAS 12Gbps | enterprise SAS       | $380.00   | $0.24 | 1600                | 1455.5           | 8760                 | 398                                                   | 1074                                                 | $0.96                               | $0.35                           |
| Inland  | Inland Premium 1TB SSD | M.2         | NVMe       | client mainstream    | $125.00   | $0.12 | 1024                | 931.5            | 1600                 | 73                                                    | 350                                                  | $1.72                               | $0.36                           |
| Micron  | 9300                   | U.2         | NVMe       | data center          | $768.00   | $0.20 | 3840                | 3493.1           | 8400                 | 364                                                   | 2000                                                 | $2.11                               | $0.38                           |
| Micron  | 9300 Max               | U.2         | NVMe       | enterprise           | $1,600.00 | $0.25 | 6400                | 5821.9           | 37300                | 1650                                                  | 4125                                                 | $0.97                               | $0.39                           |
| Micron  | 9300 Max               | U.2         | NVMe       | enterprise           | $800.00   | $0.25 | 3200                | 2910.9           | 18600                | 825                                                   | 2063                                                 | $0.97                               | $0.39                           |
| Sabrent | SB-ROCKET-NVMe4-2TB    | M.2         | NVMe       | client               | $349.00   | $0.17 | 2000                | 1819.3           | 3600                 | 163                                                   | 700                                                  | $2.14                               | $0.50                           |
| Intel   | S4600                  | SATA 2.5in  | SATA       | enterprise mixed use | $96.00    | $0.20 | 480                 | 436.6            | 2950                 | 104                                                   | 188                                                  | $0.92                               | $0.51                           |
| DRAM    | DDR4                   | DIM         | DDR4       | memory               | $1,536.00 | $3.00 | 512                 | 465.8            |                      | 2500                                                  | 2500                                                 | $0.61                               | $0.61                           |
| Intel   | P4510                  | U.2         | NVMe       | data center          | $400.00   | $0.20 | 2000                | 1819.3           | 2054                 | 115                                                   | 516                                                  | $3.49                               | $0.78                           |
| WD      | SN750                  | M.2         | NVMe       | client mainstream    | $60.00    | $0.12 | 500                 | 454.8            | 300                  | 14                                                    | 75                                                   | $4.40                               | $0.80                           |
| WD      | SN750                  | M.2         | NVMe       | client mainstream    | $120.00   | $0.12 | 1000                | 909.7            | 600                  | 27                                                    | 150                                                  | $4.40                               | $0.80                           |
| Corsair | MP510                  | M.2         | NVMe       | client               | $120.00   | $0.13 | 960                 | 873.3            | 720                  | 33                                                    | 150                                                  | $3.66                               | $0.80                           |
| Seagate | Nitro 1551             | SATA 2.5in  | SATA       | data center SATA     | $255.00   | $0.27 | 960                 | 873.3            | 2390                 | 102                                                   | 305                                                  | $2.51                               | $0.84                           |
| Samsung | 970 Evo                | M.2         | NVMe       | client maintream     | $163.84   | $0.16 | 1024                | 931.5            | 600                  | 30                                                    | 150                                                  | $5.46                               | $1.09                           |
| Intel   | P4800X                 | U.2 & AIC   | NVMe       | enterprise           | $2,062.50 | $2.75 | 750                 | 682.3            | 41000                | 1831                                                  | 1831                                                 | $1.13                               | $1.13                           |
| Samsung | 970 Pro                | M.2         | NVMe       | client high end      | $307.20   | $0.30 | 1024                | 931.5            | 1200                 | 50                                                    | 250                                                  | $6.14                               | $1.23                           |
| Intel   | 905p                   | U.2 and M.2 | NVMe       | clinet high end      | $1,152.00 | $1.20 | 960                 | 873.3            | 17520                | 938                                                   | 938                                                  | $1.23                               | $1.23                           |
| Intel   | 665p                   | M.2         | NVMe       | client mainstream    | $102.40   | $0.10 | 1024                | 931.5            | 300                  | 15                                                    | 75                                                   | $6.83                               | $1.37                           |
| Intel   | P4800X                 | U.2 & AIC   | NVMe       | enterprise           | $2,062.50 | $5.50 | 375                 | 341.1            | 41000                | 916                                                   | 916                                                  | $2.25                               | $2.25                           |
| Intel   | 660p                   | M.2         | NVMe       | client mainstream    | $122.88   | $0.12 | 1024                | 931.5            | 200                  | 10                                                    | 50                                                   | $12.29                              | $2.46                           |

estimated GiB per minute based off class of drives for drives that have yet to be tested, for the drives in the wiki I have added the measured numbers.

## Math
* NAND P/E Cycles = amount of program / erase cycles NAND can do before wearing out. NAND programs (writes) in pages and erases in blocks (contains many pages)
* Wearing out - SSD no longer meeting UBER (uncorrectable bit error rate),  retention (keeping data safe while powered off), failure rate, or user capacity
* UBER = number of data errors / number of bits read
* WAF (Write Amplification Factor) = NAND writes / host writes
* TBW or PBW – amount of host writes to SSD before wearing out
* TBW = drive capacity * cycles / WAF
* DWPD (drive writes per day): amount of data you can write to device each day of the warranty (typically 5 years) without wearing out
* DWPD = TBW/365/5/drive capacity

## Monitor Endurance in Linux


### NVMe
https://github.com/linux-nvme/nvme-cli

https://nvmexpress.org/open-source-nvme-management-utility-nvme-command-line-interface-nvme-cli/

Reading endurance with NVMe-CLI - this is the gas gauge that shows total endurance used 

`sudo nvme smart-log /dev/nvme0 | grep percentage_used`
	
Reading amount of writes that the drive have actually done

`sudo nvme smart-log /dev/nvme0 | grep data_units_written`
	
Bytes written = output * 1000 * 512B

TBW = output * 1000 * 512B / (1000^4) or (1024^4)

To find out NAND writes, you will have use the vendor plugins for NVMe-CLI.

`sudo nvme <vendor name> help`

Example with an Intel SSD

`sudo nvme intel smart-log-add /dev/nvme0`


### SATA
In SATA you can use the following commands

`sudo apt install smartmontools`

`sudo smartctl -x /dev/sda | grep Logical`

`sudo smartctl -a /dev/sda`

looking for Media_Wearout_Indicator

note this does also work for NVMe for basic SMART health info

`sudo smartctl -a /dev/nvme0`

### SAS
`sg_logs /dev/sg1 --page=0x11`

look for

```Percentage used endurance indicator: 0%```


overview of SSD endurance testing from JEDEC industry standard here
https://www.jedec.org/sites/default/files/Alvin_Cox%20%5BCompatibility%20Mode%5D_0.pdf

## Adding new models
Please add your model string below if you want me to put it into my calculator and add to the list!

