## Estimated SSD wear out, endurance table

working model can be found here
https://drive.google.com/file/d/1mNUYRWeJUaijEZXupwP5k6IuATZGj1FB/view?usp=sharing

overview of SSD endurance testing from JEDEC industry standard here
https://www.jedec.org/sites/default/files/Alvin_Cox%20%5BCompatibility%20Mode%5D_0.pdf

| Vendor  | Model                  | Form Factor | $ASP      | User Capacity (GB): | usable GiB in OS | Spec sheet rated TBW | Num concurrent k=32 | days to wear out (drive full, worst case) | years to wear out (drive full, worst case) | days to wear out (WAF=1) | years to wear out (WAF=1) | total amount plotted before wear out worse   case (TiB) | total amount plotted before wear out best   case (TiB) | $/TiB plotted worst case (high WAF) | $/TiB plotted best case (WAF=1) |
|---------|------------------------|-------------|-----------|---------------------|------------------|----------------------|---------------------|-------------------------------------------|--------------------------------------------|--------------------------|---------------------------|---------------------------------------------------------|--------------------------------------------------------|-------------------------------------|---------------------------------|
| Micron  | 5301 Max               | SATA 2.5in  | $614      | 3840                | 3493.1           | 24528                | 11                  | 748                                       | 2.05                                       | 1706                     | 4.67                      | 1114                                                    | 2539                                                   | $0.55                               | $0.24                           |
| Micron  | 5300 Max               | SATA 2.5in  | $307      | 1920                | 1746.6           | 17520                | 5                   | 748                                       | 2.05                                       | 1706                     | 4.67                      | 557                                                     | 1270                                                   | $0.55                               | $0.24                           |
| Micron  | 5301 Pro               | SATA 2.5in  | $499      | 3840                | 3493.1           | 8410                 | 11                  | 269                                       | 0.74                                       | 1344                     | 3.68                      | 400                                                     | 2000                                                   | $1.25                               | $0.25                           |
| Micron  | 5300 Pro               | SATA 2.5in  | $250      | 1920                | 1746.6           | 5256                 | 5                   | 269                                       | 0.74                                       | 1344                     | 3.68                      | 200                                                     | 1000                                                   | $1.25                               | $0.25                           |
| Samsung | PM1725b                | U.2 & AIC   | $267      | 1600                | 1455.5           | 8760                 | 4                   | 645                                       | 1.77                                       | 1612                     | 4.42                      | 400                                                     | 1000                                                   | $0.67                               | $0.27                           |
| Intel   | P4610                  | U.2         | $300      | 1600                | 1455.5           | 10613                | 4                   | 693                                       | 1.90                                       | 1663                     | 4.56                      | 430                                                     | 1031                                                   | $0.70                               | $0.29                           |
| DRAM    | DDR3                   | DIM         | $768.00   | 512                 | 465.8            |                      | 1                   | 12596                                     | 34.51                                      | 12596                    | 34.51                     | 2500                                                    | 2500                                                   | $0.31                               | $0.31                           |
| Toshiba | PX04SVQ                | 2.5in SAS   | $380.00   | 1600                | 1455.5           | 8760                 | 4                   | 641                                       | 1.76                                       | 1732                     | 4.75                      | 398                                                     | 1074                                                   | $0.96                               | $0.35                           |
| Inland  | Inland Premium 1TB SSD | M.2         | $125.00   | 1024                | 931.5            | 1600                 | 3                   | 183                                       | 0.50                                       | 882                      | 2.42                      | 73                                                      | 350                                                    | $1.72                               | $0.36                           |
| Micron  | 9300                   | U.2         | $768      | 3840                | 3493.1           | 8400                 | 11                  | 244                                       | 0.67                                       | 1344                     | 3.68                      | 364                                                     | 2000                                                   | $2.11                               | $0.38                           |
| Samsung | PM983                  | U.2 and M.2 | $173      | 960                 | 873.3            | 1366.56              | 3                   | 171                                       | 0.47                                       | 960                      | 2.63                      | 64                                                      | 357                                                    | $2.71                               | $0.48                           |
| DRAM    | DDR4                   | DIM         | $1,536.00 | 512                 | 465.8            |                      | 1                   | 12596                                     | 34.51                                      | 12596                    | 34.51                     | 2500                                                    | 2500                                                   | $0.61                               | $0.61                           |
| Intel   | P4510                  | U.2         | $400      | 2000                | 1819.3           | 2054                 | 6                   | 148                                       | 0.40                                       | 665                      | 1.82                      | 115                                                     | 516                                                    | $3.49                               | $0.78                           |
| WD      | SN750                  | M.2         | $120.00   | 1000                | 909.7            | 600                  | 3                   | 70                                        | 0.19                                       | 387                      | 1.06                      | 27                                                      | 150                                                    | $4.40                               | $0.80                           |
| WD      | SN750                  | M.2         | $60.00    | 500                 | 454.8            | 300                  | 1                   | 70                                        | 0.19                                       | 387                      | 1.06                      | 14                                                      | 75                                                     | $4.40                               | $0.80                           |
| Seagate | Nitro 1551             | SATA 2.5in  | 255       | 960                 | 873.3            | 2390                 | 3                   | 273                                       | 0.75                                       | 820                      | 2.25                      | 102                                                     | 305                                                    | $2.51                               | $0.84                           |
| Samsung | 970 Evo                | M.2         | $164      | 1024                | 931.5            | 600                  | 3                   | 76                                        | 0.21                                       | 378                      | 1.04                      | 30                                                      | 150                                                    | $5.46                               | $1.09                           |
| Intel   | P4800X                 | U.2 & AIC   | $2,063    | 750                 | 682.3            | 41000                | 2                   | 6298                                      | 17.25                                      | 6298                     | 17.25                     | 1831                                                    | 1831                                                   | $1.13                               | $1.13                           |
| Intel   | 905p                   | U.2 and M.2 | $1,152    | 960                 | 873.3            | 17520                | 3                   | 2519                                      | 6.90                                       | 2519                     | 6.90                      | 938                                                     | 938                                                    | $1.23                               | $1.23                           |
| Samsung | 970 Pro                | M.2         | $307      | 1024                | 931.5            | 1200                 | 3                   | 126                                       | 0.35                                       | 630                      | 1.73                      | 50                                                      | 250                                                    | $6.14                               | $1.23                           |
| Intel   | 665p                   | M.2         | $102      | 1024                | 931.5            | 300                  | 3                   | 38                                        | 0.10                                       | 189                      | 0.52                      | 15                                                      | 75                                                     | $6.83                               | $1.37                           |
| Intel   | P4800X                 | U.2 & AIC   | $2,063    | 375                 | 341.1            | 41000                | 1                   | 6298                                      | 17.25                                      | 6298                     | 17.25                     | 916                                                     | 916                                                    | $2.25                               | $2.25                           |
| Intel   | 660p                   | M.2         | $123      | 1024                | 931.5            | 200                  | 3                   | 25                                        | 0.07                                       | 126                      | 0.35                      | 10                                                      | 50                                                     | $12.29                              | $2.46                           |


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