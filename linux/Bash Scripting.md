# Bash Scripting
[TOC]
## cut 
(e.g.: `cat ip.txt | grep "64 bytes" | cut -d " " -f 4`)

* -d specifies delimiter to cut on is " "
* -f specifies field (once the delimiter is used to determine what separates the fields, such as a “,”

## tr
(e.g.: `cat ip.txt | grep “64 bytes” | cut -d “ ” -f 4 | tr -d “:”`)

* -d specifies the character to remove



## Bash Loop

``` sh
#!/bin/bash

for ip in `seq 1 254`; do
    ping -c 1 $1.$ip | grep "64 bytes" | cut -d " " -f 4 | tr -d ":" &
done
```

``` sh
for ip in $(cat iplist.txt); do nmap -sS -p 80 -T4 $ip & done
```



