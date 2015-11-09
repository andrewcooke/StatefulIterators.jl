
# StatefulIterators

A stream-like wrapper around iterable objects.

## Example

```
julia> using StatefulIterators

julia> s = StatefulIterator([1,2,3,4,5])
StatefulIterators.StatefulIterator([1,2,3,4,5],1)

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

## Warning

This is much less efficient than using normal iterators.
