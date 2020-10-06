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
