## Estimated SSD wear out, endurance table

| Vendor  | Model                  | $ASP   | User Capacity (GB): | raw NAND endurance (WAF=1) | rated TBW | DWPD over 5 years (calculated) | GiB/min | days to wear out (worst case) | days to wear out (WAF=1) | TiB plotted (worst case) | TiB plotted (TiB, WAF=1) | $/TiB plotted worst case | $/TiB plotted (WAF=1) |
|---------|------------------------|--------|---------------------|----------------------------|-----------|--------------------------------|---------|-------------------------------|--------------------------|--------------------------|--------------------------|--------------------------|-----------------------|
| Intel   | P4610                  | $400   | 1600                | 22677                      | 10613     | 3.24                           | 0.2     | 5093                          | 12222                    | 1432                     | 3438                     | $0.28                    | $0.12                 |
| Intel   | P4510                  | $400   | 2000                | 11339                      | 2054      | 0.69                           | 0.2     | 1358                          | 6111                     | 382                      | 1719                     | $1.05                    | $0.23                 |
| Samsung | PM983                  | $173   | 960                 | 7854                       | 1366.56   | 0.80                           | 0.2     | 756                           | 4233                     | 213                      | 1191                     | $0.81                    | $0.15                 |
| Samsung | 970 Evo                | $164   | 1024                | 3299                       | 600       | 0.35                           | 0.2     | 356                           | 1778                     | 100                      | 500                      | $1.64                    | $0.33                 |
| Samsung | 970 Pro                | $307   | 1024                | 5498                       | 1200      | 0.59                           | 0.2     | 593                           | 2963                     | 167                      | 833                      | $1.84                    | $0.37                 |
| Intel   | 660p                   | $123   | 1024                | 1100                       | 200       | 0.12                           | 0.1     | 237                           | 1185                     | 33                       | 167                      | $3.69                    | $0.74                 |
| Intel   | 665p                   | $102   | 1024                | 1649                       | 300       | 0.18                           | 0.1     | 356                           | 1778                     | 50                       | 250                      | $2.05                    | $0.41                 |
| Samsung | PM1725b                | $400   | 1600                | 21990                      | 8760      | 3.01                           | 0.2     | 4741                          | 11852                    | 1333                     | 3333                     | $0.30                    | $0.12                 |
| Micron  | 9300                   | $768   | 3840                | 43980                      | 8400      | 1.14                           | 0.2     | 4310                          | 23704                    | 1212                     | 6667                     | $0.63                    | $0.12                 |
| Intel   | P4800X                 | $2,063 | 750                 | 40265                      | 41000     | 29.42                          | 0.139   | 31225                         | 31225                    | 6104                     | 6104                     | $0.34                    | $0.34                 |
| Intel   | 905p                   | $1,152 | 960                 | 20616                      | 17520     | 11.77                          | 0.139   | 15987                         | 15987                    | 3125                     | 3125                     | $0.37                    | $0.37                 |
| Inland  | Inland Premium 1TB SSD | 125    | 1024                | 7697                       | 1600      | 0.86                           | 0.1657  | 1041                          | 5007                     | 243                      | 1167                     | $0.52                    | $0.11                 |

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

Reading endurance with NVMe-CLI - this is the gas gauge that shows total endurance used 

`sudo nvme smart-log /dev/nvme0 | grep percentage_used`
	
Reading amount of writes that the drive have actually done

`sudo nvme smart-log /dev/nvme0 | grep data_units_written`
	
Bytes written = output * 1000 * 512B
TBW = output * 1000 * 512B / (1000^4) or (1024^4)


## Adding new models
Please add your model string below if you want me to put it into my calculator and add to the list!