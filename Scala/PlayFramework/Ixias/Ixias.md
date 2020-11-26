# [Ixias](https://github.com/ixias-net/ixias)
# 概要
Ixiasとはプロダクトの作り方を標準化、ビジネスロジックの標準化を提供するためのライブラリ。
# 開発経緯
サービスによって技術の選択水準が変わり、俗人化が生まれるとプロダクト間の人材ローテーションがしづらくなる。
人材ローテーションが容易にできるようになれば、エンジニアの成長に繋がる。
また、オフショア開発でも品筆が保てるように標準化ライブラリが開発された。

# Ixiasの導入
Ixiasはbuild.sbtに以下の記述をすることで導入できる。
```scala
scalaVersion := "2.12.11"
resolvers ++= Seq(
  "IxiaS Releases" at "http://maven.ixias.net.s3-ap-northeast-1.amazonaws.com/releases"
)
libraryDependencies ++= Seq(
  "net.ixias" %% "ixias"      % "1.1.25",
  "net.ixias" %% "ixias-aws"  % "1.1.25",
  "net.ixias" %% "ixias-play" % "1.1.25",
  "mysql" % "mysql-connector-java" % "5.1.+",
  "ch.qos.logback" % "logback-classic" % "1.1.+"
)

※Scalaのバージョンは2.12〜でないと依存ライブラリの関係でうまく使えないことがある。
```
# Ixias.modelパッケージについて
ixias-coreライブラリ内に実装されており、DDDにおけるエンティティに対応する枠組みを実装しており、Ixiasの核となる実装をしている。
```scala
サンプルコード

package lib.model

import ixias.model._
import java.time.LocalDateTime

// ケースクラス
import Todo._
case class User(
  id: Option[Id],
  categoryId: Categry.Id
  title: String,
  content: String,
  updatedAt: LocalDateTime = NOW,
  createdAt: LocalDateTime = NOW
) extends EntityModel[Id]

// コンパニオンオブジェクト
object Todo {
  val Id = the[Identity[Id]]
  type Id = Long @@ Todo //Long型の値を扱うIdという異なる名前の型を持っている

  def build(name: String): Todo#WithNoId =
    new User(
      id = None,
      categoryId = categryId
      title = title,
      content = content
    ).toWithNoId
}
WithNoIdはidを持たないEntity
```
## EntityModelクラスについて
EntituModelクラスではid、updatedAt、createdAtをを定義しており、これらはEntityModelトレイトに抽象値として定義されているので、
命名と型は定義通りにしないとコンパイル時にエラーが出る。<br>
  [定義元](https://github.com/ixias-net/ixias/blob/develop/framework/ixias-core/src/main/scala/ixias/model/EntityModel.scala)<br>
  idはOption[Id]型を持つ。<br>
  updatedAtとcreatedAtはjava.time.LocalDateTime(要import)を持つ。<br>
  これら２つには[ixias.model.package.scala](https://github.com/ixias-net/ixias/blob/develop/framework/ixias-core/src/main/scala/ixias/model/package.scala)内に定義されているNOWメソッドをデフォルト値として与えている

## Idの定義について
コンパニオンオブジェクトではIdの型を定義している。
Idの型はshapeless.tagg.@@を用いて実装される。<br>
例えば
```scala
Long @@ Todo
```
という記述はLong型にTodoというタグがついている。<br>
これにより、同一性を持つ識別子として取り扱うことができる(より型安全になる！！)<br>

@@とshapelessの変換は[package.scala](https://github.com/ixias-net/ixias/blob/develop/framework/ixias-core/src/main/scala/ixias/model/package.scala)で実装されている。<br>

上記の実装ではCatedyIdがCategory.Idとっており、これににより、Long型のidであってもCategory型のid(CategoryのタグがついたId)しかDBに格納することができないので、より型安全な実装をすることができる。

ixias.EntityModelクラスには、id,updatedAt, createdAtの3つの値を定義している。
それぞれ以下のように定義されている。

```scala
val id: Option[Id]
val updatedAt: LocalDateTime
val createdAt: LocalDateTime
このうち、updatedAt,createdAtには、ixias.model.package内に定義されているNOWメソッドをデフォルトの値として与える。
```
## WithNoIdとEmbeddedId型について
EnitityModelトレイトにはWithNoId型とEmbeddedId型が定義されている[定義元](https://github.com/ixias-net/ixias/blob/develop/framework/ixias-core/src/main/scala/ixias/model/Entity.scala)<br>
WithNoId型はidを持たないEntity、EmbeddedId型はidを持たないEntityという意味になる。


## WithNoIdとEmbeddedIdを用いた処理
### 永続ストレージとの連携
永続ストレージとの連携を行うRepository層はSlickRepositoryを継承している。
このSlickRepositoryは下記画像のように複数のトレイトを継承しており、継承しているトレイトの機能が使用可能になる。（下記参考画像参照）
<br><br><br>
![参考画像](https://miro.medium.com/max/700/1*5GLmdFmzX9wZbCJWbAL3QQ.png)

永続ストレージとの連携を行うReposotoryオブジェクトへ継承
[ixias.persistence.dbio.EntityOAciton](https://github.com/ixias-net/ixias/blob/develop/framework/ixias-core/src/main/scala/ixias/persistence/dbio/EntityIOAction.scala)トレイトにはCRUD処理を行うための抽象メソッドが定義されている
```scala
def add(entity: EntityWithNoId): Future[Id] //レコードの追加
def get(id: Id): Future[Option[EntityEmbeddedId]] //レコードの取得
def update(entity: EntityEmbeddedId): Future[Option[EntityEmbeddedId]] //レコードの更新
def remove(id: Id): Future[Option[EntityEmbeddedId]] //レコードの削除
```
EntityIOActionを継承することにより、CRUD処理等のメソッドの命名や引数と返り値の型を統一することができる、プロダクトのコードを標準化することができる。

## EntityEmbeddedId型に包まれたデータの取得について
Entity型に包まれている値は`todo.v.title`のように続けて入力することで取得できる

```scala
def id(implicit ev: S =:= IdStatus.Exists): K = v.id.get
```


# EnumStatusについて
Enum型は[ixias.util.Enum](https://github.com/ixias-net/ixias/blob/develop/framework/ixias-core/src/main/scala/ixias/util/Enum.scala)を参照
```scala
lib.modelの実装例
 sealed abstract class TodoStatus(val code: Short, val name: String)extends EnumStatus
   object TodoStatus extends EnumStatus.Of[TodoStatus] {
     case object Notyet extends TodoStatus(0, "未着手")
     case object Active extends TodoStatus(1, "進行中")
     case object Finished extends TodoStatus(2, "完了")
   }

ControllerでFormの実装例
state: TodoStatus = TodoRepository.apply(state: Short) //applyメソッドを使用することにより、Enumの変換が可能
```


