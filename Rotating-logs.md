# Linux

On modern Linux systems Logrotate makes it easy to manage your log files over time. This is a decent [Logrotate explainer](https://www.digitalocean.com/community/tutorials/how-to-manage-logfiles-with-logrotate-on-ubuntu-16-04) but the quick version for Chia looks like:
```bash
sudo nano /etc/logrotate.d/chia
```
Make the following the contents of that file, adjust for your username and version, save the file, and exit.
```bash
/home/username/.chia/beta-1.0b3/log/debug.log {
  rotate 4
  weekly
  compress
  missingok
  notifempty
}
```
The first line is the log file you wish to rotate. 

The next line is how many rotations to keep. Line 3 is how long should logrotate wait to create the first rotated log - here we've chosen weekly so we'll end up with 4 rotated logs and one current log - a little more than a month of logs.

We've specified on line 4 that the rotated logs should be gzipped to save space using the 'compress' keyword.

Finally we tell logrotate that it doesn't need to throw an error if the log file is missing and if the file remains empty, it should not rotate it.
