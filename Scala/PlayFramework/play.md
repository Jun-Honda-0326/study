# PlayFrameworkについてまとめます

## JSON形式のデータの型変換
- JsValue
  - JsString
  - JsString
  - JsNumber
  - JsBoolean
  - JsObject
  - JsArray
  - JsNull

### JSONを使用したクラス構築
```scala
import play.api.libs.json._

val json: JsValue = JsObject(
  Seq(
    "name"     -> JsString("Watership Down"),
    "location" -> JsObject(Seq("lat" -> JsNumber(51.235685), "long" -> JsNumber(-1.309197))),
    "residents" -> JsArray(
      IndexedSeq(
        JsObject(
          Seq(
            "name" -> JsString("Fiver"),
            "age"  -> JsNumber(4),
            "role" -> JsNull
          )
        ),
        JsObject(
          Seq(
            "name" -> JsString("Bigwig"),
            "age"  -> JsNumber(6),
            "role" -> JsString("Owsla")
          )
        )
      )
    )
  )
)
```
上記コードはJson.objを使用して以下のように書き換えることができる
```scala
import play.api.libs.json.JsNull
import play.api.libs.json.Json
import play.api.libs.json.JsString
import play.api.libs.json.JsValue

val json: JsValue = Json.obj(
  "name"     -> "Watership Down",
  "location" -> Json.obj("lat" -> 51.235685, "long" -> -1.309197),
  "residents" -> Json.arr(
    Json.obj(
      "name" -> "Fiver",
      "age"  -> 4,
      "role" -> JsNull
    ),
    Json.obj(
      "name" -> "Bigwig",
      "age"  -> 6,
      "role" -> "Owsla"
    )
  )
```
