# Support for debug.info and debug.traceback in Type Functions

## Summary

One paragraph explanation of the feature.
<TODO ALTER>
Add support for `debug.info` and `debug.traceback` in type functions to improve the debugging process.

## Motivation

Why are we doing this? What use cases does it support? What is the expected outcome?
<TODO>
There are several cases where the type solver errors when using user-defined type functions and the error messages fail to indicate the exact point of failure.
For example, the resulting error will be `TypeError: 'clear' type function errored at runtime: [string "prettyerror"]:___: intersection`:
```luau
type function simplepretty(t: type)
  if t:is("singleton") then
    return tostring(t:value())
  end
  return t.tag
end

type function prettyerror(message: type)
  error(simplepretty(message))
end

type function clear(tab: type)
  if not tab:is("table") then
    prettyerror(tab)
  end
  return types.newtable()
end

type EmptyTab = clear<{foo: string} & {bar: number}>
```
There are also situations where the type function should only ever be used by a specific type function, however these cases are rare.

## Design

This is the bulk of the proposal. 
Explain the design in enough detail for somebody familiar with the language to understand, and include examples of how the feature is used.
<TODO>
function debug.info(level: number, s: string): ...any
function debug.info(f: function, s: string): ...any
function debug.traceback(msg: string?, level: number?): string


## Drawbacks

Why should we *not* do this?
<TODO>
It's unclear if this proposal has any drawbacks.


## Alternatives

What other designs have been considered? What is the impact of not doing this?
<TODO IMPACT?>
As opposed to using `debug.traceback`, `error` could be used at different levels in order to simulate the traceback.  
For `debug.info` there is no alternate solution.
