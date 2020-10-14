## Estimated SSD wear out, endurance table

working model can be found here
https://drive.google.com/file/d/1mNUYRWeJUaijEZXupwP5k6IuATZGj1FB/view?usp=sharing

overview of SSD endurance testing from JEDEC industry standard here
https://www.jedec.org/sites/default/files/Alvin_Cox%20%5BCompatibility%20Mode%5D_0.pdf

| Vendor  | Model                  | Form Factor | $ASP      | User Capacity (GB): | estimated NAND endurance (TBW, WAF=1) | Spec sheet rated TBW |  | GiB/min | Num concurrent k=32 | User write bandwidth (MB/s) | days to wear out (drive full, worst case) | days to wear out (WAF=1) | total amount plotted before wear out best case (TiB) | $/TiB plotted best case (WAF=1) |
|---------|------------------------|-------------|-----------|---------------------|---------------------------------------|----------------------|--|---------|---------------------|-----------------------------|-------------------------------------------|--------------------------|------------------------------------------------------|---------------------------------|
| Samsung | PM1725b                | U.2 & AIC   | $267      | 1600                | 21990                                 | 8760                 |  | 0.1     | 4                   | 137                         | 743                                       | 1856                     | 261                                                  | $1.02                           |
| Intel   | P4610                  | U.2         | $300      | 1600                | 22677                                 | 10613                |  | 0.1     | 4                   | 137                         | 798                                       | 1914                     | 269                                                  | $1.11                           |
| DRAM    | DDR3                   | DIM         | $768.00   | 512                 | 54976                                 |                      |  | 0.1     | 1                   | 44                          | 14503                                     | 14503                    | 2039                                                 | $0.38                           |
| Toshiba | PX04SVQ                | 2.5in SAS   | $380.00   | 1600                | 23622                                 | 8760                 |  | 0.1     | 4                   | 137                         | 739                                       | 1994                     | 280                                                  | $1.36                           |
| Inland  | Inland Premium 1TB SSD | M.2         | $125.00   | 1024                | 7697                                  | 1600                 |  | 0.1     | 2                   | 88                          | 211                                       | 1015                     | 143                                                  | $0.88                           |
| Micron  | 9300                   | U.2         | $768      | 3840                | 43980                                 | 8400                 |  | 0.1     | 9                   | 329                         | 281                                       | 1547                     | 218                                                  | $3.53                           |
| Samsung | PM983                  | U.2 and M.2 | $173      | 960                 | 7854                                  | 1366.56              |  | 0.1     | 2                   | 82                          | 197                                       | 1105                     | 155                                                  | $1.11                           |
| DRAM    | DDR4                   | DIM         | $1,536.00 | 512                 | 54976                                 |                      |  | 0.1     | 1                   | 44                          | 14503                                     | 14503                    | 2039                                                 | $0.75                           |
| Intel   | P4510                  | U.2         | $400      | 2000                | 11339                                 | 2054                 |  | 0.1     | 5                   | 171                         | 170                                       | 766                      | 108                                                  | $3.71                           |
| WD      | SN750                  | M.2         | $60.00    | 500                 | 1649                                  | 300                  |  | 0.1     | 1                   | 43                          | 81                                        | 446                      | 63                                                   | $0.96                           |
| WD      | SN750                  | M.2         | $120.00   | 1000                | 3299                                  | 600                  |  | 0.1     | 2                   | 86                          | 81                                        | 446                      | 63                                                   | $1.92                           |
| Seagate | Nitro 1551             | 2.5in SATA  | 255       | 960                 | 6711                                  | 2390                 |  | 0.1     | 2                   | 82                          | 315                                       | 944                      | 133                                                  | $1.92                           |
| Samsung | 970 Evo                | M.2         | $164      | 1024                | 3299                                  | 600                  |  | 0.1     | 2                   | 88                          | 87                                        | 435                      | 61                                                   | $2.68                           |
| Intel   | P4800X                 | U.2 & AIC   | $2,063    | 750                 | 40265                                 | 41000                |  | 0.1     | 2                   | 64                          | 7251                                      | 7251                     | 1020                                                 | $2.02                           |
| Samsung | 970 Pro                | M.2         | $307      | 1024                | 5498                                  | 1200                 |  | 0.1     | 2                   | 88                          | 145                                       | 725                      | 102                                                  | $3.01                           |
| Intel   | 905p                   | U.2 and M.2 | $1,152    | 960                 | 20616                                 | 17520                |  | 0.1     | 2                   | 82                          | 2901                                      | 2901                     | 408                                                  | $2.82                           |
| Intel   | 665p                   | M.2         | $102      | 1024                | 1649                                  | 300                  |  | 0.1     | 2                   | 88                          | 44                                        | 218                      | 31                                                   | $3.35                           |
| Intel   | P4800X                 | U.2 & AIC   | $2,063    | 375                 | 20133                                 | 41000                |  | 0.1     | 1                   | 32                          | 7251                                      | 7251                     | 1020                                                 | $2.02                           |
| Intel   | 660p                   | M.2         | $123      | 1024                | 1100                                  | 200                  |  | 0.1     | 2                   | 88                          | 29                                        | 145                      | 20                                                   | $6.03                           |

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

## Adding new models
Please add your model string below if you want me to put it into my calculator and add to the list!