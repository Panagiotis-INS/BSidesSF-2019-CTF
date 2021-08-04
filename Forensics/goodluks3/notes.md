# Author:Panagiotis Fiskilis/Neuro

# Challenge name:BsidesSF CTF 2019: Goodlucks 3

## Lore: ##

```
Our third suspect was caught with a running machine with the encrypted disk mounted. We captured the whole hard drive and system memory for you. Can you help us?
```

Solution:

As always we get a .7z file with the hard drive image(.img file) and a mem dump(.mem file)

So probably we will need to get the Luks decryption key from the mem dump

But let's start with the disk using <code>testdisk</code> we create a backup and start the investigation:

## Disk Forensics: ##

The disk has 2 Linux Partitions and a swap

We backup only the first 2

## Memory Forensics: ##

We know that the pc runs Linux so we need to build the user profile for volatility version2

```bash
strings goodluks3.mem |grep -i "welcome to ubuntu"
```

We get:

```
Welcome to Ubuntu 12.04.5 LTS (GNU/Linux 3.13.0-32-generic x86_64)
```

Luckily the volatility foundation provides this profile so we don't have to setup it up from a VM

```bash
git clone https://github.com/volatilityfoundation/volatility.git
mv Ubuntu12045.zip volatility/volatility/plugins/overlays/linux/
python2 volatility/vol.py --info |grep "Ubuntu"
```

We are up and running

```bash
mkdir Volatility_dump
python2 volatility/vol.py goodluks3.mem --profile=LinuxUbuntu12045x64 -f goodluks3.mem linux_pslist
python2 volatility/vol.py goodluks3.mem --profile=LinuxUbuntu12045x64 -f goodluks3.mem linux_psaux
python2 volatility/vol.py goodluks3.mem --profile=LinuxUbuntu12045x64 -f goodluks3.mem linux_psxview
python2 volatility/vol.py goodluks3.mem --profile=LinuxUbuntu12045x64 -f goodluks3.mem linux_bash
```

After some enumeration we find a suspicious process:

<mark> 0x0000000008a917f0 crypto                   32 True   True   True     False      False   True   </mark>

After some trial and error found this article with the <code>findaes</code> tool to find the master key inside the memory dump

```
https://blog.appsecco.com/breaking-full-disk-encryption-from-a-memory-dump-5a868c4fc81e
```

We follow the article:

```bash
./findaes ../goodluks3.mem
```

We get the aes keys for the Luks decryption:


```
Found AES-256 key schedule at offset 0x895dd88: 
b0 7a 29 f5 44 15 47 76 57 04 6e ec d3 03 f5 bd af a4 e6 df b2 71 01 ab af 7e 22 e1 23 94 15 f5 
Found AES-256 key schedule at offset 0x895df78: 
8e 8c 3a 67 eb 11 54 6c b1 cc 7d 0f cc 85 e8 43 30 7c 16 d4 7f 86 08 a1 0f 59 3d 4c 31 0f c8 6a 
```

## AES Master Key: ##

```bash
echo '8e 8c 3a 67 eb 11 54 6c b1 cc 7d 0f cc 85 e8 43 30 7c 16 d4 7f 86 08 a1 0f 59 3d 4c 31 0f c8 6a b0 7a 29 f5 44 15 47 76 57 04 6e ec d3 03 f5 bd af a4 e6 df b2 71 01 ab af 7e 22 e1 23 94 15 f5' | tr -d ' ' | xxd -r -p > key0
```

```bash
sudo cryptsetup luksOpen --master-key-file key0 /dev/loop0 decrypted
```

## Flag: ##

CTF{lucky_U_k33p_secrets!}
