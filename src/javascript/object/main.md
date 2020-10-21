# Object

## 参考ページ
```
jsprimer https://jsprimer.net/basic/object/
MDN https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Object
```

## はじめに
基本的にJavascriptで扱われるインスタンスの大体全てが```Object```の派生となっている。例えばユーザー定義のclassやDateといったものも、全て```Object```に属する型として扱われる。

この章では```Object```について、プリミティブとの差異、ユーザー定義、基礎操作を理解出来ればと思います。

## プリミティブ
Javascript以外でもよくある基礎的なデータ型。
symbolは明確な目的がないと使わないため、記憶とどめる程度でOK
```
参考url https://developer.mozilla.org/ja/docs/Glossary/Primitive
```

- number
- string
- boolean
- BigInt  
> numberよりも大きな数値を扱うことができる型。
コード内で数値を記載する場合は、末尾に```n```を付けるか```BigInt()```の引数に数値を書く。  
Mathやnumberとの計算は出来ず、少数を表すこともできないのが特徴。
```javascript
const bigInt1 = 9999999999999999999n;
console.log(bigInt1); // 9999999999999999999n
const bigInt2 = 10n + 3n;
console.log(bigInt2); // 13n
const bigInt3 = 10n / 3n;
console.log(bigInt3); // 3n
```
- symbol
> symbolはデバッグ用のもので、全く同じインスタンスが生成されることがない型。
```
参考url: https://developer.mozilla.org/ja/docs/Glossary/Symbol
```
- undefined
> 型が未定義であることを表すプリミティブ値。
undefinedが取得できる例としては、初期値を入れてない変数
、returnでオブジェクトを返さない関数を実行したときの戻り値がある。

### nullについて
空の値を示す```null```はプリミティブではなく、Objectの状態という定義のため非プリミティブとなる。

### 型の判別法
Javascriptでは型の判別を```typeof```演算時で行うことができる。
以下はjsprimeterからの引用
```javascript
console.log(typeof true);// => "boolean"
console.log(typeof 42); // => "number"
console.log(typeof 9007199254740992n); // => "bigint"
console.log(typeof "JavaScript"); // => "string"
console.log(typeof Symbol("シンボル"));// => "symbol"
console.log(typeof undefined); // => "undefined"
console.log(typeof null); // => "object"
console.log(typeof ["配列"]); // => "object"
console.log(typeof { "key": "value" }); // => "object"
console.log(typeof function() {}); // => "function"
```

## Object
1つの変数に任意の変数を複数記録できる型。
用語として任意の変数名をキー、変数の値をバリューと呼ぶ。

用例1) 任意のキー、バリューで作成したオブジェクトを変数に代入
```typescript
const user = {
  id: 100,
  name: 'テスト太郎',
};
```
このとき```id```、```name```はキー、```100```、```'テスト太郎'```はバリューに分類される。

### オブジェクトの操作について
以下urlが非常にわかりやすく纏まっているため、ご参照いただきたい。
https://jsprimer.net/basic/object/

以下は読んでほしい章タイトル。他の章も余力があれば読むのがオススメ
- オブジェクトを作成する
  - {}はObjectのインスタンスオブジェクト
- プロパティへのアクセス
- [ES2015] オブジェクトと分割代入
- プロパティの追加
  - プロパティの削除
- プロパティの存在を確認する
  - プロパティの存在確認: undefinedとの比較
  > 補足: http通信などでレスポンスに欲しいキーがあるか判別する際に使うので覚えておきましょう。
  - プロパティの存在確認: in演算子を使う
- toStringメソッド
- [コラム] オブジェクトのプロパティ名は文字列化される
- オブジェクトの静的メソッド
  - オブジェクトの列挙
  > 補足: Object.entriesは動的なフォーマットのレスポンスの解析に有効。特にオブジェクトのキーとバリューを配列にできるため、Array.filterやArray.reduceでデータの抽出が行える点が魅力的なので覚えましょう。
- オブジェクトのマージと複製
  - オブジェクトのマージ
  - オブジェクトのspread構文でのマージ
  - オブジェクトの複製