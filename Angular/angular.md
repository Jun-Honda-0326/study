## フォーム
```js
<input type="text" [(ngModel)] = "member.name" placeholder="名前">
ngModelというのがangular特有の書き方
app.Modules.tsにFormsModuleをimportする
```
## 繰り返し処理
```js
<li *ngFor= "let member of members">
//membersという変数をmemberという変数に束縛している
ループ内ではmemberという変数が使える
```
