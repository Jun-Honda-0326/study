# Slickについてまとめます

## DBIOActionについて
DBIOAciton classとDBIOオブジェクトがあり、テーブルを作成したり、データを挿入するといったDBに対し実行する処理は全てDBIOActionのインスタンスになる。<br>

## CRUD処理
<br>

### 取得
`result`でアクションを取得する。単に`.result`だけなら全件取得になる。<br>
データを一件取得したい場合などは`id`を`.filter`引数にし条件で絞る必要がある.<br>
```scala
使い方

val action1: DBIO[Seq[UsersRow]] = Users.result //全件取得
val action3: DBIO[Option[UsersRow]] = Users.filter(_.id === id).result.headOption //一件取得
```
### 登録
- +=... 1件登録する.
- ++=... 複数件登録する
- forceInsertQuery... select-insert
- insertOrupdate... primary keyが存在しない場合は登録、存在しない場合は更新する
```scala
使い方

val action1: DBIO[Int] = Users += UsersRow(0, "なまえ")
val action2: DBIO[Option[Int]] = Users ++= Seq(
  UsersRow(0, "なまえ1"),
  UsersRow(0, "なまえ2")
)
val action3: DBIO[Int] = Users forceInsertQuery SubUsers.filter(_.name === name)
val action4: DBIO[Int] = Users insertOrUpdate UsersRow(10, "なまえ")
```

AutoIncの値は`returning`で取得することができる
```scala
// AutoIncの値のみ 単にDBに挿入する場合
val action1: DBIO[Long] = Users returning Users.map(_.id) += UsersRow(0, "なまえ")
// ケースクラスに詰めて返す
val action2: DBIO[UsersRow] = Users returning Users.map(_.id) into ((row, id) => row.copy(id = id)) += UsersRow(0, "なまえ")
```
## 更新
- update... クエリに該当するレコードを削除する
```scala
val action1: DBIO[Int] = Users.filter(_.id === id).update(UsersRow(1, "なまえ変更"))
// 特定のカラムだけ更新
val action2: DBIO[Int] = Users.map(t => t.name -> t.updDate).update("なまえ変更" -> new java.util.Date)
```
## 削除
- delete... クエリに該当するレコードを削除する
```scala
val action: DBIO[Int] = Users.filter(_.id === id).delete
```

## filter(条件)
 =
 ```scala
Users.filter(_.id === id)
 ```
 <>(≠)
 ```scala
Users.filter(_.id =!= id)
 ```
 is Null
 ```scala
 User.filter(_.companyId.isEmpty)
 ```
 is Not Null
 ```scala
 Users.filter(_.companyId.nonEmpty)
 ```
 IN
 ```scala
 DBの中からidが1,2の物を取り出す
 User.filter(_.id inSet Seq(1,2))
```
like
```scala
検索機能で使う
前方一致の場合
Users.filter(_.name startwWith "Biz")
後方一致の場合
Users.filter(_.name endsWith "Biz")

あいまい検索の場合
Users.filter(_.name like "%Biz%")
変数を受け取って絞り込むためには以下のように記述する
Users.filter(_.name like s"%${keyword}%")
```
AND (&&)
```scala
Users.filter(t => (t.name === name) && (t.companyId === companyId))
```
OR (||)
```scala
Users.filter(t => (t.name === name)) || (t.companyId === companyId)
```
