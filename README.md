# hiredis-locker

Redis key locking implementation for hiredis C library. It implements something like described
here: http://redis.io/commands/setnx. But it uses lua scripts for both locking and unlocking.
This allows to perform the following patterns as single atomic operation:
 - lock key and read data
 - write back data and release lock

## Benchmarks
On my "Intel(R) Core(TM) i3 CPU         550  @ 3.20GHz" desktop running linux:

    ./bench_incr: 36697.959 locks/sec    # Key increment with redis INCR command
    ./bench_locker: 10579.007 locks/sec  # Manual key increment with lua locking
    ./bench_native: 6825.620 locks/sec   # Manual key increment with direct SETNX, GET, SET, DEL sequence

On "Intel(R) Xeon(R) CPU           E5620  @ 2.40GHz"x2 server running freebsd:

    ./bench_incr: 34183.209 locks/sec
    ./bench_locker: 8562.157 locks/sec
    ./bench_native: 6617.033 locks/sec

## TODO

Implement locking using new SET command syntax, http://redis.io/commands/set.
