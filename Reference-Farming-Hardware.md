# Reference Farming Hardware

The farming process is very lightweight and can be ran with minimal CPU and DRAM resources. The goal of a good farming platform is to have the maximum amount of capacity in the least amount of space, using as little power as possible. In other words, the priority for a farming platform, independent of obtaining storage for the lowest cost possible, is to have the highest amount of TB/W in a small space.

## JBOD, DAS (direct attached storage)
A JBOD, or “Just a bunch of disks” is a device dedicated to housing a large amount of hard disk drives, and does not contain any integrated compute resources. A JBOD is typically made up of an enclosure, enclosure slots that identify each drive individually, a SAS expander and backplane, fans, and power supplies. All the disks in a JBOD can be accessed by a single SAS cable connected to a host server or desktop through a HBA (host bus adapter) which converts a PCIe slot to SAS.

### Example
Mainstream JBOD – 45 disks in 4U chassis. Referred to in the farming channel as the SM45, this can be found on eBay for $300-400 making it a very cost efficient for medium to large size farms
https://www.supermicro.com/en/products/chassis/4U/847/SC847E16-RJBOD1 

High drive count – 90 disks in 4U chassis. highest density on the market, but typically goes for $1200-2000 used.

https://www.supermicro.com/en/products/chassis/4U/946/SC946ED-R2KJBOD 

**Pros**
High number of slots. Fully integrated power supplies and fans. Uses SAS enclosure management to identify slots in software and identify a failed device with an LED locate function. Can use SAS or SATA drives.

**Cons**
Fans can be loud. Heavy. Requires data center rack to be mounted correctly.

## Desktop farming
A desktop in a full tower can house between 12-16 drives. This is a great setup for small farmers as desktops are the easiest to build and manage for PC enthusiasts. A full tower case that houses many drives can be found from many different vendors at a low cost. Typical desktop motherboards contain between 6-10 SATA ports, so expanding past that will also require a SAS HBA.
Pros – cheap, easy to configure and customize
Cons – need to build yourself and source 

### Examples
https://www.corsair.com/us/en/Categories/Products/Cases/Obsidian-Series%E2%84%A2-750D-Full-Tower-ATX-Case/p/CC-9011035-WW

## NAS farming
A NAS, or networked attached storage, is a device dedicated to having hard drives included in a backplane and a lightweight CPU and DRAM. NAS serves storage through the network (as opposed to DAS, or direct attached storage)

### Examples
https://www.synology.com/en-us/products/DS1821+

**Pros** – high number of drives in small space, extremely power efficient
**Cons** – expensive compared to other options, plugin required for farmer or harvested (not complete yet), typically setup with redundancy for data protection which is not required for farming. SATA drives only (which is fine for most)

# DIY builds
