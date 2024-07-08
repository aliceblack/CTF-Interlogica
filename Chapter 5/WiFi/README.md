The secret is the wifi password, the pcap has a EAPOL auth to bruteforce.

I use as a base for the dictionary the file "rockyou.txt":
https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt
Adding for each line NTRLGC{ at the start and } at the end.
For example, using VI:
```
:%s/^/NTRLGC{/
:%s/$/}/
:w
```

Then to start the BruteForce the command is:
```
aircrack-ng -w rockyou_exited.txt ShipyardWiFi.cap

```
![immagine](https://github.com/aliceblack/CTF-Interlogica/assets/9288402/c7091e0b-db96-401a-9b76-c262b30afea3)
