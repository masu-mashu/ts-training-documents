# RxJS

## RxJS とは

- 追加予定

## Observable の生成

### of

- 受け取った値をそのまま流す

```ts
import { of } from 'rxjs';

of(1).subscribe((data) => console.log(data));
// Output // 1
of(1, 2, 3).subscribe((data) => console.log(data));
// Output
// 1
// 2
// 3
of([1, 2, 3]).subscribe((data) => console.log(data));
// Output
// [1, 2, 3]
```

### from

- 要素をストリームに順次流す

```ts
import { from } from 'rxjs';

from([1, 2, 3]).subscribe((data) => console.log(data));
// Output
// 1
// 2
// 3
```

### fromEvent

- イベントをストリームに変換する

```ts
import { fromEvent } from 'rxjs';

const button = document.getElementById('button');
fromEvent(button, 'click').subscribe((_) => {
  console.log('Button clicked!');
});
```

### interval

- 一定時間ごとにデータを流すストリームを生成する
- take は回数を制限する

```ts
import { interval } from 'rxjs';
import { take } from 'rxjs/operators';

interval(1000)
  .pipe(take(5))
  .subscribe(
    (t) => console.log(t),
    (_) => _,
    () => console.log('5 seconds left!!')
  );
// Output
// 0
// 1
// 2
// 3
// 4
// 5 seconds left!!
```

### timer

- 指定時間経過したらデータを流し始めるストリームを生成する

```ts
import { timer } from 'rxjs';

timer(5000).subscribe(
  (t) => console.log(t),
  (_) => _,
  () => console.log('5 seconds left!!')
);
// Output
// 0
// 5 seconds left!!
```

- 第一引数は「最初のデータが流すまでの時間」
- 第二引数は「データを流す間隔」

```ts
timer(1000, 500).subscribe(
  (t) => console.log(t),
  (_) => _,
  () => console.log('complete')
);
// Output
// 0
// 1
// 2
// ...
```

### marge

- 複数のストリームを合成する
- 複数の Observable を引数に取り、1 本のストリームに変換します。生成されたストリームでは merge に渡された順に関係なく、next が発火したものから順にデータが流れます。

```ts
import { take } from 'rxjs/operators';
import { timer, merge, interval } from 'rxjs';

const timer5 = timer(5000);
const interval1 = interval(1000).pipe(take(5));
merge(interval1, timer5).subscribe(
  (t) => console.log(t),
  (_) => _,
  () => console.log('completed!!')
);
// Output
// 0
// 1
// 2
// 3
// 0
// 4
// completed!!
```

### zip

- 複数のストリームを合成する
- 複数の Observable を引数に取り、1 本のストリームに変換します。merge と異なり、それぞれのデータを配列にまとめて次のオペレータに流します。

```ts
import { of, zip } from 'rxjs';

const a = of(1);
const b = of(2);
zip(a, b).subscribe((data) => console.log(data));
// Output // [1, 2]
```

- Observable が流すデータの個数がそれぞれ異なる場合は、余剰分はデータが破棄されます。

```ts
import { take, map } from 'rxjs/operators';
import { timer, merge, interval, zip } from 'rxjs';

const interval1 = interval(1000).pipe(
  take(3),
  map((n) => `obs1: ${n}`)
);
const interval2 = interval(1000).pipe(
  take(5),
  map((n) => `obs2: ${n}`)
);
const interval3 = interval(1000).pipe(
  take(2),
  map((n) => `obs3: ${n}`)
);

zip(interval1, interval2, interval3).subscribe(
  (data) => console.log(data),
  (error) => console.log(error),
  () => console.log('complete')
);
// Output
// (1 秒待ち)
// obs1: 0, obs2: 0, obs3: 0 // (1 秒待ち)
// obs1: 1, obs2: 1, obs3: 1 // complete
```

### forkJoin

- すべてのストリームが完了したらデータを流す

```ts
import { of, timer, forkJoin } from 'rxjs';

forkJoin([of(1), of(1, 2), timer(1000)]).subscribe(
  (data) => console.log(data),
  (error) => console.log(error),
  () => console.log('complete')
);
// Output
// (1 秒待ち)
// [1, 2, 0]
// complete
```

- 1 件でもエラーが発生するとストリーム全体がエラーになります。

```ts
import { of, timer, forkJoin, throwError } from 'rxjs';

forkJoin([
  of(1),
  of(1, 2),
  timer(1000),
  throwError(new Error('error')),
]).subscribe(
  (data) => console.log(data),
  (error) => console.log(error),
  () => console.log('complete')
);
// Output
// Error: error
```

### race

- 最初にデータが流れたストリームに絞る

```ts
import { timer, race } from 'rxjs';

race(timer(1000), timer(2000), timer(3000)).subscribe(
  (t) => console.log(t),
  (error) => console.log(error),
  () => console.log('complete')
);
// Output
// (1 秒待ち)
// 0
// complete
```

### range

- 指定範囲の値を流すストリームを生成する

```ts
import { range } from 'rxjs';

range(1, 5).subscribe((n) => console.log(n));
// Output
// 1
// 2
// 3
// 4
// 5
```

### throw

- エラーを流すストリームを生成する

```ts
import { throwError } from 'rxjs';

throwError(new Error('sample error!')).subscribe(
  (data) => console.log(`next: ${data}`),
  (error) => console.log(`error: ${error}`)
);
// Output
// Error: sample error!
```

## データの加工

### map

- データを加工

```ts
import { of } from 'rxjs';
import { map } from 'rxjs/operators';

of(1, 2, 3)
  .pipe(map((n) => n * n))
  .subscribe((n) => console.log(n));
// Output
// 1
// 4
// 9
```

### mapTo

- 固定値への変換

```ts
import { of } from 'rxjs';
import { mapTo } from 'rxjs/operators';

of(1, 2, 3)
  .pipe(mapTo(1))
  .subscribe((data) => console.log(data));
// Output
// 1
// 1
// 1
```

### filter

- データの選別

```ts
import { range } from 'rxjs';
import { filter } from 'rxjs/operators';

range(1, 10)
  .pipe(filter((n) => n % 2 === 0))
  .subscribe((n) => console.log(n));
// Output
// 2
// 4
// 6
// 8
// 10
```

### reduce

- データの集計

```ts
import { range } from 'rxjs';
import { reduce } from 'rxjs/operators';

range(1, 5)
  .pipe(reduce((acc, value) => acc + value, 0))
  .subscribe((n) => console.log(n));
// Output
// 15
```

### scan

- データの集計
- reduce と似ていますが、データが流れるたびにオペレータに流す点が異なります

```ts
import { range } from 'rxjs';
import { scan } from 'rxjs/operators';

range(1, 5)
  .pipe(scan((acc, value) => acc + value, 0))
  .subscribe((n) => console.log(n));
// Output
// 1
// 3
// 6
// 10
// 15
```

### distinct

- 重複データの除去

```ts
import { from } from 'rxjs';
import { distinct } from 'rxjs/operators';

from([1, 2, 3, 1, 1, 2])
  .pipe(distinct())
  .subscribe((n) => console.log(n));
// Output
// 1
// 2
// 3
```

### distinctUntilChanged

- 値の変化を抽出

```ts
import { from } from 'rxjs';
import { distinctUntilChanged } from 'rxjs/operators';

from([1, 1, 2, 3, 3, 1, 1])
  .pipe(distinctUntilChanged())
  .subscribe((n) => console.log(n));
```
