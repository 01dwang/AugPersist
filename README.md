# AFL_Persistent


## find the loop start point of the loop demo server(by std)
`cd demo`

`touch /tmp/loop_trace`

`~/AugPersist/afl-fuzz -i in/ -o out/ -P std -t 2000 -Q ./demo_svr_std`

`cp /tmp/loop_trace /tmp/loop_trace1`

`~/AugPersist/afl-fuzz -i in/ -o out/ -P std -t 1800 -Q ./demo_svr_std`

`cd finder`

`./findPBB`



## find the loop start point of the loop demo server(by net)
`cd demo`

`touch /tmp/loop_trace`

`~/AFL/afl-fuzz -i in/ -o out/ -P net -t 4000 -Q ./demo_svr_net`

`cp /tmp/loop_trace /tmp/loop_trace1`

`~/AFL/afl-fuzz -i in/ -o out/ -P net -t 3000 -Q ./demo_svr_net`

`cd finder`

`./findPBB`


## fuzz the the loop demo server (break the loop logic)
### by std
`~/AFL/afl-fuzz -i in/ -o out/ -p std -R breakit -L 0x400000095c -Q ./demo_svr_std`

### by net
`~/AFL/afl-fuzz -i in/ -o out/ -p net -R breakit -L 0x4000000c89 -Q ./demo_svr_net`



## fuzz the the loop demo server (reserve the loop logic)
### by std
`~/AFL/afl-fuzz -i in/ -o out/ -p std -R reserve -L 0x400000095c -Q ./demo_svr_std`

### by net
`~/AFL/afl-fuzz -i in/ -o out/ -p net -R reserve -L 0x4000000c89 -Q ./demo_svr_net`