# Scalaについてまとめていきます

- sealedキーワード<br>
クラス、トレイトの先頭に`sealed`をつけることによって同一ファイル内でしか継承できないようにする。プログラムの保守性の観点から。

- lazy<br>
lazyとは英語で怠け者を指しており、計算結果を遅らせる効果がある。<br>
大きな計算やクエリ処理を行う場合で、その処理によって得られた値が毎回、頻繁には使われない時。<br>
本当に必要な時だけ実行したいものの場合に使用する<br>
```scala
class Sum(x: Int, y: Int){
  (lazy) val sum = {
    println("lazy")
    x + y
  }
}
lazyがない場合
val s = new Sum(1,2)
//lazy
インスタンス生成時にフィールド呼び出されている。
s.sum
// 3

lazyありの場合
val l = new Sum(1,2)
s.sum
// lazy
//3
インスタンス実行時にのみフィールドが呼び出されているのがわかる
```
[参考Qita](https://qiita.com/kounorimich/items/a9da08847ce8c0ea8380)
