# 第二回<br>オトナのフロントエンド入門<br>Angular編
2017年10月4日

オトナのプログラミング勉強会<br>
協力 **未来会議室**

---

# 自己紹介

- 村上　卓
- 30歳
- フリーランス
- Angular/Ruby On Rails

Angularまだまだ勉強中

---

# この勉強会について

オトナのプログラミング勉強会<br>
[http://otona.pro](http://otona.pro)

- 2016年8月から開始
- 月2回（第1水曜、第3水曜）
- いつでも講師募集中
    - プログラム言語、機械学習、Web系...
- YouTubeで振り返り可

---

# 前回の復習

---

# <img src="image/20170906/angular.png" width="62" height="62" style="margin: 0 10px 0; border: none;">Angular

- OSSのWebフロントエンドフレームワーク
- コンポーネント指向
- Typescript(JavascriptのaltJS)
- 現在バージョン4、9月に5のRCでました
- バージョン1からの互換性はなし

---

# Typescriptとは

- Javascriptを拡張したOSS
- 静的型付けが可能
- TypescriptからJavascriptを生成して読み込む
- AngularではTypescriptを推奨

---

# Interface

Interfaceを宣言することでオブジェクトのプロパティの型を指定できます

```ts
interface Person {
    firstName: string;
    lastName: string;
}

function greeter(person: Person) {
    console.log(`Hello ${person.firstName} ${person.lastName}!!`);
}

let p = {firstName: 'Taro', lastName: 'Yamada'};
greeter(p);
```

---

# トランスパイル

TypescriptからJavascriptに変換

```
$ tsc person.ts
# person.jsが生成される

$ node person.js
Hello Taro Yamada!!
```

---

# クラス

student.tsを作成

```ts
class Student {
    fullName: string;
    constructor(
        public grade: number,
        public firstName: string,
        public lastName: string) {
            this.fullName = `${firstName} ${lastName}`;
        }        
    toString() {
        return `${this.grade}: ${this.fullName}`;
    }
}
let s = new Student(1, 'Taro', 'Yamada');
console.log('' + s);
```

---

# Cloud9で<br>前回のソース確認

---

# ハンズオン資料URL

ハンズオンをやっていきます

[https://github.com/sugumura/hands-on](https://github.com/sugumura/hands-on)

```
# cloud9でのサーバ起動は下記を利用
$ ng serve --host 0.0.0.0 --public-host $C9_HOSTNAME
```

---

# 前回の質問

---

# デコレータについて

[Decorators](https://www.typescriptlang.org/docs/handbook/decorators.html)
[サンプル](https://embed.plnkr.co/AGxS7ULtbVB43ce2n3Xg/)

---

# Cloud9でハンズオン

---

# Dependency Injection<br>(依存性の注入)

例

```
class Computer {
    cpu: CPU = new IntelCoreI7();
    storage: Storage = new ToshibaSSD();
}
```

---

# 依存の注入

```
class Computer {
    cpu: CPU;
    storage: Storage;

    constructor(_cpu: CPU, _storage: Storage) {
        this.cpu = _cpu;
        this.storage = _storage;
    }
}
```

---

# DIの利点

- 完全な機能がなくてもモックからの開発が可能
- 依存性を切り離すことで実装範囲の明確化

---

# AngularのDI(1)

HeroServiceの注入

```
// 関係ないものは省略
@Component({
    providers: [HeroService],
})
export class AppComponent {
    constructor(_heroService: HeroService) { }
}

```

---

# AngularのDI(2)

前のコードはシンタックスシュガー

```
// 関係ないものは省略
@Component({
    providers: [
        new Provider(HeroService, {useClass: HeroService})
    ],
})
export class AppComponent {
    constructor(@Inject(HeroService) _heroService: HeroService) { }
}
```

useClass, useValue, useFactoryが指定可能

---

# 資料

- [公式ドキュメント](https://angular.io/docs)
- [Angular Japan User Group](https://ngjapan.blogspot.jp)
- [Angular アプリケーションプログラミング](https://gihyo.jp/dp/ebook/2017/978-4-7741-9192-8)


---

# おつかれさまでした🍵
