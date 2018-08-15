# オトナのAndroid入門 in Kotlin@未来会議室
2018年8月15日

オトナのプログラミング勉強会<br>
協力 **未来会議室**

[connpass](https://otona.connpass.com/event/97668/)<!--- .element target="_blank" -->

---

# 自己紹介

- 村上　卓
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

# Android

- モバイル向けのオペレーティングシステム
- Android TVやAndroid Auto(車向け)、Wear OSなどの派生
- 2005年にGoogleが開発していたAndroid社を買収
- Apache License 2.0でソースコード公開
- スマホに限らず広く利用されている

[Androidストーリー](https://www.android.com/intl/ja_jp/history/)<!--- .element target="_blank" -->

---

# Kotlin

発音は「コトリン」

- IDEで有名なJetBrains社が中心となって開発
- Apache License 2.0でソースコード公開
- Java互換のJVM言語
- 簡潔な記述とNULL安全など多数の機能を利用できるように
- 2017年5月にAndroid開発の公式言語に

---

# Kotlinを書こう

Webでお試し

[https://try.kotlinlang.org/](https://try.kotlinlang.org/)<!--- .element target="_blank" -->

---

# 利用する構文

末尾のセミコロンは必要ありません

関数

```kotlin
fun sum(a: Int, b: Int): Int {
  return a + b
}
```

変数

```kotlin
// 1度だけ代入できる（読み取り専用）ローカル変数
val a: Int = 1
val b = 1   // `Int`型が推論される
val c: Int  // 初期値が与えられない場合、型指定が必要
c = 1 
// 変更可能 (Mutable) な変数
var x = 5 // `Int`型が推論される
x += 1
```

---

# println

`System.out` パッケージは不要

```kotlin
fun main(args: Array<String>) {
  val message = "Hello, world!"
  println(message)
}
```

---

# ハンズオン

Android開発を始めよう

[Build Your First Android App in Kotlin ](https://codelabs.developers.google.com/codelabs/build-your-first-android-app-kotlin/index.html)<!--- .element target="_blank" -->

---

# 今後の学習

書籍はたくさんあるので書店で確認されるのをおすすめします

- [リファレンス](https://dogwood008.github.io/kotlin-web-site-ja/docs/reference/)<!--- .element target="_blank" -->
    - リファレンスの日本語翻訳
- [Samples](https://developer.android.com/samples/)<!--- .element target="_blank" -->
    - 公式のサンプル集。作りたいものがある場合探すと参考になる

---

# おつかれさまでした🍵
