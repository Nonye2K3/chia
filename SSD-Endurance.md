## Estimated SSD wear out, endurance table

working model can be found here
https://docs.google.com/spreadsheets/d/1mNUYRWeJUaijEZXupwP5k6IuATZGj1FB/edit#gid=1268465441

| Vendor  | Model                  | $ASP   | User Capacity (GB): | estimated NAND endurance (TBW, WAF=1) | Spec sheet rated TBW | DWPD over 5 years (calculated) | GiB/min | days to wearout | days to wear out (WAF=1) | amount plotted (TiB) | amount plotted (TiB, WAF=1) | $/TiB plotted worst case (high WAF) | $/TiB plotted best case (WAF=1) |
|---------|------------------------|--------|---------------------|---------------------------------------|----------------------|--------------------------------|---------|-----------------|--------------------------|----------------------|-----------------------------|-------------------------------------|---------------------------------|
| Intel   | P4610                  | $400   | 1600                | 22677                                 | 10613                | 3.24                           | 0.2     | 463             | 1111                     | 130                  | 313                         | $3.07                               | $1.28                           |
| Intel   | P4510                  | $400   | 2000                | 11339                                 | 2054                 | 0.69                           | 0.2     | 123             | 556                      | 35                   | 156                         | $11.52                              | $2.56                           |
| Samsung | PM983                  | $173   | 960                 | 7854                                  | 1366.56              | 0.80                           | 0.2     | 69              | 385                      | 19                   | 108                         | $8.94                               | $1.60                           |
| Samsung | 970 Evo                | $164   | 1024                | 3299                                  | 600                  | 0.35                           | 0.2     | 32              | 162                      | 9                    | 45                          | $18.02                              | $3.60                           |
| Samsung | 970 Pro                | $307   | 1024                | 5498                                  | 1200                 | 0.59                           | 0.2     | 54              | 269                      | 15                   | 76                          | $20.28                              | $4.06                           |
| Intel   | 660p                   | $123   | 1024                | 1100                                  | 200                  | 0.12                           | 0.1     | 22              | 108                      | 3                    | 15                          | $40.55                              | $8.11                           |
| Intel   | 665p                   | $102   | 1024                | 1649                                  | 300                  | 0.18                           | 0.1     | 32              | 162                      | 5                    | 23                          | $22.53                              | $4.51                           |
| Samsung | PM1725b                | $400   | 1600                | 21990                                 | 8760                 | 3.01                           | 0.2     | 431             | 1077                     | 121                  | 303                         | $3.30                               | $1.32                           |
| Micron  | 9300                   | $768   | 3840                | 43980                                 | 8400                 | 1.14                           | 0.2     | 392             | 2155                     | 110                  | 606                         | $6.97                               | $1.27                           |
| Intel   | P4800X                 | $2,063 | 750                 | 40265                                 | 41000                | 29.42                          | 0.139   | 2839            | 2839                     | 555                  | 555                         | $3.72                               | $3.72                           |
| Intel   | 905p                   | $1,152 | 960                 | 20616                                 | 17520                | 11.77                          | 0.139   | 1453            | 1453                     | 284                  | 284                         | $4.06                               | $4.06                           |
| Inland  | Inland Premium 1TB SSD | 125    | 1024                | 7697                                  | 1600                 | 0.86                           | 0.1657  | 95              | 455                      | 22                   | 106                         | $5.67                               | $1.18                           |

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


## Adding new models
Please add your model string below if you want me to put it into my calculator and add to the list!