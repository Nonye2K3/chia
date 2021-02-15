The farming process is very lightweight and can be run with minimal CPU and DRAM resources. The goal of a good farming platform is to have the maximum amount of capacity in the least amount of space, using as little power as possible. In other words, the priority for a farming platform, independent of obtaining storage for the lowest cost possible, is to have the highest amount of TB/W in a small space.

## Desktop farming
A desktop in a full tower can house between 12-16 drives. This is a great setup for small farmers as desktops are the easiest to build and manage for PC enthusiasts. A full tower case that houses many drives can be found from many different vendors at a low cost. Typical desktop motherboards contain between 6-10 SATA ports, so expanding past that will also require a SAS HBA.
Pros – cheap, easy to configure and customize
Cons – need to build yourself and source 

### Examples
[Obsidian Series™ 750D Full Tower ATX Case](https://www.corsair.com/us/en/Categories/Products/Cases/Obsidian-Series%E2%84%A2-750D-Full-Tower-ATX-Case/p/CC-9011035-WW)

<img src="https://preview.redd.it/xd8bgja34vg61.png?width=960&crop=smart&auto=webp&s=b4879c70c0afc1a79a0157b9d1f3abbc61e3c590" width="300">

[Source for pic](https://www.reddit.com/r/DataHoarder/comments/lhp1g7/first_nas_build_update_corsair_750d/)

A desktop board can be put into an easily obtainable [Rosewill 4U Server Chassis Case](https://www.amazon.com/dp/B0091IZ1ZG/ref=cm_sw_em_r_mt_dp_RQRJF9S2PHGBPC6DQ90D). This case features up to 16 drives and 7 fans included, and just needs a standard desktop PSU to get going.


## DIY farms
There are many unique DIY builds in the farming hardware channel that find unique uses for repurposing existing hardware to mount drives.
Here a build from user @xorinox that houses 32 drives farming off a [Rock Pi 4](https://rockpi.org/rockpi4) and USB hubs for an average power consumption of 75W & 1.79kWh per day - easily making it one of the most power-efficient farms built!

<img src="https://user-images.githubusercontent.com/61642896/103497288-0d53be00-4e0f-11eb-98b6-c9bffe57d3f3.jpg" width="700">

## JBOD, DAS (direct-attached storage)
A JBOD, or “Just a bunch of disks” is a device dedicated to housing a large number of hard disk drives, and does not contain any integrated compute resources. A JBOD is typically made up of an enclosure, enclosure slots that identify each drive individually, a SAS expander and backplane, fans, and power supplies. All the disks in a JBOD can be accessed by a single SAS cable connected to a host server or desktop through a HBA (host bus adapter) which converts a PCIe slot to SAS.

![SM45](https://www.supermicro.com/a_images/products/Chassis/4U/SC847-RJBOD_spec.jpg)

### Example
Mainstream JBOD – 45 disks in 4U chassis. Referred to in the farming channel as the SM45, this can be found on eBay for $300-400 making it very cost-efficient for medium to large size farms
[Supermicro SuperChassis 847E16-RJBOD1](https://www.supermicro.com/en/products/chassis/4U/847/SC847E16-RJBOD1)

Recommended HBAs to attach to host - LSI 9200-8e [(ebay)](https://www.ebay.com/sch/i.html?_from=R40&_trksid=p2380057.m570.l1313&_nkw=LSI+9200-8e&_sacat=0), 9200-16e [(ebay)](https://www.ebay.com/sch/i.html?_from=R40&_trksid=p2380057.m570.l1311&_nkw=lsi+sas9200-16e&_sacat=0) along with SFF-8088 to SFF-8088 1M External SAS Cable, or 9300 [(ebay)](https://www.ebay.com/sch/i.html?_from=R40&_trksid=p2380057.m570.l1311&_nkw=lsi+9300&_sacat=0) or 9400-8e [(ebay)](https://www.ebay.com/sch/i.html?_from=R40&_trksid=p2380057.m570.l1311&_nkw=9400-8e&_sacat=0) with SAS SFF-8644 to SFF-8088 cable

High drive count – 90 disks in 4U chassis. highest density on the market, but typically goes for $1200-2000 used.
[Supermicro SuperChassis 946ED-R2KJBOD](https://www.supermicro.com/en/products/chassis/4U/946/SC946ED-R2KJBOD)

<img src="https://www.supermicro.com/CDS_Image/uploads/chassis/sc946ed_ad_pull-out_new_20150727.jpg" width="300">

**Pros**
High number of slots. Fully integrated power supplies and fans. Uses SAS enclosure management to identify slots in software and identify a failed device with an LED locate function. Can use SAS or SATA drives.

**Cons**
Fans can be loud. Heavy. Requires data center rack to be mounted correctly.

## NAS farming
A NAS, or networked attached storage, is a device dedicated to having hard drives included in a backplane and a lightweight CPU and DRAM. NAS serves storage through the network (as opposed to DAS, or direct-attached storage)

### Examples
[Synology DiskStation DS1821+](https://www.synology.com/en-us/products/DS1821+)

<img src="https://www.synology.com/img/products/detail/DS1821plus/heading@2x.png" width="300">

**Pros** – high number of drives in small space, extremely power efficient

**Cons** – expensive compared to other options, plugin required for farmer or harvester (not complete yet), typically setup with redundancy for data protection which is not required for farming. SATA drives only (which is fine for most)
