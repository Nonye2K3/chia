## Estimated SSD wear out, endurance table

working model can be found here
https://docs.google.com/spreadsheets/d/1mNUYRWeJUaijEZXupwP5k6IuATZGj1FB/edit#gid=1268465441

overview of SSD endurance testing from JEDEC industry standard here
https://www.jedec.org/sites/default/files/Alvin_Cox%20%5BCompatibility%20Mode%5D_0.pdf

| Vendor  | Model                  | Form Factor | $ASP    | User Capacity (GB): | Raw Capacity (GiB) | endurance best case (TBW)| Spec sheet rated TBW | GiB/min | days to wear out (worst case) | days to wear out (WAF=1) | plots worse case (TiB) | plots best (TiB) | $/TiB plot worst | $/TiB plotted best |
|---------|------------------------|-------------|---------|---------------------|--------------------|---------------------------------------|----------------------|---------|-------------------------------------------|--------------------------|-------------------------------------------------------|------------------------------------------------------|-------------------------------------|---------------------------------|
| Toshiba | PX04SVQ                | 2.5in SAS   | $380.00 | 1600                | 2200               | 23622                                 | 8760                 | 0.1     | 857                                       | 2315                     | 121                                                   | 326                                                  | $3.15                               | $1.17                           |
| Inland  | Inland Premium 1TB SSD | M.2         | $125.00 | 1024                | 1024               | 7697                                  | 1600                 | 0.1657  | 95                                        | 455                      | 22                                                    | 106                                                  | $5.67                               | $1.18                           |
| Micron  | 9300                   | U.2         | $768    | 3840                | 4096               | 43980                                 | 8400                 | 0.14    | 560                                       | 3078                     | 110                                                   | 606                                                  | $6.97                               | $1.27                           |
| Intel   | P4610                  | U.2         | $300    | 1600                | 2112               | 22677                                 | 10613                | 0.14    | 661                                       | 1587                     | 130                                                   | 313                                                  | $2.30                               | $0.96                           |
| Samsung | PM1725b                | U.2 & AIC   | $267    | 1600                | 2048               | 21990                                 | 8760                 | 0.14    | 616                                       | 1539                     | 121                                                   | 303                                                  | $2.20                               | $0.88                           |
| Samsung | PM983                  | U.2 and M.2 | $173    | 960                 | 1045               | 7854                                  | 1366.56              | 0.14    | 98                                        | 550                      | 19                                                    | 108                                                  | $8.94                               | $1.60                           |
| Intel   | P4510                  | U.2         | $400    | 2000                | 2112               | 11339                                 | 2054                 | 0.14    | 176                                       | 794                      | 35                                                    | 156                                                  | $11.52                              | $2.56                           |
| WD      | SN750                  | M.2         | $60.00  | 500                 | 512                | 1649                                  | 300                  | 0.1     | 29                                        | 162                      | 4                                                     | 23                                                   | $14.52                              | $2.64                           |
| WD      | SN750                  | M.2         | $120.00 | 1000                | 1024               | 3299                                  | 600                  | 0.1     | 59                                        | 323                      | 8                                                     | 45                                                   | $14.52                              | $2.64                           |
| Seagate | Nitro 1551             | 2.5in SATA  | 255     | 960                 | 1250               | 6711                                  | 2390                 | 0.1     | 219                                       | 658                      | 31                                                    | 92                                                   | $8.27                               | $2.76                           |
| Samsung | 970 Evo                | M.2         | $164    | 1024                | 1024               | 3299                                  | 600                  | 0.14    | 46                                        | 231                      | 9                                                     | 45                                                   | $18.02                              | $3.60                           |
| Intel   | P4800X                 | U.2 & AIC   | $2,063  | 750                 | 750                | 40265                                 | 41000                | 0.14    | 2818                                      | 2818                     | 555                                                   | 555                                                  | $3.72                               | $3.72                           |
| Samsung | 970 Pro                | M.2         | $307    | 1024                | 1024               | 5498                                  | 1200                 | 0.14    | 77                                        | 385                      | 15                                                    | 76                                                   | $20.28                              | $4.06                           |
| Intel   | 905p                   | U.2 and M.2 | $1,152  | 960                 | 960                | 20616                                 | 17520                | 0.14    | 1443                                      | 1443                     | 284                                                   | 284                                                  | $4.06                               | $4.06                           |
| Intel   | 665p                   | M.2         | $102    | 1024                | 1024               | 1649                                  | 300                  | 0.08    | 40                                        | 202                      | 5                                                     | 23                                                   | $22.53                              | $4.51                           |
| Intel   | 660p                   | M.2         | $123    | 1024                | 1024               | 1100                                  | 200                  | 0.08    | 27                                        | 135                      | 3                                                     | 15                                                   | $40.55                              | $8.11                           |

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