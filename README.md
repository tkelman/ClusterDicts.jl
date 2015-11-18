# ClusterDicts

Distributed dictionary for Julia.

GlobalDict and DistributedDict
---------------------------

This package provides two types of distributed dictionaries:

    - GlobalDict where the same key-value pair is stored on all participating workers
    - DistributedDict where the key-value pairs are distributed over the participating workers based on the key's hash value

Constructors
------------

    - `GlobalDict(pids=PidsWorkers(); name::AbstractString=next_name(), ktype=Any, vtype=Any)` where
        - `pids` is one of `PidsAll()`, `PidsWorkers()` or `Pids([ids...])` where
            - `PidsAll()` represents all processes in the cluster
            - `PidsWorkers()` represents all worker processes in the cluster
            - `Pids([ids...])` represents a specific subset of workers identified by their pids.

        - `name` is a logical name for the dictionary. It should be unique across any such dictionary created across
        modules or different packages. Usually left unspecified in which case the system creates a unique name.

        - `ktype` is the type of the keys

        - `vtype` is the type of the values

    - `DistributedDict(pids=PidsWorkers(); name::AbstractString=next_name(), ktype=Any, vtype=Any)` similar to the `GlobalDict` constructor.


Methods
-------

The following methods of `Associative` are available.

    - `isempty`
    - `length`
    - `get`
    - `get!`
    - `getindex`
    - `setindex!`
    - `pop!`
    - `delete!`


Garbage Collection
------------------

The global and distributed dictionaries created by `GlobalDict` and `DistributedDict` are NOT collected when the objects go out of scope.
They have to be necessarily released by calling `delete!(d::GlobalDict)` and `delete!(d::DistributedDict)` respectively.


TODO
----
    - Concurrent updates : This current version makes no attempt to handle race conditions when multiple tasks or processes
    update the same key. For example, if `d` is a `GlobalDict`, `d[k] = v` sets `k` to `v` on all participating workers.
    If two tasks try to set `d[k] = v1` and ``d[k] = v2` at the same time, there is no guarantee the value will be either
    all `v1` or all `v2` on all workers.

    - More `Associative` related methods supported.

    - Handle worker exits : Resiliency in the event of workers exiting has to be handled.
