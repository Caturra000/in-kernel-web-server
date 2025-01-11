# In-kernel web server

## Introduction

```
server_kernel.c: Implementation of the web server in kernel mode
server_user.c:   Implementation of the web server in user mode
```

You can build them using `make kernel` and `make user`, respectively.

NOTES:
* You need to modify the kernel source code; follow the link below for guidance.
* Zero-copy test is available by enabling `zerocopy_flag`.

More details: https://www.bluepuni.com/archives/in-kernel-web-server/

## wrk

```c
// in-kernel-web-server
// insmod server_kernel.ko num_threads=8
~/in_kernel_web_server$ wrk -t12 -c400 -d10s --latency "http://127.0.0.1:8848/"
Running 10s test @ http://127.0.0.1:8848/
  12 threads and 400 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency   583.60us    2.09ms  57.67ms   95.59%
    Req/Sec   151.83k    41.89k  214.34k    54.02%
  Latency Distribution
     50%  131.00us
     75%  257.00us
     90%    0.85ms
     99%    9.94ms
  18259600 requests in 10.10s, 0.88GB read
Requests/sec: 1807901.24
Transfer/sec:     89.66MB
```

```c
// user-space-web-server
// ./server_user 8
~/in_kernel_web_server$ wrk -t12 -c400 -d10s --latency "http://127.0.0.1:8848/"
Running 10s test @ http://127.0.0.1:8848/
  12 threads and 400 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency   567.92us    2.46ms 110.91ms   97.24%
    Req/Sec   120.37k    31.57k  207.68k    66.75%
  Latency Distribution
     50%  211.00us
     75%  337.00us
     90%    0.90ms
     99%    8.08ms
  14282576 requests in 10.05s, 708.29MB read
Requests/sec: 1421451.12
Transfer/sec:     70.49MB
```

## Flame graphs

![kernel](/flamegraph-kernel.svg)

![user](/flamegraph-user.svg)
