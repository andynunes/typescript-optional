# TypeScript Optional

[![npm](https://img.shields.io/npm/v/typescript-optional.svg)](https://www.npmjs.com/package/typescript-optional)
[![License](https://img.shields.io/npm/l/typescript-optional.svg)](https://www.npmjs.com/package/typescript-optional)
[![Build Status](https://travis-ci.org/bromne/typescript-optional.svg?branch=master)](https://travis-ci.org/bromne/typescript-optional)
[![Coverage Status](https://coveralls.io/repos/github/bromne/typescript-optional/badge.svg?branch=master)](https://coveralls.io/github/bromne/typescript-optional?branch=master)

Optional (like Java) implementation in TypeScript

## Overview

`Optional<T>` is a type which *may* or *may not* contain a *payload* of type `T`.
It provides a common interface regardless of whether an instance is *present* or is *empty*. 

This module is inspired by [Optional class in Java 8+](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html).

### Install

```
npm install --save typescript-optional
```

## Usage

### import

```ts
// import Optional type from this module
import Optional from 'typescript-optional';
```

### creating `Optional<T>` objects

```ts
let nullableString: string | null = /* some nullable value */;

// all the following variables will be parameterized as Optional<string>.
let optional = Optional.ofNullable(nullableString);
let optionalPresent1 = Optional.ofNullable("foo");
let optionalPresent2 = Optional.ofNonNull("foo"); // accepts non-null value (or else throws TypeError)
let optionalEmpty1: Optional<string> = Optional.empty(); // type hinting required
let optionalEmpty2 = Optional.empty<string>(); // or parameterize explicitly
```

### operations

```ts
let optional: Optional<string> = Optional.ofNullable( /* some optional value: null | string */ );

// force to retrieve the payload. (or else throws TypeError.)
// this method is not used match.
optional.get();

// be whether a payload is present or not.
optional.isPresent

// be whether this is empty or not. (negation of `isPresent` property)
optional.isEmpty

// execute the given consumer if a payload is present.
optional.ifPresent(value => console.log(value));

// execute the first argument (consumer) if a payload is present, execute the second argument (emptyAction) otherwise.
optional.ifPresentOrElse(value => console.log(value), () => console.log("empty"));

// filter a payload with additional predicate.
optional.filter(value => value.length > 0);

// map a payload with the given mapper.
optional.map(value => value.length);

// map a payload with the given mapper which returns value wrapped with Optional type.
let powerIfPositive: (x: Number) => Optional<Number>
    = x => (x > 0) ? Optional.ofNonNull(x * x) : Optional.empty();
let numberOptional: Optional<number> = Optional.ofNullable(/* some optional value: null | number */)
numberOptional.flatMap(value => powerIfPositive(value));

// return this if this is present, return the given another optional otherwise.
let another: Optional<string> = Optional.ofNullable(/* ... */);
optional.or(another);

// retrieve a payload if this is present, return the given value otherwise.
optional.orElse("bar");

// retrieve a payload if this is present, return a value supplied by the given function otherwise.
optional.orElseGet(() => "bar");

// retrieve a payload if this is present, throws an exception supplied by the given function otherwise.
optional.orElseThrow(() => new Error());
```

## License

MIT License - [LICENSE.md](LICENSE.md)
