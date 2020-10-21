# JavaScript 配列操作

## 参考ページ

- [MDN Array](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array)

## よく使うメソッド

- forEach
- map
- reduce
- sort
- filter
- find
- findIndex
- includes
- indexOf
- every
- some
- pop
- push
- shift
- unshift
- from

### forEach

- 配列の各要素に対して一度ずつ実行
- 基本的な配列処理

```js
const array = ['a', 'b', 'c'];
array.forEach((element) => console.log(element));

// output
// a
// b
// c
```

### map

- 配列の各要素に対して一度ずつ実行、その結果からなる新しい配列を生成
- forEach と map の違いは戻り値の有無が違う
- 使用例：配列を加工して新しい配列を生成

```js
const array = [1, 4, 9, 16];
const result = array.map((x) => x * 2);

console.log(result);

// output
// [ 2, 8, 18, 32 ]
```

### reduce

- 配列の買う要素に対して一度ずつ実行、単一の値を生成
- 第二引数に初期値を設定できる
- 使用例：配列の合計を求める場合に使われる

```js
const array = [1, 2, 3, 4];
const result1 = array.reduce(
  (accumulator, currentValue) => accumulator + currentValue
);
console.log(result1);

// output
// 10

const result2 = array.reduce(
  (accumulator, currentValue) => accumulator + currentValue,
  10
);
console.log(result2);

// output
// 20
```

### sort

- 既定のソート順は昇順で、要素を文字列に変換してから、辞書的な順番で並び替え
- 使用例：ソート

```js
const months = ['March', 'Jan', 'Feb', 'Dec'];
months.sort();
console.log(months);

// output
// [ 'Dec', 'Feb', 'Jan', 'March' ]

const array = [1, 30, 4, 21, 100000];
array.sort();
console.log(array);

// output
// [ 1, 100000, 21, 30, 4 ]
```

- よく使う大きさ順のソート

- [参考ページ](https://qiita.com/ndj/items/689e3eec398fc3c564fe)

```js
const array = [1, 30, 4, 21, 100000];
array.sort((a, b) => a - b);

console.log(array);

// output
// [ 1, 4, 21, 30, 100000 ]
```

### filter

- 条件を満たした、すべての配列からなる新しい配列を生成
- 使用例：null や undefined など余計なデータが入っている配列の要素の除外

```ts
const words = [
  'spray',
  'limit',
  'elite',
  'exuberant',
  'destruction',
  'present',
];
const result = words.filter((word) => word.length > 6);

console.log(result);

// output
// [ 'exuberant', 'destruction', 'present' ]
```

### find

- 条件を満たす配列内の最初の要素の値を返す

```ts
const array = [5, 12, 8, 130, 44];
const found = array.find((element) => element > 10);

console.log(found);

// output
// 12
```

### findIndex

- 条件を満たす最初の要素の位置を返す。条件を満たす要素がない場合は-1 返す

```ts
const array = [5, 12, 8, 130, 44];
const found = array.findIndex((element) => element > 10);

console.log(found);

// output
// 1
```

### includes

- 特定の要素が配列に含まれているかどうかを true または false で返す

```ts
const array = [1, 2, 3];

console.log(array.includes(2));
console.log(array.includes(4));

// output
// true
// false
```

### indexOf

- 引数に与えられた内容と同じ内容を持つ配列要素の内、最初のものの添字を返す。存在しない場合は -1 を返す
- findIndex との違いは引数がコールバックか直接配列の要素を指定するかの違い
  - findIndex がコールバックを使う。indexOf は要素をそのまま指定

```ts
const words = ['ant', 'bison', 'camel', 'duck', 'bison'];

console.log(words.indexOf('bison'));
console.log(words.indexOf('bison', 2));

// output
// 1
// 4
```

### every

- 配列内のすべての要素が指定された関数で実装された条件を満たしているか確認

```ts
const isBelowThreshold = (value) => value < 40;
const array = [1, 30, 39, 29, 10, 13];

console.log(array.every(isBelowThreshold));

// output
// true
```

### some

- 配列内の１つ以上の要素が指定された関数で実装された条件を満たしているか確認

```ts
const isBelowThreshold = (value) => value < 10;
const array = [1, 30, 39, 29, 10, 13];

console.log(array.some(isBelowThreshold));

// output
// true
```

### pop

- 配列から最後の要素を取り除き、その要素を返す

```ts
const array = [1, 2, 3, 4, 5];
array.pop();

console.log(array);
console.log(array.pop());

// output
// [ 1, 2, 3, 4 ]
// 4
```

### push

- 配列の末尾に 1 つ以上の要素を追加

```ts
const array = [1, 2, 3, 4, 5];

array.push(6);
console.log(array);

array.push(7, 8, 9);
console.log(array);

// output
// [ 1, 2, 3, 4, 5, 6 ]
// [ 1, 2, 3, 4, 5, 6, 7, 8, 9 ]
```

### shift

- 配列から最初の要素を取り除き、その要素を返す

```ts
const array = [1, 2, 3, 4, 5];
array.shift();

console.log(array);
console.log(array.shift());

// output
// [ 2, 3, 4, 5 ]
// 2
```

### unshift

- 配列の最初に 1 つ以上の要素を追加

```ts
const array = [1, 2, 3, 4, 5];

array.unshift(6);
console.log(array);

array.unshift(7, 8, 9);
console.log(array);

// output
// [ 6, 1, 2, 3, 4, 5 ]
// [ 7, 8, 9, 6, 1, 2, 3, 4, 5 ]
```

## 似たようなメソッド

### forEach/map

- forEach
  - 戻り値が**ない**
- map
  - 戻り値が**ある**

### find/findIndex・indexOf/ includes

- find
  - **要素**が欲しい場合
- findIndex・indexOf
  - **要素の位置**のみ欲しい場合
- includes
  - **要素が含まれているか**の確認、true か false のみが欲しい場合

### every/some

- every
  - **全て**条件を満たしたら true
- some
  - **一つでも**条件を満たしたら true

### pop/push/shift/unshift

- pop
  - 配列の**一番後**の要素を**取り出す**
- push
  - 配列の**一番後**に要素を**追加する**
- shift
  - 配列の**一番前**の要素を**取り出す**
- unshift
  - 配列の**一番前**に要素を**追加する**
