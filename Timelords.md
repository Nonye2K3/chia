# Alpha testnet Timelords

Initially, the fastest timelord we've see so far is Chia's own twin Intel machines running Ubuntu on an Intel(R) Xeon(R) W-2155 CPU @ 3.30GHz with 10 cores and  64 GB PC4-21300 DDR4-2666V-R REGISTERED ECC. These machines get about 145,000 ips (iterations per second using alpha 1.3/1.4.) The fastest we've run is on a z1d instance at EC2 which gets about 166,000 ips. We have seen (approximately 2/17/20) a VDF server capable of ~200,000 ips.

It looks like it takes two full cores (usually looks like 4 in the OS) to both run the squarings and run the proofs and it takes about 8GB of RAM per VDF - though that may now be down to 6GB or even 4GB with some changes implemented right at testnet launch.

Chia Network has four timelords running:

*   Two twins are each a timelord with two VDF's each
    *   [Intel(R) Xeon(R) W-2155](https://ark.intel.com/content/www/us/en/ark/products/125042/intel-xeon-w-2155-processor-13-75m-cache-3-30-ghz.html) CPU @ 3.30GHz with 10 cores
    *   64 GB PC4-21300 DDR4-2666V-R REGISTERED ECC
    *   ~145,000 ips

*   Older large instance with 21 cores (42 reported) that has 6 VDF's running on it
    *   [Intel(R) Xeon(R) CPU E7-L8867](https://ark.intel.com/content/www/us/en/ark/products/53577/intel-xeon-processor-e7-8867l-30m-cache-2-13-ghz-6-40-gt-s-intel-qpi.html)  @ 2.13GHz
    *   63G RAM
    *   ~57,000 ips
    *   It tends to garbage collect if the twins both choose the same possible proofs of space to run a VDF on

*   Expiremental cluster timelord
    *   [AWS t3small](https://aws.amazon.com/ec2/instance-types/t3/) as timelord
    *   3 VDFs on 3 AWS c5n.xlarge instances
    *   [Intel(R) Xeon(R) Platinum 8124M](https://en.wikichip.org/wiki/intel/xeon_platinum/8124m) CPU @ 3.00GHz
    *   10 GB RAM
    *   ~110,000 ips
    *   Still optimizing this

*   AWS EC2 z1d.6xlarge 24 vCPU's reported
    *   [AWS z1d.6xlarge](https://aws.amazon.com/ec2/instance-types/z1d/) as timelord
    *   8 VDFs
    *   [Intel(R) Xeon(R) Platinum 8151](https://www.cpubenchmark.net/cpu.php?cpu=Intel+Xeon+Platinum+8151+%40+3.40GHz&id=3458) CPU @ 3.40GHz - turbo 4.00GHz
    *   192 GB RAM
    *   ~166,000 ips