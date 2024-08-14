# Hash Hash Hash
TODO introduction

## Building
```
make
```

## Running
```shell
TODO how to run and results
```

## First Implementation
In the `hash_table_v1_add_entry` function, I added a lock in the first line of the function. This version adds a single global lock for the entire hash table, which means that each thread needs to access this lock in order to access the hash table. 

### Performance
```shell
TODO how to run and results
```
Version 1 is slower than the base version. As TODO

## Second Implementation
In the `hash_table_v2_add_entry` function, I TODO

### Performance
```shell
TODO how to run and results
```

TODO more results, speedup measurement, and analysis on v2

## Cleaning up
```
make clean
```