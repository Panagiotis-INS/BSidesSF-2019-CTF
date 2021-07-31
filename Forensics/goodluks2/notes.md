# Author:Panagiotis Fiskilis/Neuro

# Challenge name:BsidesSF CTF 2019: Goodlucks 2

## Lore: ##

```
Our first insider threat has lead to a second insider. We haven't found any clues to the passphrase here, but given the vocabulary of the suspect, I don't think you'll have a hard time.
```

Link for the challenge file:

```
https://www.mediafire.com/file/7y1pv38jn9ctcln/goodluks2.7z/file
```

The challenge only gives us a .7z file with a disk image inside from the challenge indicator it is clear that we have to crack a Luks password from the drive

So we use <code>testdisk</code> in order to create a mountable Linux partition from the .img

Now in order to get the flag we have to brute-force the Luks password

After a quick search and by knowing that we have to deal with a Luks encrypted drive I found a Luks brute-forcer in github:

```
https://github.com/glv2/bruteforce-luks
```

```bash
sudo ./bruteforce-luks -t 4 -f /opt/1337/rockyou.txt /dev/loop0
```
After a long sleep we get the password and after the disk decryption we also get the flag

Luks password:gaffer3

## Flag: ##
CTF{lame_users_keys_suck}
