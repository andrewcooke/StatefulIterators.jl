
# StatefulIterators

A stream-like wrapper around iterable objects (it's spelt "Stateful"
with a trailing lowercase L, and then "Iterators" with a leading,
uppercase I).

## Example

This makes the usual iter-related methods "stateful" - they don't
"reset" to the start of the collection on each use:

```
julia> using StatefulIterators

julia> s = StatefulIterator([1,2,3,4,5])
StatefulIterators.ArrayIterator{Int64}([1,2,3,4,5],1)

julia> collect(take(s, 3))
3-element Array{Any,1}:
 1
 2
 3

julia> collect(take(s, 3))
2-element Array{Any,1}:
 4
 5
```

## Read Methods

The iterators also support read methods similar to streams.

```
julia> s = StatefulIterator([0x1, 0x2, 0x3, 0x4])
StatefulIterators.ArrayIterator{UInt8}(UInt8[0x01,0x02,0x03,0x04],1)

julia> read(s, Int16, 2)
2-element Array{Int16,1}:
  513
 1027
```

Note that type conversion only works for iterators over typed arrays:

```
julia> StatefulIterator("abc")
StatefulIterators.IterIterator("abc",1)

julia> collect(take(StatefulIterator("abc"), 2))
2-element Array{Any,1}:
 'a'
 'b'

julia> read(StatefulIterator("abc"), UInt8)
ERROR: MethodError: `*` has no method matching *(::Type{UInt8})
Closest candidates are:
  *(::Any, ::Any, ::Any, ::Any...)
 in read at /home/andrew/.julia/v0.4/StatefulIterators/src/StatefulIterators.jl:48
```

(I have no idea where that specific error originates, but this fails
in general because we only "know" the underlying type for arrays).

## Warning

This is much less efficient than using normal iterators.
