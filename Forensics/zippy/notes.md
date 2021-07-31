# Author:Panagiotis Fiskilis/Neuro

# Challenge name:BsidesSF CTF 2019: zippy

Lore:

```
Can you read the flag from the PCAP?
```

## Solution: ##

We do some basic enumeration with some low hanging fruits:

```bash
strings zippy.pcapng
binwalk zippy.pcapng
binwalk -e zippy.pcapng
cd _zippy.pcapng.extracted/
```
From the <code>strings</code> command we find a password for a zip file that we extracted via <code>binwalk</code>

Zip password:supercomplexpassword

```bash
unzip 4DE.zip
cat flag.txt
```

## Flag: ##

CTF{this_flag_is_your_flag}
