# doublearray-es

効率的なキー・バリューの保存とプレフィックスベースの検索のための、Double-Array（ダブル配列）トライのJavaScript実装です。

## 機能

- キー・バリューのペアによる単語登録
- 完全一致検索（`contain`, `lookup`）
- 共通接頭辞検索（`commonPrefixSearch`）
- 永続化のためのバッファのシリアライズ/デシリアライズ

## 必要条件

ESモジュールをサポートするモダンなJavaScript環境（例: モダンブラウザやNode.js）。

## 使い方

### トライの構築

キー・バリューのオブジェクトの配列から、または `append` メソッドのチェーンによってトライを構築できます。

**配列から構築する場合:**

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

**メソッドチェーンを使用する場合:**

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

### 検索操作

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

### 保存と読み込み

内部バッファ（`BASE` および `CHECK` 配列）を抽出してトライの状態を保存し、後で読み込むことができます。

**保存:** バッファを `Int32Array` 型付き配列として取得します。

```javascript
const base_buffer = trie.bc.getBaseBuffer();
const check_buffer = trie.bc.getCheckBuffer();
```

**読み込み:** バッファから新しい `DoubleArray` インスタンスを作成します。

```javascript
const loaded_trie = doublearray.load(base_buffer, check_buffer);

// 読み込んだトライは検索が可能です
console.log(loaded_trie.lookup('奈良先端')); // -> 4
```

## ライセンス

MIT License — 詳細は [LICENSE](LICENSE) を参照してください。
