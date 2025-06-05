# Name `type` Method

## Summary

Support fetching a type's name, extending the generic type `name` method for use with all types.

## Motivation

Accessing the name of types provides developers with [
  comparison of types via name, 
  easier to make understandable type error messages, 
  ability to process types based on their name,
  ability to easily check cyclic types,
].  The expected outcome is an improvement to development experience as more complex types are desired for creation.

## Design

We propose an extension to allow for the generic type `name` method to acquire the name of the type even if it is not generic.
The name of a type is defined when the type is initialized, ie. for `type A = string` the name of the type would be `A`.
The name of a type will be nonexistant (`nil`) should the type itself be the argument, ie. `type B = C<"Hello World!">`.

### Example

Printing the type names.
```luau
type Bar = number

type function printName(input: type)
  print(input:name() or "Nameless")
  return types.never
end

type Foo = printName<Bar> -- Type Error: Bar
type Goo = printName<"Hello"> -- Type Error: Nameless
```

### Update `type` Instance
| New | Instance Methods | Return Type | Description |
| --- | ---------------- | ----------- | ----------- |
| New | `name()` | `string?` | name of the type, `nil` for inferred types |


## Drawbacks

Backwards compatibility with code that relies on only generic types having a name is broken.

## Alternatives

type's __tostring() metamethod returns the name if the type is named, else it returns `type{typeHash}`.
