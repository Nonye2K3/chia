# Various k size statistics - Current - Beta 8 and newer

These results from various machines should give a sense of how long and how much space a plot will take on different hardware. The first section is from Beta 1.8 or newer. Historical data is below and is currently still useful at the Beta 1.8 stage. Please add yours here or post the details in #testnet of the [Keybase Chat](https://keybase.io/team/chia_network.public)

## MacBook Pro (13-inch, 2019, Four Thunderbolt 3 ports)
* 2.8 GHz Quad-Core Intel Core i7
* 16 GB 2133 MHz LPDDR3
* APPLE SSD AP1024M
* ~ 2900 MB/s write

|  k  | Phase 1 (min) | Plot time (min) | Plot size (GB)  | GB/minute | working (GB) | CPU Utilization  | -b setting | Version | Note |
| --- |      ---      |        ---      |     ---         |    ---    |    ---       |      ---         |     ---    |   ---   | ---  |
| 30  |    149.91     |      327.83     |    23.79        |   0.0726  |   125.93     |     83.74%.      |   default  |   1.8   |      |

## Ubuntu 19.04
* Intel(R) Xeon(R) W-2155 CPU @ 3.30GHz (10 cores)
* 64 GB PC4-21300 DDR4-2666V-R REGISTERED ECC
* Western Digital 4 TB [WD10EZEX](https://documents.westerndigital.com/content/dam/doc-library/en_us/assets/public/western-digital/product/internal-drives/wd-blue-hdd/data-sheet-wd-blue-pc-hard-drives-2879-771436.pdf)
* ~ - MB/s write

|  k  | Phase 1 (min) | Plot time (min) | Plot size (GB)  | GB/minute | working (GB) | CPU Utilization  | -b setting | Version | Note |
| --- |      ---      |        ---      |     ---         |    ---    |    ---       |      ---         |     ---    |   ---   | ---  |
| 30  |    131.25     |       302.8     |    23.82        |   0.0787  |   126.02     |     79.86%.      |   default  |   1.8   |      |
| 31  |    333.50     |       724.3     |    49.17        |   0.0679  |   262.04     |     70.66%.      |   default  |   1.8   |      |
| 32  |    674.69     |      1577.7     |    101.3        |   0.0642  |   523.93     |     63.74%.      |   default  |   1.8   |      |

## Razer Blade Stealth 2018
* Intel(R) Core(TM) i7-8550U CPU @ 1.80GHz, 2001 Mhz, 4 Core(s), 8 Logical Processor(s)
* 16 GB RAM
* Samsung SSD PM981 MZVLB512HAJQ
* Crystal - CDM 5 Write Seq: 1468 MB/s

|  k  | Phase 1 (min) | Plot time (min) | Plot size (GB)  | GB/minute | working (GB) | CPU Utilization  | -b setting | Version | Note |
| --- |      ---      |        ---      |     ---         |    ---    |    ---       |      ---         |     ---    |   ---   | ---  |
| 31  |     278.5     |      613.09     |    49.134       |   0.0801  |   261.944    |     100.0%       |   default  |   1.8   |      |


## Intel i7 4770k
* Intel i7 4770k (2013, 7 year old CPU)
* 32 GB RAM
* NVME SSD (Microcenter brand, raid0 over 2 x 512 GB)
* Iozone 490 (256 kiB blocks, fsynced to storage): 876 MiB/s seq write, 3464 MiB/s seq reads

|  k  | Phase 1 (min) | Plot time (min) | Plot size (GB)  | GB/minute | working (GB) | CPU Utilization  | -b setting | Version | Note |
| --- |      ---      |        ---      |     ---         |    ---    |    ---       |      ---         |     ---    |   ---   | ---  |
| 32  |     429.9     |      935.87     |    101.34       |   0.1083  |   523.96*    |     94.36%.      |   default  |   1.8   | *Actual working space 10GiB higher     |

## AWS EC2 r5dn.12xlarge
* Processor: 48 Intel(R) Xeon(R) Platinum 8259CL CPU @ 2.50GHz
* Memory: 374GB
* Storage: tempdir: ramdisk of 264GB (Ubuntu 18.04) finaldir: Amazon.com, Inc. NVMe SSD
* ~ - MB/s write

|  k  | Phase 1 (min) | Plot time (min) | Plot size (GB)  | GB/minute | working (GB) | CPU Utilization  | -b setting | Version | Note |
| --- |      ---      |        ---      |     ---         |    ---    |    ---       |      ---         |     ---    |   ---   | ---  |
| 31  |    185.59     |      361.67     |    49.17        |   0.13595 |   262.04     |     99.99%.      |     64GB   |   1.8   |      |


# Historical k sizes - Pre Beta 8

## MacBook Pro (13-inch, 2019, Four Thunderbolt 3 ports)
* 2.8 GHz Quad-Core Intel Core i7
* 16 GB 2133 MHz LPDDR3
* APPLE SSD AP1024M
* ~ 2900 MB/s write

| k  |  plot time (minutes) | plot size (GB)  | working (GB)  | CPU Utilization  | Note |
|---|---|---|---|---|---|
| 26  |  11.6 | 1.296  | 7.06  | 91.86%  |   |
| 27  |  30.1 | 2.697  | 14.51  | 83.04%  |   |
| 28  |  82.1 | 5.58  | 30.27  | 69.4%  |  some use |
| 29  |  179 | 11.5  | 60.9  | 69.68%  |   |
| 30  | 406.5  | 23.8  | 128  | 71.67%  | in use  |
| 30  | 377  | 23.8  | 128  | 73.18%  | machine idle  |
| 31  | 856.7  | 49.16  | 262.0  | 92.29%  | idle - last 80%  |

## iMac (Retina 4K, 21.5-inch, 2017)
* 3.4 GHz Quad-Core Intel Core i5
* 16 GB 2400 MHz DDR4
* 1TB Fusion Drive (Actually attached USB 3.0 4Tb RAID 1)
* ~ - MB/s write

| k  |  plot time (minutes) | plot size (GB)  | working (GB)  | CPU Utilization  |
|---|---|---|---|---|
| 25  | 4.1 | 0.62 | 3.4 | 97.85 |
| 26  |  27 | 1.3  | -  | -  |
| 27  | 81  | 2.7  | -  | -  |
| 28  | 260  | 5.6  | 30.3  | -  |
| 29  | 787  | 11.5  | 61  | -  |

## Ubuntu 19.04
* Intel(R) Xeon(R) W-2155 CPU @ 3.30GHz (10 cores)
* 64 GB PC4-21300 DDR4-2666V-R REGISTERED ECC
* Western Digital 4 TB [WD10EZEX](https://documents.westerndigital.com/content/dam/doc-library/en_us/assets/public/western-digital/product/internal-drives/wd-blue-hdd/data-sheet-wd-blue-pc-hard-drives-2879-771436.pdf)
* ~ - MB/s write

| k  |  plot time (minutes) | plot size (GB)  | working (GB)  | CPU Utilization  | Note |
|---|---|---|---|---|---|
| 28  |  55.3 | 5.58  | 30.3  | 99.56%  | |
| 28  |  43 | 5.56  | 30.3  | 99.07%  | beta4 |
| 30  | 308  | 23.8  | 128  | 83.78%  | |
| 30  | 273  | 23.8  | 128  | 75.44%  | beta4 |
| 31  | 805  | 49.1  | 262  | 67.36%  | |
| 31  | 621  | 49.1  | 262  | 69.68%  | beta4 |
| 32  | 1,866  | 101.4  | 544  | 60.57%  | |
| 33  | 4663.5  | 208.8  | 1095.97  | 52.2%  | |
| 34  | 10,865.6  | 429.8  | 2,287  | 50.13%  | |
| 35  | 25,703  | 884.1  | 4672.2  | 39.45%  | |

## AWS EC2 r5.12xlarge
* Processor: 48 Intel(R) Xeon(R) Platinum 8175M CPU @ 2.50GHz
* Memory: 374GB
* Storage: Memory tempfs (Ubuntu 19.10)
* ~ - MB/s write

| k  |  plot time (minutes) | plot size (GB)  | working (GB)  | CPU Utilization  | Note |
|---|---|---|---|---|---|
| 31  |  425 | 49.15  | 262  | 99.99%  | alpha 1.3 |    |

## AWS EC2 r5dn.12xlarge
* Processor: 48 Intel(R) Xeon(R) Platinum 8259CL CPU @ 2.50GHz
* Memory: 374GB
* Storage: tempdir: ramdisk of 264GB (Ubuntu 18.04) finaldir: Amazon.com, Inc. NVMe SSD
* ~ - MB/s write

| k  |  plot time (minutes) | plot size (GB)  | working (GB)  |  GB/minute |  CPU Utilization | Note         |
| ---|  ---                 |       ---       |    ---        |     ---    |  ---             |    ---       |
| 26 |    9                 |     1.3         |      7.1      |   0.144    |    99.99%        | pre-beta-1.5 |
| 27 |    20                |     2.7         |      14.5     |   0.135    |    99.99%        | pre-beta-1.5 |
| 28 |    37.5              |     5.57        |      30.2     |   0.149    |    99.99%        | pre-beta-1.5 |
| 30 |    200               |     23.8        |      128.1    |   0.119    |    99.99%        | pre-beta-1.5 |
| 31 |    412               |     49.14       |      262*     |   0.119    |    99.99%        | actual working 267GB, chiapos 0.12.13 |
| 31 |    408.4             |     49.14       |      262*     |   0.120    |    99.99%        | actual working 267GB, [speedy branch](https://github.com/Chia-Network/chiapos/commit/9200540e7e0a0e7cdbf307ef84a4f8e19cf33403) |

## MacBook Pro (15-inch, 2017)
* 2,8 GHz Quad-Core Intel Core i7
* 16 GB 2133 MHz LPDDR3
* 251 GB (Flash Storage) Device Name: APPLE SSD SM0256L
* ~ - MB/s write

| k  |  plot time (minutes) | plot size (GB)  | working (GB)  | CPU Utilization  | Note |
|---|---|---|---|---|---|
| 30  |  559 | 23.79  | 217.94  | 61.65%  | alpha 1.0 |
| 30  | 614  | 23.8  | 127.992  | 61.13%  | alpha 1.1 |

## MacBook Pro (15-inch, 2017)
* Processor: 2,8 GHz Quad-Core Intel Core i7
* Memory: 16 GB 2133 MHz LPDDR3
* Storage: 1 TB (TOSHIBA HD)
* ~ - MB/s write

| k  |  plot time (minutes) | plot size (GB)  | working (GB)  | CPU Utilization  | Note |
|---|---|---|---|---|---|
| 30  |  1512 | 23.838  | 128.05  | 27.59%  | alpha 1.0 |    |

## Fedora Server 31
* Processor: AMD Ryzen 5 3600 6-Core Processor            
* Memory: G-Skill 64 GB DDR4 3000MHz            
* Storage: GIGABYTE AORUS NVMe Gen4 SSD 1 TB               

| k  |  plot time (minutes) | plot size (GB)  | working (GB)  | CPU Utilization  | Note |
|---|---|---|---|---|---|
| 31  |  571.5 | 49.15  | 261.99  | 71.17%  | beta 3.0 3 concurrent |    |
| 32  |  957 | 101.36  | 544.0  | 97.49%  | beta 3.0 |    |

## Fedora Server 31
* Processor: AMD Ryzen Threadripper 2950X           
* Memory: Corsair 128 GB DDR4 2133MHz           
* Storage: LVM-RAID0 (3 x Inland Performance Gen4 SSD 1 TB)                 

| k  |  plot time (minutes) | plot size (GB)  | working (GB)  | CPU Utilization  | Note |
|---|---|---|---|---|---|
| 33  |  3093 | 208.85  | 1096.03  | 79.63%  | beta 3.0 2 concurrent |    |
