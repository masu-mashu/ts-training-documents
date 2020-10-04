# JavaScript文字列操作

## 参考ページ

- [MDN String](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/String)

## よく使うテクニック

### 文字列の大文字化、小文字化

- [String.prototype.toLowerCase](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/String/toLowerCase)
- [String.prototype.toUpperCase](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/String/toUpperCase)

- **大文字・小文字に変換できないもの(数字、記号)などは変換無し**

```javascript
const str = 'I am 24 years-old. ';
console.log(str.toUpperCase()); // I AM 24 YEARS-OLD.
console.log(str.toLowerCase()); // i am 24 years-old.
```

- そのため、**大文字、小文字は同じものとして文字列を比較する**といった処理の場合に使ったりする

---

### 文字列の特定範囲切り出し

- [String.prototype.slice](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/String/slice)

```javascript
const date = '20201004';

const year = date.slice(0, 4);
const month = date.slice(4, 6);
const day = date.slice(6, 8);

console.log(year, month, day); // 2020 10 04
```

- 固定長の文字列の分解などに使用
  - YYYYMMDDの形式の文字列からYYYY, MM, DDをそれぞれ取り出す
- マイナスの値を指定した場合、`str.length + beginIndex`または、`str.length + endIndex`になる
  - 「末尾の文字を取り出す」といった操作が可能になる
- 第二引数(endIndex)を指定しない場合は末尾まで取り出し
  - 「特定の位置から末尾まで取り出す」といった操作が可能になる

```javascript
const book = 'ba-103';
const category = book.slice(0, 2);
const subCategory = Number(book.slice(-3));
console.log(category, subCategory); // ba 103
```

- ゼロパディング(0埋め)に使用
  - ES6からはpadStartを使った方が良いけど、ES5までは以下のように書いていることが多い

```javascript
const num = 1;
console.log(`000${num}`.slice(-3));
// 001
```

---

### 文字列の検索

- [String.prototype.includes](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/String/includes)
- [String.prototype.indexOf](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/String/indexOf)

- 2つとも、「文字列内に特定の文字列が含まれているかどうか」を検索するメソッド
  - includes：含まれている or いないのboolean値
  - indexOf：含まれている場合はそのindexを、含まれていない場合は-1

```javascript
const sentence = 'I have a dog. dog is cute.';
const word = 'dog';
console.log(sentence.includes(word)); // true
console.log(sentence.indexOf(word));  // 9
```

```javascript
const sentence = 'I have a dog. dog is cute.';
const word = 'cat';
console.log(sentence.includes(word)); // false
console.log(sentence.indexOf(word));  // -1
```

- そのため、特定の文字が含まれているかどうかを調べるためのif文は以下のどちらかで書く必要がある

```javascript
if(sentence.includes(word)){
  // trueの場合
}else{
  // falseの場合
}

if(sentence.indexOf(word) !== -1){
  // trueの場合
}else{
  // falseの場合
}
```

- 個人的には以下のように使い分けている
  - includes: 文字が含まれているかどうかだけ知りたい、位置はどうでもいい
  - indexOf: 文字が含まれているかどうか＋後続でindexを用いた処理が必要
    - 大体indexOfを使うときは上のsliceなどと組み合わせることが多いので
- 「文字列が何個含まれているかどうか」はmatchの方が綺麗にかけるのでそちらを採用
- indexOf系には[lastIndexOf](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/String/lastIndexOf)もある
  - 「複数含まれていること前提＋末尾のものだけ取りたい」以外の時にはあまり出番が無いかなと思う

---

### 文字列の分割

- [String.prototype.split](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/String/split)

- 文字列を指定した区切り文字で分割し、新しい文字列の配列を作る

```javascript
const str = 'The quick brown fox jumps over the lazy dog.';
console.log(str.split(' '));
// ['The', 'quick', 'brown', 'fox', 'jumps', 'over', 'the', 'lazy','dog.']
```

- 空白区切り、ハイフン区切り、スラッシュ区切り…など使いそうな箇所は多い
- 連続する場合は空文字が途中に入る
  - よって区切り文字の連続を無視するためには以下のどちらかをしなければならない
    1. splitした結果から空文字をfilterで除去
    1. 正規表現で予め置換(replace)してからsplit

```javascript
const str = 'test,,,1';
console.log(str.split(','));
// [ 'test', '', '', '1' ]
```

```javascript
const str = 'test,,,1';
console.log(str.split(',').filter(r => r !== '')) // 1. split -> filter
console.log(str.replace(/,+/g, ',').split(','));  // 2. replace -> split
// ['test', '1']
```

- **先頭or末尾にある場合、空文字が追加されるので注意すること**
- ただし、文字列を1文字ずつの配列にしたい場合はArray.fromを使うこと
  - `str.split()`ではサロゲートペアが壊されるので
  - [stack overflow](https://stackoverflow.com/questions/4547609/how-do-you-get-a-string-to-a-character-array-in-javascript/34717402#34717402)

```javascript
const str = 'lemon water';
console.log(Array.from(str));
// ['l', 'e', 'm', 'o', 'n', ' ', 'w', 'a', 't', 'e', 'r']
```

---

### 文字列の結合

- [Array.join](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/join)

- 逆に分割した文字列の配列を特定の文字で結合したい場合はArray.joinを使う

```javascript
const arr = ['this', 'is', 'a', 'pen'];
console.log(arr.join(' '));
// this is a pen
```

- 引数に指定が無い場合はカンマ区切り
  - 空白も何も付けずに連結したい場合は**空文字**を入れる

---

### 文字列の端の空白削除

- [String.prototype.trim](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/String/trim)
- [String.prototype.trimEnd](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/String/trimEnd)
- [String.prototype.trimStart](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/String/trimStart)

- 文字列の末尾または先頭にある空白を削除することができる
- 例えば、「フォームで入力した文字列の前と後ろの空白だけ削除する」などで使う
- **スペース、タブ(`\t`)、改行(`\r`, `\n`)などをまとめて削除できる**

```javascript
const greeting = '\t\t   Hello world! \n\t ';
console.log(greeting.trim()); // "Hello world!"
```

- 通常、これらの削除はreplaceを使って記述することが多い気がする
