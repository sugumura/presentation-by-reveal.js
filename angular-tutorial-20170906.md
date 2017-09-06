# オトナのフロントエンド入門<br>Angular編@未来会議室
2017年9月6日

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

おかげ様で1周年  
イベントのべ30回開催

---

# <img src="image/20170906/angular.png" width="62" height="62" style="margin: 0 10px 0; border: none;">Angular

- OSSのWebフロントエンドフレームワーク
- コンポーネント指向
- Typescript(JavascriptのaltJS)
- 現在バージョン4、9月に5がリリース予定
- バージョン1からの互換性はなし

---

# Typescriptとは

- Javascriptを拡張したOSS
- 静的型付けが可能
- TypescriptからJavascriptを生成して読み込む
- AngularではTypescriptを推奨

---

# Tyepscriptを使ってみよう

テスト用のフォルダを作ります

```
$ npm install -g typescript
$ mkdir test && cd test
$ touch person.ts
```

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

```console
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

# トランスパイル

TypescriptからJavascriptに変換

```console
$ tsc student.ts
# student.jsが生成される

$ node student.js
1: Taro Yamada
```

---

# ハンズオン資料URL

Tutorialをやっていきます

[https://github.com/ng-japan/hands-on](https://github.com/ng-japan/hands-on)

```
# cloud9でのサーバ起動は下記を利用
$ ng serve --host 0.0.0.0 --public-host $C9_HOSTNAME
```

---

# 以下、Cloud9でハンズオン

---

# 資料

- [公式ドキュメント](https://angular.io/docs)
- [Angular Japan User Group](https://ngjapan.blogspot.jp)
- [Angular アプリケーションプログラミング](https://gihyo.jp/dp/ebook/2017/978-4-7741-9192-8)


---

# おつかれさまでした🍵
