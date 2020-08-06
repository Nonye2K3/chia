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

# Cluster Timelord

*Note: Out of date for Beta - will update soon*

The default distribution in alpha 1.5 will now launch 2 VDF instances on the current machine from the `run_all.sh` startup script. However one can have each VDF or couple of VDFs on different machines. We tend run a cluster in AWS with the fullnode and timelord running on a t3.small and then a single VDF on a group of e.g. c5.xlarge instances.

On the Timelord server you will need to edit the `scripts/run_timelord.sh` script and comment out `_run_bg_cmd python -m src.timelord_launcher`. In `config/config.yaml` you will add the IP address of each client machine under `vdf_clients` and enter an `ips_estimate` for each VDF client machine. VDF client machines must be able to see the port in `vdf_server: port:`. The default config has chosen port 8000. Additionally you will need to set the `vdf_server: host:` to either the IP that you want your clients to reach or leave the entry blank for all of your configured IP interfaces.

You can test your server configuration and port availability by running `nmap -Pn -p 8000 TIMELORD_SERVER` on the client machines where TIMELORD_SERVER is the hostname or IP address of the Timelord.

On each client, you place the IP of the Timelord server in the `timelord_launcher: host:` section of `config_yaml` on the client machine. Note that the `port:` here must match what you choose on the Timelord server. Also you can choose how many VDFs run on the client by setting `timelord_launcher: process_count` to 1 to only run one VDF on the client machine. The default is set at 2. Once the Timelord server is started you can start the VDF client(s) by issuing `python -m src.timelord_launcher &` from the Python virtual environment.