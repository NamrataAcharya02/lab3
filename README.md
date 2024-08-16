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
This version adds a single global lock for the entire hash table, which means that each thread needs to access this lock in order to access the hash table. Hence, the mutex lock is declared for the entire hash table in `struct hash_table_v1`. In the `hash_table_v1_add_entry` function, I locked the mutex in the first line of the function to prevent race conditions and keep any access to the hash table itself in the critical section. This keeps the hash table protected as a single global lock prevents access to it, which means threads will not be competing in race conditions when modifying values and there will be no deadlocks due to untimely context switches. Threads instead wait to obtain the single lock, ensuring each thread completes running before the next one executes. Only one thread runs at a time, ensuring correctness as operations happen in a sequential manner.

To run this implementation with 4 threads and 100000 entries per thread, run:

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
This strategy assigns a mutex lock for each bucket in the hash table, which represents separate linked lists. In this method, threads need not wait for the entire hash table to be available and can instead work on separate linked lists at the same time due to the separate mutexes. 

In the `hash_table_v2_add_entry` function, I added the mutex locking call before getting the head of the linked list that the thread is trying to access. This is because multiple threads may be trying to insert to the same linked list and hence change the head pointers, which can become corrupted due to race conditions. To prevent this, we need to add fetching the head in the critical section. Only once a thread fetches the head, inserts an entry and reassigns the head can another thread work with the head and that linked list again. This makes the code concurrent and preserves correct operations without any missing entries.

To run this implementation with 4 threads and 100000 entries per thread, run:

### Running
```
./hash-table-tester -t 4 -s 100000

```
### Performance
```
TODO how to run and results
```

As we can see, Version 2 is faster than both base and Version 1 implementation. Due to multiple locks and a lock for each linked list in the hash table, threads no longer have to wait for the previous thread to finish its execution. The contention between threads for the data structure is reduced as different threads can work on different sections of the hash table at the same time. This allows concurrent use of the hash table and speeds up operations. It also makes it faster than the base implementation as instead of a single threaded execution, we can concurrently carry out the operations using multiple threads. This improvement is more significant than the cost of locking/unlocking mutexes, which makes version 2 faster than base. 


## Cleaning up
```
make clean
```
