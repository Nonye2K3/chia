# Various k size statistics - Current - Beta 8 and newer

These results from various machines should give a sense of how long and how much space a plot will take on different hardware. The first section is from Beta 1.8 or newer. Historical data is below and is currently still useful at the Beta 1.8 stage. Please add yours here or post the details in the #testnet channel of the [Keybase Chat](https://keybase.io/team/chia_network.public). The theory and process of plotting are described in the [Chia Proof of Space Construction](https://www.chia.net/assets/proof_of_space.pdf) document.

estimated space = 0.762 * k * 2^k bytes
| k | space GiB | temp GiB | space GB | temp GB |
| ---|---|---|---|---|
| 28 |5.33 | 32.00 | 5.73 | 34.36 |
| 29 |11.05 | 66.29 | 11.86 | 71.18 |
| 30 |22.86 | 137.16 | 24.55 | 147.27 |
| 31 |47.24 | 283.46 | 50.73 | 304.37 |
| 32 |97.54 | 585.22 | 104.73 | 628.37 |
| 33 |201.17 | 1207.01 | 216.00 | 1296.01 |
| 34 |414.53 | 2487.17 | 445.10 | 2670.58 |
| 35 |853.44 |5120.64 | 916.37 | 5498.25 |

## Dell Inspiron Desktop 
* Intel Hexacore i5 8400
* 8 GiB DDR4
* Western Digital WD7500AYYS 750GB 7200 RPM
* ~ write

|  k  | Phase 1 (min) | Plot time (min) | Plot size (GiB)  | GiB/minute | working (GiB) | CPU Utilization  | -b setting | Version | Note |
| --- |      ---      |        ---      |     ---          |    ---     |    ---        |      ---         |     ---    |   ---   | ---  |
| 32  |    1526.128   |      4253.52    |    101.3         |   0.024      |   529         |     100  %.      |   default  |   1.9   | Using Native Window Plotter, not WSL2 |

## Dell Inspiron Desktop 
* Intel Hexacore i5 8400
* 8 GiB DDR4
* Toshiba dt01aca100 7200 RPM - 1TB Hard Drive
* ~ write

|  k  | Phase 1 (min) | Plot time (min) | Plot size (GiB)  | GiB/minute | working (GiB) | CPU Utilization  | -b setting | Version | Note |
| --- |      ---      |        ---      |     ---          |    ---     |    ---        |      ---         |     ---    |   ---   | ---  |
| 32  |    1001.865   |      2572.58    |    101.3         |   0.039    |   529         |     100  %.      |   default  |   1.9   | Using Native Window Plotter, not WSL2 |

## MacBook Pro (13-inch, 2019, Four Thunderbolt 3 ports)
* 2.8 GHz Quad-Core Intel Core i7
* 16 GiB 2133 MHz LPDDR3
* APPLE SSD AP1024M
* ~ 2900 MB/s write

|  k  | Phase 1 (min) | Plot time (min) | Plot size (GiB)  | GiB/minute | working (GiB) | CPU Utilization  | -b setting | Version | Note |
| --- |      ---      |        ---      |     ---          |    ---     |    ---        |      ---         |     ---    |   ---   | ---  |
| 30  |    149.91     |      327.83     |    23.79         |   0.0726   |   125.93      |     83.74%.      |   default  |   1.8   |      |

## Ubuntu 19.04
* Intel(R) Xeon(R) W-2155 CPU @ 3.30GHz (10 cores)
* 64 GiB PC4-21300 DDR4-2666V-R REGISTERED ECC
* Western Digital 4 TB [WD10EZEX](https://documents.westerndigital.com/content/dam/doc-library/en_us/assets/public/western-digital/product/internal-drives/wd-blue-hdd/data-sheet-wd-blue-pc-hard-drives-2879-771436.pdf)
* ~ - MB/s write

|  k  | Phase 1 (min) | Plot time (min) | Plot size (GiB)  | GiB/minute | working (GiB) | CPU Utilization  | -b setting | Version | Note |
| --- |      ---      |        ---      |      ---         |    ---     |     ---       |      ---         |     ---    |   ---   | ---  |
| 29  |     60.36     |       128.9     |     11.52        |   0.0893   |     61.00     |     81.77%.      |   default  |   1.8   |      |
| 30  |    131.25     |       302.8     |     23.82        |   0.0787   |    126.02     |     79.86%.      |   default  |   1.8   |      |
| 31  |    333.50     |       724.3     |     49.17        |   0.0679   |    262.04     |     70.66%.      |   default  |   1.8   |      |
| 32  |    674.69     |      1577.7     |     101.3        |   0.0642   |    523.93     |     63.74%.      |   default  |   1.8   |      |
| 33  |    1738.2     |     4285.56     |     208.8        |   0.0487   |   1095.91     |     52.53%       |   default  |   1.8   |      |

## Ubuntu 20.04.1 LTS
* Intel(R) Xeon(R) E3-1270v6 CPU @ 3.80GHz (4 cores)
* Samsung(R) M391A2K43BB1-CRC 32 GiB (2x16) PC4-19200 DDR4 2400Mhz ECC
* Intel(R) P4510 NVMe SSD (2x) RAID 1
* fio randwrite: ~380 MB/s

|  k  | Phase 1 (min) | Plot time (min) | Plot size (GiB)  | GiB/minute | working (GiB) | CPU Utilization  | -b setting | Version | Note |
| --- |      ---      |        ---      |      ---         |    ---     |     ---       |      ---         |     ---    |   ---   | ---  |
| 32  |    325.61     |     ?     |     ?        |   ?   |   ?     |     ?       |   20000  |   1.10   |      |
| 32  |    389.07     |      880.45     |     101.42        |   0.1152   |    524.14     |     85.38%      |   20000  |   1.9   |      |

## Razer Blade Stealth 2018
* Intel(R) Core(TM) i7-8550U CPU @ 1.80GHz, 2001 Mhz, 4 Core(s), 8 Logical Processor(s)
* 16 GiB RAM
* Samsung SSD PM981 MZVLB512HAJQ
* Crystal - CDM 5 Write Seq: 1468 MB/s

|  k  | Phase 1 (min) | Plot time (min) | Plot size (GiB)  | GiB/minute | working (GiB) | CPU Utilization  | -b setting | Version | Note |
| --- |      ---      |        ---      |      ---         |     ---    |     ---       |      ---         |     ---    |   ---   | ---  |
| 31  |     278.5     |      613.09     |     49.134       |    0.0801  |    261.944    |     100.0%       |   default  |   1.8   |      |


## Fedora Server 32
* Intel i7 4770k (2013, 7 year old CPU)
* 32 GB RAM
* NVME SSD (Microcenter brand, raid0 over 2 x 512 GB)
* Iozone v490 (ext4, 256 kiB blocks, fsynced to storage): 876 MiB/s seq write, 3464 MiB/s seq reads

|  k  | Phase 1 (min) | Plot time (min) | Plot size (GiB)  | GiB/minute | working (GiB) | CPU Utilization  | -b setting | Version | Note |
| --- |      ---      |        ---      |      ---         |    ---     |     ---       |      ---         |     ---    |   ---   | ---  |
| 32  |     429.9     |      935.87     |     101.34       |   0.1083   |    523.96*    |     94.36%.      |   default  |   1.8   | *Actual working space 10GiB higher     |


## Fedora Server 32
* AMD Ryzen 7 3800x
* 128 GB RAM
* NVME SSD (Microcenter brand, PCIe v4, single 1 TB)
* --

|  k  | Phase 1 (min) | Plot time (min) | Plot size (GiB)  | GiB/minute | working (GiB) | CPU Utilization  | -b setting | Version | Note |
| --- |      ---      |        ---      |      ---         |    ---     |     ---       |      ---         |     ---    |   ---   | ---  |
| 30  |     67.4      |      143.6      |     23.80        |   0.1657   |    125.95     |     99.60%       |   default  |   1.8   | GiB tmp re/writes 1282 |
| 30  |     59.92     |      125.5      |     23.80        |   0.1657   |    125.95     |     99.67%       |   110000   |   1.8   | GiB tmp re/writes 666 |


## Intel Pentium CPU G4500 in WSL2 on ext4
* Intel Pentium CPU G4500 @ 3.50Ghz - no AVX
* 8 GB RAM
* Samsung SSD 860 EVO 250GB
* ? MB/s

|  k  | Phase 1 (min) | Plot time (min) | Plot size (GiB)  | GiB/minute | working (GiB) | CPU Utilization  | -b setting | Version | Note |
| --- |      ---      |        ---      |      ---         |    ---     |     ---       |      ---         |     ---    |   ---   | ---  |
| 30  |     244.8     |      540.59     |      23.82       |   0.0441   |    126.014    |     83.62%       |   default  |   1.8   |      |

## Intel Pentium CPU G4500 in Windows on NTFS
* Intel Pentium CPU G4500 @ 3.50Ghz - no AVX
* 8 GiB RAM
* Samsung SSD 860 EVO 250GB
* ? MB/s

|  k  | Phase 1 (min) | Plot time (min) | Plot size (GiB)  | GiB/minute | working (GiB) | CPU Utilization  | -b setting | Version | Note |
| --- |      ---      |        ---      |      ---         |    ---     |     ---       |      ---         |     ---    |   ---   | ---  |
| 30  |     153.6     |      359.62     |      23.83       |   0.0663   |    126.038    |      100%        |   default  |   1.8   |      |

## AWS EC2 r5dn.12xlarge
* Processor: 48 Intel(R) Xeon(R) Platinum 8259CL CPU @ 2.50GHz
* Memory: 374GiB
* Storage: tempdir: ramdisk of 310GiB (Ubuntu 18.04) finaldir: Amazon.com, Inc. NVMe SSD
* ~ - MB/s write

|  k  | Phase 1 (min) | Plot time (min) | Plot size (GiB)  | GiB/minute | working (GiB) | CPU Utilization  | -b setting  | Version | Note |
| --- |      ---      |        ---      |      ---         |     ---    |     ---       |      ---         |     ---     |   ---   | ---  |
| 30  |     81.50     |      165.49     |     23.80        |   0.14382  |    125.95     |     99.99%.      |     64GiB   |   1.8   |      |
| 31  |    185.59     |      361.67     |     49.17        |   0.13595  |    262.04     |     99.99%.      |     64GiB   |   1.8   |      |

## AWS EC2 i3en.large
* Processor: 2 Intel(R) Xeon(R) Scalable (Skylake) processors with new Intel Advanced Vector Extension (AVX-512) instruction set @ 3.1 GHz
* Memory: 16 GiB
* Storage: 1 x 1,250 NVMe SSD

|  k  | Phase 1 (min) | Plot time (min) | Plot size (GiB)  | GiB/minute | working (GiB) | CPU Utilization  | -b setting  | Version | Note |
| --- |      ---      |        ---      |      ---         |     ---    |     ---       |      ---         |     ---     |   ---   | ---  |
| 32  |    576.30     |     1360.41     |     101.338      |    0.0745  |    523.946    |     79.19%.      |      6GiB   |   1.8   |      |

## AWS EC2 i3.xlarge
* Processor: 4 Intel(R) Xeon(R) E5-2686 v4 (Broadwell) CPU @ 2.30 GHZ
* Memory: 30.5 GiB
* Storage: 1 x 950 NVMe SSD

|  k  | Phase 1 (min) | Plot time (min) | Plot size (GiB)  | GiB/minute | working (GiB) | CPU Utilization  | -b setting  | Version | Note |
| --- |      ---      |        ---      |      ---         |     ---    |     ---       |      ---         |     ---     |   ---   | ---  |
| 32  |    616.78     |     1340.32     |     101.338      |    0.0742  |    523.997    |     90.96%.      |    20GiB    |   1.8   |      |

## Raspberry Pi 4
* Processor: Broadcom BCM2711, Quad core Cortex-A72 (ARM v8) 64-bit SoC @ 1.5GHz
* Memory: 4 GB LPDDR4-3200 SDRAM
* Storage: Samsung 860 EVO V-NAND SSD 1 TB (SATA to USB 3.0 connection)

|  k  | Phase 1 (min) | Plot time (min) | Plot size (GiB)  | GiB/minute | working (GiB) | CPU Utilization  | -b setting  | Version | Note |
| --- |      ---      |        ---      |      ---         |     ---    |     ---       |      ---         |     ---     |   ---   | ---  |
| 30  |    448.87     |      919.50     |      23.8151     |   0.0259   |    125.996    |     92.92%.      |     2GiB    |   1.8   |      |
| 31  |    940.39     |     1958.23     |      49.1596     |   0.0251   |    262.008    |     92.45%.      |     2GiB    |   1.8   |      |
| 32  |   1857.45     |     3954.88     |     101.3680     |   0.0256   |    524.017    |     93.68%.      |     2GiB    |   1.8   |      |
| 32  |   1869.62     |     4311.85     |     101.2990     |   0.0235   |    523.855    |     85.38%.      |   2.5GiB    |   1.9   |      |

# Historical k sizes - Pre Beta 8 - incorrectly listed as GB - most should be GiB

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
