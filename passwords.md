| Level | Password | Command Used |
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