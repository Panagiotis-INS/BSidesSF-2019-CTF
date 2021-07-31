# Author:Panagiotis Fiskilis/Neuro

# Challenge name:BsidesSF CTF 2019: diskimage

Lore:

```
Disk "image"
```

## Solution: ##

Of course the challenge is a steganography challenge so we will have to do our basic stego enumeration:

```bash
strings diskimage.png
binwalk -e diskimage.png
foremost diskimage.png
java -jar stegsolve
```
Aaaand nothing.

I later tried the <code>zsteg</code> tool and got 

```bash
zsteg diskimage.png
zsteg -a diskimage.png |grep -i "dos"
zsteg -a diskimage.png -e 'b8,rgb,lsb,xy' > diskimage.img
```

From the challenge indicator we know that it has to do with DOS and Hard drive Forensics

```bash
file diskimage.img
mkdir mount_point
sudo testdisk diskimage.img
```
We <code>undelete</code> the 

```
_LAG.ICO
```

Use <code>eog</code> to open the icon and get the flag.

## Flag: ##

CTF{FAT12_FTW}
