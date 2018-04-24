# RxJSで始めるReactive Extensions
2018年5月9日

オトナのプログラミング勉強会<br>
協力 **未来会議室**

[connpass](https://otona.connpass.com/event/83010/)<!--- .element target="_blank" -->

---

# 自己紹介

- 村上　卓
- 30歳
- フリーランス
- Angular/Ruby On Rails

---

オトナのプログラミング勉強会<br>
[http://otona.pro](http://otona.pro)<!--- .element target="_blank" -->

- 2016年8月から開始
- 月2回（第1水曜、第3水曜）
- いつでも講師募集中
    - プログラム言語、機械学習、Web系...
- [YouTube Live](https://www.youtube.com/channel/UCrXf76sF5RUKcGpMpZASqow)<!--- .element target="_blank" -->でリモート参加も振り返りも可(?)

---

# RxJS

![rxjs-logo](../../image/20180509/rxjs-logo.png)

---

# RxJSとは

- Reactive Extensions for JavaScript
- 元々はC#向けライブラリとして設計・開発
- Functional Reactive Programing(FRP)を行うために開発したRxの移植
- 他の言語でもRxNな感じである
    - Rx.NET, RxJava, RxSwift, RxRuby, RxPy...

---

# 考え方

![everything-is-a-stream.jpg](../../image/20180509/everything-is-a-stream.jpg)

---

# Reactive Extensions

- 全てはストリームという考え方
- 時間と共に変化するものを扱うための設計
    - 例)DOMイベントやHTTPリクエスト等
- RxはObserverパターンがベース

---

# 実装比較

単純な例  
(ObservableはObserverパターンでいうSubject)

```js
// None RxJS
for (let i=i; i<=100; i++) {
    console.log(i);
}
```

```js
// Use RxJS
import { Observable } from ’rxjs’
Observable.range(1, 100)
    .subscribe(v => console.log(v));
```

---

# 注意点

ネットのRxJSサンプルで`Rx.xxx`表記がありますが、パッケージインポートによる違いです

```js
// 全てをインポート
import * as Rx from ’rxjs’
Rx.Observable.range(1, 100)
    .subscribe(v => console.log(v));
```

```js
// 個別インポート
import { Observable } from ’rxjs’
Rx.Observable.range(1, 100)
    .subscribe(v => console.log(v));
```

---

# もうちょっと実用的な例

動かしたい人は[ここから](https://stackblitz.com/edit/rxjs-mouse-sample1)エディタへ

```js
import { fromEvent } from 'rxjs/observable/fromEvent';

fromEvent(document, 'mousemove')
  .subscribe((e) => console.log(e.clientX, e.clientY));
```

---

# debounce

マウスの動きが1秒止まった場合に発火

```js
import { fromEvent } from 'rxjs/observable/fromEvent';
import { interval } from 'rxjs/observable/interval';
import { debounce } from 'rxjs/operators';

fromEvent(document, 'mousemove')
  .pipe(
    debounce(() => interval(1000)),
  )  
  .subscribe((e) => console.log(new Date(), e.clientX, e.clientY));

```

---

# filter

特定の座標以下にフィルター

```js
import { fromEvent } from 'rxjs/observable/fromEvent';
import { interval } from 'rxjs/observable/interval';
import { debounce, filter, tap } from 'rxjs/operators';

fromEvent(document, 'mousemove')
  .pipe(
    debounce(() => interval(1000)),
    tap(() => console.log('fire')),
    filter(e => e.clientX > 100),
  )  
  .subscribe((e) => console.log(new Date(), e.clientX, e.clientY));
```

---

# オペレータ

debounceやfilterをオペレータという  
オペレータを組み合わせて処理を記述していく  
RxJS 5.5から自作オペレータも使えるように

[ReactiveX - Operators](http://reactivex.io/documentation/operators.html)

---

# Observable.subscribe

Observableは書いただけでは実行されず、subscribeすることで値が流れる  
Observableが各イベントをコールバックする

```js
subscribe(
    (x) => console.log(x),              // onNext
    (error) => console.error(error),    // onError
    () => console.log('complted');      // onComplete
)
```

---

# コールバック

`error`が呼ばれると`next`と`complete`は呼ばれない  
`complete`が呼ばれると`next`と`error`は呼ばれない

```js
import { interval } from 'rxjs/observable/interval';

const subscription = interval(1000).subscribe(    
    (data) => console.log(data),
    (error) => console.error(error),
    () => console.log('complete')
);

// 5秒後に完了
setTimeout(() => subscription.complete(), 5000)
// => 0, 1, 2, 3, complete
```

---

# N Observer

いままでは1対1のストリーム処理  
複数のObserverに対して配信する場合は`Subject`を使用

[サンプル](https://stackblitz.com/edit/rxjs-subject-sample)

---

# Subject

```js
import { Subject } from 'rxjs';

const subject = new Subject();
console.log('first subscribe');
subject.subscribe(
  (val) => console.log('subscribe 1', val),
  () => {},
  () => { console.log('complete 1') }
);
console.log('second subscribe');
subject.subscribe(
  (val) => console.log('subscribe 2', val),
  () => {},
  () => { console.log('complete 2') }
);
subject.next(1);
subject.next(2);
subject.complete();
```

---

# 基本的な知識はここまで

他にRxではHot/Coldといった特性や、Schedulerが学習要素としてあります  
発展的な内容ですのでぜひ学習を進めてみてください

(今回扱ったものはすべてCold特性)

最後にJavascriptを利用する場合に頻出するであろうPromiseの扱いを取り上げます

---

# Promise

`$.ajax`や`fetch`といったPromiseを扱うオブジェクトはJavascriptで近年増加
fromPromiseを使うことでRxに組み込み可能

[サンプル](https://stackblitz.com/edit/rxjs-sample-promise)

```js
import { fromPromise } from 'rxjs/observable/fromPromise';
import { mergeMap } from 'rxjs/operators';

fromPromise(fetch('https://dog.ceo/api/breeds/image/random'))
  .pipe(
    mergeMap((response) => response.json())
  )
  .subscribe(
    (data) => console.log(data)
  );
```

---

# toPromise

ストリームをPromiseに変換するには `toPromise` を使用
`complete`時の値を流す

```js
import { of } from 'rxjs/observable/of';
import { concat } from 'rxjs/operators';

const of1$ = of(1, 2, 3);
const of2$ = of(4, 5, 6);

of1$.pipe(
  concat(of2$),
)
.toPromise()
.then((data) => console.log(data));
// => 6

```

---

# 今後の学習

- [ReactiveX](http://reactivex.io/)
    - 本家
    - 各オペレータには図が用意されてるので詰まった場合は参照
- [Learn RxJS](https://www.learnrxjs.io/)
    - オペレータが解説されてあり動かせるサンプル尽き
- [RxのHotとColdについて](https://qiita.com/toRisouP/items/f6088963037bfda658d3)
    - Hot/Coldに関してとてもわかりやすい日本語記事

---

# おつかれさまでした🍵
