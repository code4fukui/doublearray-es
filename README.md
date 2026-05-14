# doublearray-es

> 日本語のREADMEはこちらです: [README.ja.md](README.ja.md)

JavaScript implementation of a Double-Array trie for efficient key-value storage and prefix-based searching.

## Features

-   Word registration with key-value pairs
-   Exact match lookup (`contain`, `lookup`)
-   Common prefix search (`commonPrefixSearch`)
-   Buffer serialization/deserialization for persistent storage

## Requirements

A modern JavaScript environment supporting ES modules (e.g., modern browsers or Node.js).

## Usage

### Build a Trie

You can build a trie from an array of key-value objects or by chaining `append` calls.

**From an array:**

```javascript
import { doublearray } from "https://code4fukui.github.io/doublearray-es/doublearray.js";

const words = [
    { k: 'a', v: 1 },
    { k: 'abc', v: 2 },
    { k: '奈良', v: 3 },
    { k: '奈良先端', v: 4 },
    { k: '奈良先端科学技術大学院大学', v: 5 }
];

const trie = doublearray.builder().build(words);
```

**Using method chaining:**

```javascript
import { doublearray } from "https://code4fukui.github.io/doublearray-es/doublearray.js";

const trie = doublearray
       .builder()
       .append('a', 1)
       .append('abc', 2)
       .append('奈良', 3)
       .append('奈良先端', 4)
       .append('奈良先端科学技術大学院大学', 5)
       .build();
```

### Search Operations

```javascript
trie.contain('a');
// -> true

trie.lookup('abc');
// -> 2

trie.commonPrefixSearch('奈良先端科学技術大学院大学');
// -> [ { v: 3, k: '奈良' },
//      { v: 4, k: '奈良先端' },
//      { v: 5, k: '奈良先端科学技術大学院大学' } ]
```

### Save and Load

You can extract the internal buffers (`BASE` and `CHECK` arrays) to save the trie's state and load it back later.

**Save:** Get the buffers as `Int32Array` typed arrays.

```javascript
const base_buffer = trie.bc.getBaseBuffer();
const check_buffer = trie.bc.getCheckBuffer();
```

**Load:** Create a new `DoubleArray` instance from the buffers.

```javascript
const loaded_trie = doublearray.load(base_buffer, check_buffer);

// The loaded trie is ready for searching
console.log(loaded_trie.lookup('奈良先端')); // -> 4
```

## License

MIT License — see [LICENSE](LICENSE).