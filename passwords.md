| Level | Password Used To Reach The Level | Command Used |
| :--- | :--- | :--- |
| 1 | ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If | `cat readme` |
| 2 | 263JGJPfgU6LtdEvgfWU1XP5yac29mFx | `cat ./-` |
| 3 | MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx | `cat ./"--spaces in this filename--"` |
| 4 | 2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ | `cd inhere` `ls -a` |
| 5 | 4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw | `cd inhere` `file ./*` |
| 6 | HWasnPhtq9AVKe0dmk45nxy20cvUa6EG | `cd inhere` `find . -type f -size 1033c ! -executable` |
| 7 | morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj | `find / -user bandit7 -group bandit6 -size 33c 2>/dev/null` |
| 8 | dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc | `grep "millionth" data.txt"` |
| 9 | 4CKMh1JI91bUIZZPXDqGanal4xvAg0JM | `sort data.txt \| uniq -u` |
| 10 | FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey | `strings data.txt \| grep "=="` |
| 11 | dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr | `base64 -d data.txt` |
| 12 | 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4 | `cat data.txt \| tr 'A-Za-z' 'N-ZA-Mn-za-m'` |
| 13 | FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn | `mktemp -d` `cp ~/data.txt .` `xxd -r data.txt > data2` `mv data2 data2.gz \|\| gunzip data2.gz` `bunzip2 data2` `mv data2.out data2.gz \|\| gunzip data2.gz` `tar -xf data2` `tar -xf data5.bin` `bunzip2 data6.bin` `tar -xf data6.bin.out` `mv data8.bin data8.bin.gz \|\| gunzip data8.bin.gz` `cat data8.bin` |
| 14 | MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS | `scp -P 2220 bandit13@bandit.labs.overthewire.org:~/sshkey.private ./bandit14.id_rsa` `chmod 600 bandit14.id_rsa` `ssh -i bandit14.id_rsa bandit14@bandit.labs.overthewire.org -p 2220` |
| 15 | 8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo | `cat etc/bandit_pass/bandit14` `nc localhost 30000`|
| 16 | kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx | `openssl s_client -connect localhost:30001` |
| 17 | EReVavePLFHtFlFsjn3hyzMlvSuSAcRD | `nmap -sV -p 31000-32000 localhost` `openssl s_client -connect localhost:31790 -ign_eof` |
| 18 | x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO | `diff passwords.new passwords.old` |
| 19 | cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8 | `ssh bandit18@bandit.labs.overthewire.org "cat readme"` |
| 20 | 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO | `find / -perm -4000 -user bandit20 2>/dev/null` `./bandit20-do cat /etc/bandit_pass/bandit20` |
| 21 | EeoULMCra2q0dSkYj561DX7s1CpBuOBt | `nc -l localhost 1234 &` `./suconnect 1234` |
| 22 | tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q | `cat /etc/cron.d/cronjob_bandit22` `cat /usr/bin/cronjob_bandit22.sh` `cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv`|
| 23 | 0Zf11ioIjMVN551jX3CmStKLYqjk54Ga | `echo "I am bandit23 \| mdsum5 \| cut -d ' ' -f 1` |
| 24 | gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 | `cd /tmp` `mkdir myexploit` `nano /tmp/myexploit/exploit.sh` `cat /etc/bandit_pass/bandit24 > /tmp/myexploit/password.txt` `chmod 777 exploit.sh` `touch password.txt` `chmod 777 password.txt` `cp exploit.sh var/spool/bandit24/foo` |
| 25 | iCi86ttT4KSNe1armKiwbQNmB3YJP3q4 | `nano tmp/loop.sh` `chmod 777 loop.sh` `touch list.txt` `chmod 777 list.txt` `./loop.sh` `cat list.txt \| nc localhost 30002` |