# FIO

`sudo apt update`

`sudo apt install fio`


save this file as a job file called "chia.fio" or whatever you want, and change filename to the path of the drive or RAID volume that you want to test

```
[global]
bs=128K
iodepth=256
direct=1
ioengine=libaio
group_reporting
time_based
name=chia
log_avg_msec=1000
bwavgtime=1000
filename=/<yourRAID>/fiotest.tmp
size=100G

[rd_qd_256_128k_1w]
stonewall
bs=128k
iodepth=256
numjobs=1
rw=read
runtime=60
write_bw_log=seq_read_bw.log

[wr_qd_256_128k_1w]
stonewall
bs=128k
iodepth=256
numjobs=1
rw=write
runtime=60
write_bw_log=seq_write_bw.log
```

run `sudo fio chia.fio`