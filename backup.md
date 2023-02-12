## Backup

It's actually a summary for the practice in 1/14/2023

1. Things to backup:
 */etc, /var/www/html, /home, /root ....
 
2. process:
  * create folder to backup
    ** mkdir /root/backups
    ** cd backups
    ** mkdir etc home usr-bin usr-sbin var-www ...
  * backup with rsync
    ** rsync -avP [target file] [location]
    ** ex. rsync -avP /var/www /root/backups/var-www
  * compress backups and encrypt
    ** cd ..
    ** tar czvf backups.tgz backups
    */ gpg -c backups.tgz
  * (in another backup computer)
    ** scp root@ip:/root/backups.tgz.gpg . 

3.So if the system is compromised
  * (in another backup computer)
    ** scp ./backups.tgz.gpg root@ip:/root/backups.tgz.gpg
  * decrypt
    ** gpg -o backups.tgz -d backups.tgz.gpg
  * decompress
    ** tar xzvf backups.tgz
