# TypeScriptについてまとめます

型の互換性
- 抽象度の高いものに具体的な物を代入するの問題ないが、より具体的なものに抽象的なものは代入できない
- 引数の型が違う関数は代入できない

型の決まり方
- コンパイラによる型推論
- 型注釈による型付け...アノテーション

変数の定義
```js
const //再代入できない
let  //再代入できる
```

TypeScriptのデータ型
```js
string
number
boolean
```
オブジェクトの定義の仕方
```js
interface NAME{
  first: stging;
  last: string | null;   // | nullでnullを許可することできる
}

let nameObj: NAME = {first: "Yamada", last: "Tarou"};
```
関数の定義の仕方
```js
const func = (x: number, y: number):number => {
  return x + y;
}

引数の型はアノテーションで指定する必要がある
```

Intersection Types
```js
type PROFILE = {
  age: number;
  city: string;
}
type LOGIN = {
  username: string;
  password: stiring;
}

type USER = PROFILE & LOGIN; //型の結合
//結合した型から新たなオブジェクトを作る場合

const USERA: USER = {
  age: 30,
  city: "Tokyo",
  username: "yamada",
  password: "xxx"
}
```
Union Types 変数が受け取れるデータ型を制限できる
```js
let value: boolean | number
value = true;  ○
value = "hello"  ✖︎

let arrayUni: {number | string}[];
array1 = [0,1,2,"hello"]; ○
array1 = [0,1,2,"hello", true]; ✖︎

let company: "Facebook" | "Google" | "Amazon"
company = "Google" ○
company = "Apple" ✖︎
//文字列リテラルに対してUnion Typesを適用している
```
typeof  宣言済の変数の型を取得する
```ts
let msg: string = "Hello";
let msg2: typeof msg;
let msg2 = "こんにちは"

let animal = {cat: "small cat"}; //cat: string
let newAnimal: typeod animal = {cat: "big cat"};

//JSON型のオブジェクトの中に複雑な属性を持っていたとしてもtypeofを使うと丸ごと継承してくれる
```
keyof
```ts 属性の名前をunion typeで取得する
type KEYS = {
  primary: string;
  secondary: string;
};
let key: keyof KEYS
key = "primary" ○

// typeof + keyof
const SPORTS = {
  soccer: "Soccer",
  baseball "Baseball",
};

let keySports: keyof typof SPORTS;
keySports = "soccer"; or "baseball";
//keySportsの中には"Soccer" or "basevball"しか受け付けない
```
enum 列挙型
```ts
enum OS {
  Windows,  //0
  Mac,  //1
  Linux  //2
}
//自動で連番を振ってくれる
(ex)
interface PC {
  id: number;
  OSType: OS;
}

const PC1: PC = {
  id: 1,
  OSType: OS.Windows, //0
}

//ソフトウェアのバグに強くなりメンテナンス性が向上する
```
Generics ジェネリクス
```ts
interface GEN<T>{
  item: T
}
//デフォルトの型も指定することができる

const gen0: GEN<string> = { item: "hello" }; //○
const gen1: GEn<number> = { item: "hello"};  //✖︎

interface GEN2<T extends string | number>  //特定のデータ型だけジェネリクスを使いたい場合の定義方法

//関数に対するジェネリクスの適用
function funcGen<T>(props: T) {
  return {item: props}
}
const gen2 = funcGen<string>("hello")
```
