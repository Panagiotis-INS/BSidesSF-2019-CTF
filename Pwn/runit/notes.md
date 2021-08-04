# Author:Panagiotis Fiskilis/Neuro

# Challenge name:BsidesSF CTF 2019: runit

Lore:

```
Send code to the server, and it'll run! Grab the flag from /home/ctf/flag.txt
```

Solution:

We write a simple exploit for intel 80386 cpu with <code>shellcraft</code> and send it via the <code>asm()</code> and <code>sendline()</code>

** Flag: **
CTF{you_ran_it}
