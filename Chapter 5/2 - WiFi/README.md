# Wi-fi
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

# Official solution 

This is obviously a wi-fi handshake capture file.
We can try cracking it with `aircrack-ng` using the `rockyou` wordlist.
We know that the flag follows the `NTRLGC{.*}` pattern, so a fair thing to do is to mutate the wordlist to make it use this format.
There are infinite ways to do it, but here are two:

- with `python`
```python
with open('rockyou.txt','r', errors='ignore') as rf:
    with open('newrock.txt','w') as nf:
        while True:
            line = rf.readline()
            if not line:
                break
            line = line.strip()
            nf.write("NTRLGC{"+line+"}\n")
```

- with `sed`
```shell
sed 's/^/NTRLGC{/' newrock.txt | sed 's/$/}/' > ntrlgc.txt
```

Now we can start cracking:

```shell
aircrack-ng -w newrock.txt ShipyardWiFi.cap
```

There it goes:

![alt cracked](cracked.jpg)