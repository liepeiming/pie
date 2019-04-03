# 🥧 `github.com/elliotchance/pie` [![GoDoc](https://godoc.org/github.com/elliotchance/pie?status.svg)](https://godoc.org/github.com/elliotchance/pie)

**Enjoy a slice!** `pie` is a utility library for dealing with slices that
focuses on type safety and performance.

It can be used with the Go-style package functions:

```go
names := []string{"Bob", "Sally", "John", "Jane"}
shortNames := pie.StringsOnly(names, func(s string) bool {
	return len(s) <= 3
})

// pie.Strings{"Bob"}
```

Or, they can be chained for more complex operations:

```go
pie.Strings{"Bob", "Sally", "John", "Jane"}.
	Without(pie.Prefix("J")).
	Transform(pie.ToUpper()).
	Last()

// "SALLY"
```

# Functions

## Slices

| Strings | Ints | Description | Performance |
| ------- | ---- | ----------- | ----------- |
| [`StringsContains`](https://godoc.org/github.com/elliotchance/pie#StringsContains) | [`IntsContains`](https://godoc.org/github.com/elliotchance/pie#IntsContains) | Check if the value exists in the slice. | O(n) |
| [`StringsFirst`](https://godoc.org/github.com/elliotchance/pie#StringsFirst) | [`IntsFirst`](https://godoc.org/github.com/elliotchance/pie#IntsFirst) | The first element, or a zeroed value. | O(1) |
| [`StringsFirstOr`](https://godoc.org/github.com/elliotchance/pie#StringsFirstOr) | [`IntsFirstOr`](https://godoc.org/github.com/elliotchance/pie#IntsFirstOr) | The first element, or a default value. | O(1) |
| [`StringsLast`](https://godoc.org/github.com/elliotchance/pie#StringsLast) | [`IntsLast`](https://godoc.org/github.com/elliotchance/pie#IntsLast) | The last element, or a zeroed string. | O(1) |
| [`StringsLastOr`](https://godoc.org/github.com/elliotchance/pie#StringsLastOr) | [`IntsLastOr`](https://godoc.org/github.com/elliotchance/pie#IntsLastOr) | The last element, or a default value. | O(1) |
| [`StringsOnly`](https://godoc.org/github.com/elliotchance/pie#StringsOnly) | [`IntsOnly`](https://godoc.org/github.com/elliotchance/pie#IntsOnly) | A new slice containing only the elements that returned true from the condition. | O(n) |
| [`StringsTransform`](https://godoc.org/github.com/elliotchance/pie#StringsTransform) | [`IntsTransform`](https://godoc.org/github.com/elliotchance/pie#IntsTransform) | A new slice where each element has been transformed. | O(n) |
| [`StringsWithout`](https://godoc.org/github.com/elliotchance/pie#StringsWithout) | [`IntsWithout`](https://godoc.org/github.com/elliotchance/pie#IntsWithout) | A new slice containing only the elements that returned false from the condition. | O(n) |

## Conditional

| Strings | Ints | Description |
| ------- | ---- | ----------- |
| [`EqualString`](https://godoc.org/github.com/elliotchance/pie#EqualString) | [`EqualInt`](https://godoc.org/github.com/elliotchance/pie#EqualInt) | Check if the values are equal. |
| [`Prefix`](https://godoc.org/github.com/elliotchance/pie#Prefix) |  | Check if the string starts with another string. |
| [`Suffix`](https://godoc.org/github.com/elliotchance/pie#Suffix) |  | Check if the string ends with another string. |

## Transforms

Some of the functions listed here are part of the Go native libraries, but they
are useful to list.

### Strings

| Function | Description |
| -------- | ----------- |
| [`strings.ToLower`](https://golang.org/pkg/strings/#ToLower) | Convert string to lower case. |
| [`strings.ToTitle`](https://golang.org/pkg/strings/#ToTitle) | Convert string to title case. |
| [`strings.ToUpper`](https://golang.org/pkg/strings/#ToUpper) | Convert string to upper case. |
| [`strings.TrimSpace`](https://golang.org/pkg/strings/#TrimSpace) | Trim leading and trailing whitespace. |

### Ints

| Function | Description |
| -------- | ----------- |
| [`AddInt`](https://godoc.org/github.com/elliotchance/pie#AddInt) | Addition. |

# FAQ

## How do I use it?

You can include it like any other package through your favourite package
manager:

1. Go modules ([you should be using this one](http://elliot.land/post/migrating-projects-from-dep-to-go-modules)):
`go get -u github.com/elliotchance/pie`

2. Dep: `dep ensure -add github.com/elliotchance/pie`

## Why do we need another library for this?

Yes, there are some other great options like
[`thoas/go-funk`](https://github.com/thoas/go-funk),
[`leaanthony/slicer`](https://github.com/leaanthony/slicer),
[`viant/toolbox`](https://github.com/viant/toolbox) and
[`alxrm/ugo`](https://github.com/alxrm/ugo) to name a few.

A lot of my work is dealing with servers that need to be high performance. I
found myself creating all the same utility functions like StringSliceContains
because I wanted to avoid reflection.

## What are the goals of `pie`?

1. **Type safety.** I never want to hit runtime bugs because I could pass in the
wrong type, or perform an invalid type case out the other end.

2. **Performance.** The functions need to be as fast as native Go
implementations otherwise there's no point in this library existing.

3. **Nil-safe.** All of the functions will happily accept nil and treat them as
empty slices. Apart from less possible panics, it makes it easier to chain.

There are some downsides with this approach:

1. It won't support all slice types. Sorry, you can use these actions on
`[]Foo`.

2. Until
[parametric polymorphism (generics) possibly arrives in Go v2](https://go.googlesource.com/proposal/+/master/design/go2draft-generics-overview.md)
there will need to be duplicate code in `pie` to compensate.

## Can I contribute?

Absolutely. Pull requests are always welcome.
