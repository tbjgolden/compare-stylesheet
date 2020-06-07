# `compare-stylesheet`

[![npm version](https://img.shields.io/npm/v/compare-stylesheet.svg?style=flat-square)](https://www.npmjs.com/package/compare-stylesheet)
[![npm downloads](https://img.shields.io/npm/dm/compare-stylesheet.svg?style=flat-square)](https://www.npmjs.com/package/compare-stylesheet)
![Coveralls Github](https://img.shields.io/coveralls/github/tbjgolden/ainsley?style=flat-square)

JavaScript methods (and TypeScript bindings) to normalize a stylesheet by the
rules, properties and values it contains.

This allows for comparison by contents (equality, superset).

It uses `csso` to ensure the stylesheet is normalized (and to remove redundant
code).

After this, it parses the rest, looking for @rules, selectors, properties and
values and checks them for equivalence.

It (intentionally) ignores the order of the rules, so a more loose comparison
can be made, making it particularly useful for unit tests for code that
manipulates CSS.

## Basic Usage

The base function is compare, which returns the omissions in each stylesheet.

```js
import { compare } from 'compare-stylesheet'

const first = `
.lightgray {
  color: lightgray;
  background-color: black;
}
@supports (display: flex) and (-webkit-appearance: checkbox) {
  .red {
    color: red;
  }
}`

const second = `
.blue {
  color: blue;
}
@supports (display: flex) and (-webkit-appearance: checkbox) {
  .red {
    color: red;
    background: white;
  }
}`

const [missingInFirst, missingInSecond] = compare(first, second)

console.log(missingInFirst)
/*
[
  [".blue", "color", "#00f"],
  ["@supports (display:flex) and (-webkit-appearance:checkbox)", ".red", "background", "#fff"]
]
*/

console.log(missingInSecond)
/*
[
  [".lightgray", "background-color", "#000"],
  [".lightgray", "color", "#d3d3d3"]
]
*/
```

Other methods include:

- `normalize(css)` which turns a stylesheet into a JSON friendly array of
  strings (which can be then compared safely with JSON.stringify)
- `areEqual(a, b)` which returns true when two stylesheets are functionally
  equivalent (ignoring order of rules)
- `isSubsetOf(subsheet, supersheet)` which returns true when subsheet's
  definitions are completely contained by supersheet (again, ignoring order of
  rules)

## Installation

## [`API`](docs/api)

## License

MIT
