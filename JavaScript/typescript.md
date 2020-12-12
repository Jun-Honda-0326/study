# TypeScriptについてまとめます

### リテラル型
決め打った値を示す型。
```ts
const three:3 = 3;
const hello: helllo = hello
※その値しか代入できない
```

## anyとunknown
any unkonow型はどのような値も代入できる
any型に代入したプロパティ、メソッドは使用することはできる(基本使わない)<br>
unknown型に代入したプロパティ、メソッドは使用することど実行することもできない。
