# Run-time configuration

RediSearch supports a few run-time configuration options that should be determined when loading the module. In time more options will be added. 

!!! tip "Passing Configuration Options"
    In general, passing configuration options is done by appending arguments after the `--loadmodule` argument in the command line, `loadmodule` configuration directive in a Redis config file, or the `MODULE LOAD` command. For example:
    
    In redis.conf:

    ```
    loadmodule redisearch.so OPT1 OPT2
    ```

    From redis-cli:

    ```
    127.0.0.6379> MODULE load redisearch.so OPT1 OPT2
    ```

    From command line:

    ```
    $ redis-server --loadmodule ./redisearch.so OPT1 OPT2
    ```

# RediSearch configuration options

## TIMEOUT

The maximum amount of time **in Milliseconds** that a search query is allowed to run. If this time is exceeded, we return the top results accumulated so far. 

**NOTE**: This works only in concurrent mode, so enabling `SAFEMODE` disables this option.

### Default

500

### Example

```
$ redis-server --loadmodule ./redisearch.so TIMEOUT 100
```

---

## SAFEMODE

If present in the argument list, RediSearch will turn off concurrency for query processing, and work in a single thread.

This is useful if data consistency is extremely important, and avoids a situation where deletion of documents while querying them can cause momentarily inconsistent results (i.e. documents that were valid during the invocation of the query are not returned because they were deleted during query processing).

### Default
Off (not present)

### Example

```
$ redis-server --loadmodule ./redisearch.so SAFEMODE
```

---

## EXTLOAD {file_name}

If present, we try to load a RediSearch extension dynamic library from the specified file path. See [Extensions](/Extensions) for details.

### Default

None

### Example

```
$ redis-server --loadmodule ./redisearch.so EXTLOAD ./ext/my_extension.so
```

---

## NOGC

If set, we turn off Garbage Collection for all indexes. This is used mainly for debugging and testing, and should not be set by users.

### Default

Not set

### Example

```
$ redis-server --loadmodule ./redisearch.so NOGC
```

---

## MINPREFIX

The minimum number of characters we allow for prefix queries (e.g. `hel*`). Setting it to 1 can hurt performance.

### Default

2

### Example

```
$ redis-server --loadmodule ./redisearch.so MINPREFIX 3
```

---

## MAXEXPANSIONS

The maximum number of expansions we allow for query prefixes. Setting it too high can cause performance issues.

### Default

200

### Example

```
$ redis-server --loadmodule ./redisearch.so MAXEXPANSIONS 1000
```
