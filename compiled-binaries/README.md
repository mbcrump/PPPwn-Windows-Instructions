Here's my compiled binaries that I created using WSL targetting firmware 9.00. 

```bash
mbcrump@CRUMPPC:~/exploit/PPPwn$ make -C stage1 FW=900 clean && make -C stage1 FW=900
make: Entering directory '/home/mbcrump/exploit/PPPwn/stage1'
make: Leaving directory '/home/mbcrump/exploit/PPPwn/stage1'
make: Entering directory '/home/mbcrump/exploit/PPPwn/stage1'
gcc    -c -o start.o start.S
gcc -DSMP -isystem ../freebsd-headers/include -Wl,--build-id=none -Os -fno-stack-protector -DFIRMWARE=900   -c -o stage1.o stage1.c
gcc -DSMP -isystem ../freebsd-headers/include -Wl,--build-id=none -Os -fno-stack-protector -DFIRMWARE=900 start.o stage1.o -o stage1.elf -T linker.ld -nostartfiles -nostdlib
objcopy -S -O binary stage1.elf stage1.bin
make: Leaving directory '/home/mbcrump/exploit/PPPwn/stage1'
mbcrump@CRUMPPC:~/exploit/PPPwn$ make -C stage2 FW=900 clean && make -C stage2 FW=900
make: Entering directory '/home/mbcrump/exploit/PPPwn/stage2'
make: Leaving directory '/home/mbcrump/exploit/PPPwn/stage2'
make: Entering directory '/home/mbcrump/exploit/PPPwn/stage2'
gcc    -c -o start.o start.S
gcc -DSMP -isystem ../freebsd-headers/include -Wl,--build-id=none -Os -fno-stack-protector -DFIRMWARE=900   -c -o stage2.o stage2.c
gcc -DSMP -isystem ../freebsd-headers/include -Wl,--build-id=none -Os -fno-stack-protector -DFIRMWARE=900 start.o stage2.o -o stage2.elf -T linker.ld -nostartfiles -nostdlib
objcopy -S -O binary stage2.elf stage2.bin
make: Leaving directory '/home/mbcrump/exploit/PPPwn/stage2'
mbcrump@CRUMPPC:~/exploit/PPPwn$
```
