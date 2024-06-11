# AugPersist

## prepare and compile
### 1. compile the afl-fuzz 
`make`

### 2. compile the afl-qemu-loop-trace for tracing the BBs when locating the PBB
```
cd AugPersist/qemu_loop_mode/
./build_qemu_support.sh
```

### 3. compile the afl-qemu-trace for tracing the BBs when fuzzing the target
```
cd AugPersist/qemu_mode/
./build_qemu_support.sh
```


## fuzz the persistent software by stdio
### 1. trace the BBs of the target.
`cd AugPersist/demo`

`touch /tmp/loop_trace`

`AugPersist/afl-fuzz -i in/ -o out/ -P std -t 2000 -Q ./demo_svr_std`

`cp /tmp/loop_trace /tmp/loop_trace1`

`AugPersist/afl-fuzz -i in/ -o out/ -P std -t 1800 -Q ./demo_svr_std`

### 2. locate the PBB of the target.
`AugPersist/finder/findPBB /tmp/loop_trace /tmp/loop_trace1`
<p align="center">
  <img alt="Light" src="https://github.com/01dwang/AugPersist/blob/master/screenshots/findPBB_for_demo_svr_std.png" width="45%">
</p>

### 3. fuzz the persistent target with the PBB (specified by -L parameter).
`AugPersist/afl-fuzz -i in/ -o out/ -p std -R reserve -L 0x400000095c -Q ./demo_svr_std`
<p align="center">
  <img alt="Light" src="https://github.com/01dwang/AugPersist/blob/master/screenshots/AugPersist_fuzz_demo_svr_std.png" width="45%">
</p>

### 4. Compare with the fuzzing results of pure AFL for 'demo_svr_std_patched_loop_for_AFL' and 'demo_svr_std_patched_loop_and_init_for_AFL'
```
AFL/afl-fuzz -i in/ -o out/ -t 1500 -Q ./demo_svr_std_patched_loop_for_AFL
AFL/afl-fuzz -i in/ -o out/ -Q ./demo_svr_std_patched_loop_and_init_for_AFL
```
<p align="center">
  <img alt="Light" src="https://github.com/01dwang/AugPersist/blob/master/screenshots/AFL_fuzz_demo_svr_std_patched1.png" width="45%">
&nbsp; &nbsp; &nbsp; &nbsp;
  <img alt="Dark" src="https://github.com/01dwang/AugPersist/blob/master/screenshots/AFL_fuzz_demo_svr_std_patched2.png" width="47%">
</p>


## fuzz the persistent software by network socket
### 1. Modify send_net_packet() in afl-fuzz.c to fit the port and data format of the target network protocol
`...`

### 2. trace the BBs of the target.
`cd AugPersist/demo`

`touch /tmp/loop_trace`

`AugPersist/afl-fuzz -i in/ -o out/ -P net -t 4000 -Q ./demo_svr_net`

`cp /tmp/loop_trace /tmp/loop_trace1`

`AugPersist/afl-fuzz -i in/ -o out/ -P net -t 3000 -Q ./demo_svr_net`

### 3. locate the PBB of the target.
`AugPersist/finder/findPBB /tmp/loop_trace /tmp/loop_trace1`
<p align="center">
  <img alt="Light" src="https://github.com/01dwang/AugPersist/blob/master/screenshots/findPBB_for_demo_svr_net.png" width="45%">
</p>

### 4. fuzz the persistent target with the PBB (specified by -L parameter).
`AugPersist/afl-fuzz -i in/ -o out/ -p net -R reserve -L 0x4000000c89 -Q ./demo_svr_net`
<p align="center">
  <img alt="Light" src="https://github.com/01dwang/AugPersist/blob/master/screenshots/AugPersist_fuzz_demo_svr_net.png" width="45%">
</p>