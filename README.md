# AugPersist

## prepare and compile
### compile the afl-fuzz 
`make`

### compile the afl-qemu-loop-trace for tracing the BBs when locating the PBB
```
cd AugPersist/qemu_loop_mode/
./build_qemu_support.sh
```

### compile the afl-qemu-trace for tracing the BBs when fuzzing the target
```
cd AugPersist/qemu_mode/
./build_qemu_support.sh
```


## fuzz the persistent software by stdio
### trace the BBs of the target.
`cd AugPersist/demo`

`touch /tmp/loop_trace`

`AugPersist/afl-fuzz -i in/ -o out/ -P std -t 2000 -Q ./demo_svr_std`

`cp /tmp/loop_trace /tmp/loop_trace1`

`AugPersist/afl-fuzz -i in/ -o out/ -P std -t 1800 -Q ./demo_svr_std`

### locate the PBB of the target.
`AugPersist/finder/findPBB /tmp/loop_trace /tmp/loop_trace1`

### fuzz the persistent target with the PBB (specified by -L parameter).
`AugPersist/afl-fuzz -i in/ -o out/ -p std -R reserve -L 0x400000095c -Q ./demo_svr_std`


## fuzz the persistent software by network socket
### Modify send_net_packet() in afl-fuzz.c to fit the port and data format of the target network protocol
`...`

### trace the BBs of the target.
`cd AugPersist/demo`

`touch /tmp/loop_trace`

`AugPersist/afl-fuzz -i in/ -o out/ -P net -t 4000 -Q ./demo_svr_net`

`cp /tmp/loop_trace /tmp/loop_trace1`

`AugPersist/afl-fuzz -i in/ -o out/ -P net -t 3000 -Q ./demo_svr_net`

### locate the PBB of the target.
`AugPersist/finder/findPBB /tmp/loop_trace /tmp/loop_trace1`

### fuzz the persistent target with the PBB (specified by -L parameter).
`AugPersist/afl-fuzz -i in/ -o out/ -p net -R reserve -L 0x4000000c89 -Q ./demo_svr_net`