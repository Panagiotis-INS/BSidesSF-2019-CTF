# Author:Panagiotis Fiskilis/Neuro

# Challenge name:BsidesSF CTF 2019: TheKey

Lore:

```
Can you read flag.txt from the pcap?
```

## Solution: ##

```bash
tshark -r ./out.pcapng -Y 'usb' -T fields -e usb.capdata > usbPcapData
python2 usbkeyboard.py usbPcapData |tee flag.txt
```

## Flag: ##

CTF{MY_FAVOURITE_EDITOR_IS_VIM}
