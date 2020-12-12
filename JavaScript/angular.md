# Angularまとめ
Angularでは、アプリを構成する機能を モジュール という単位で分割して作成できるようになっており、1つ以上のモジュールを組み合わせることでアプリ全体を構成します。
また、Angularでは、ビュー・スタイル・ロジックをひとまとめにした コンポーネント という単位でUI部品を作っていきます。作ったコンポーネントは、モジュールに登録することでそのモジュール内で利用可能になります。

## 読み込みの流れ
- main.ts は AppModule を起動する
- AppModule は AppComponent を読み込んでいる
- AppComponent は app.component.html を読み込んでいる



## component作成のためのCLI
```
ng generate component [name]
```

## フォーム
```js
<input type="text" [(ngModel)] = "member.name" placeholder="名前">
ngModelというのがangular特有の書き方
app.Modules.tsにFormsModuleをimportする
```
## 繰り返し処理
```
<li *ngFor= "let member of members">
//membersという変数をmemberという変数に束縛している
ループ内ではmemberという変数が使える
```
## サービスの実装
コンポネント内では直接データの取得や保存を行うべきではなく、コンポネントはデータの受け渡しのみの処理を行う。
データを一括取得するService実装が必要である。
