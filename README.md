# Hash Hash Hash
This implements a concurrent version of a hash table by using two different locking strategies. The first strategy uses a single lock, while the second one uses multiple locks. The goal is to make the hash table thread-safe such that there are no missing entries and is also performance optimized. 

## Building
To build the program, run
```
make
```

## Running

To run this program, we need to specify the number of threads to use and the number of hash table entries per thread. To do this, run the following command:

```
./hash-table-tester -t <number of threads> -s <number of entries per thread>
```
For example, if we wanted to use 4 threads and 100,000 entries per thread, we would run:
```
./hash-table-tester -t 4 -s 100000
```
The result displays the performance of a base strategy that uses no threading, the first locking strategy and the second locking strategy. The performance time is displayed in mu seconds. 

## First Implementation
This version adds a single global lock for the entire hash table, which means that each thread needs to access this lock in order to access the hash table. Hence, the mutex lock is declared for the entire hash table in `struct hash_table_v1`. In the `hash_table_v1_add_entry` function, I locked the mutex in the first line of the function to prevent race conditions and keep access to the hash table itself in the critical section. To run this implementation with 4 threads and 100000 entries per thread, run:

### Running
```
./hash-table-tester -t 4 -s 100000

```
### Performance
```
./hash-table-tester -t 4 -s 100000

```

Version 1 is slower than the base version. The base version uses no threads, so it operates as a single threaded program. Despite Version 1 using multiple threads, it is slower since there is a significant overhead added by the locking mechanism. Since we have restricted access to the entire hash table through a global lock, threads wait for that single lock and hence, the program ends up functioning like a single threaded program. Only one thread can do any operations on the hash table. The additional performance cost of locking and unlocking mutexes means that Version 1 is slower than the base version. 

## Second Implementation
In the `hash_table_v2_add_entry` function, I 

### Performance
```shell
TODO how to run and results
```

TODO more results, speedup measurement, and analysis on v2

## Cleaning up
```
make clean
```
