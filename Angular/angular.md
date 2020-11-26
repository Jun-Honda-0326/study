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
