# 基本構文

## コンソールに出力
```py
print("Hello World") // Hello World
```
## 数
```py
- 引き算
+ 足し算
* 掛け算
/ 割り算
** べき乗
round(3.141592, 2)  // 3.14 下2桁まで出力
```
## 文字列
```py
print('hello') //hello
print("hello") //hello
print('I don\'t know') //I'dont know
print("say "I don't know \""") //say "I'dont know"
print('Py' + 'thon') //Python
```

## 文字列のインデックス
```py
word = print
word[1] // r
word[-1] //t
word[0:2] //pr
word[2:5] //int
```

## 文字列のメソッド
```py
s = ”My name is Mike. Hi Mike”

startswith(x) //文字列がxから始まっているかどうか調べるメソッド。　true or falseで返却される
s.startwith('m')  //false
s.startswith('M') //true

find // ('xxx') //文字列が何番目にあるか調べるメソッド
s.find('Mike') //11(空白を含めての最初のMikeが何番目にあるか)
s.rfind('Mike')  //最後の'Mikeが何番目から始まるか' 後ろから調べる
s.count('Mike') //'Mike'が文字列の中に何個入っているか
s.capitalize()//My name is mike hi mike. 一番初めだけ大文字
s.title()//My Name Is Mike Hi Mike 単語の最初だけ大文字/
s.upper()// MY NAME IS MIKE HI MIKE 全て大文字
s.lower() my name is mike hi mike 全て小文字
s.replace('Mike', 'Bob')// My name is Bob. Hi Bob Mikeという文字列をBobという文字列に変換
```

## 文字の代入
```py
'a is {}'.format('honda')   //a is honda
'a is {2} {1} {0}'.format(1,2,3) //a is 3 2 1
'My name is {name} {family}'.formt{name = 'Jun', family = 'Honda'}
// My name is Jun Honda
```

## リストの操作
```py
l = [1,2,3,4,5] //リストの宣言
l[-2] //4
l[0:2] //[1,2]
l[:2] //[1,2]
[l2:4] //3,4
l[1:] //[2,3,4,5]
leb[l] //5

s = ['a','b','c','d','e','f']
s[2:5] = []  // ['a', 'b', e]
s[2:5] = ['C', 'D', 'F'] //['a', 'b', 'C', 'D', 'E', 'f']

n = [1,2,3,4,5] m = [6,7,8,9,10]
n.append(100) //n = [1,2,3,4,5,100]
n.insert(1,200) //n = [1,200,2,3,4,5,100]
del n[1] // n = [1,2,3,4,5,100]
n.remove(100) //n = [1,2,3,4,5]

n.extend(m) //[1,2,3,4,5,6,7,8,9,10]
```
## リストのメソッド
```py
r = [1,2,3,4,5,6,1,2,3] //リストの関数
r.index(3) //2  //3は要素の何番目にあるか
r.index(3,3) //8  //要素の3番目から3検索を検索する
r.sort() // r = [1,1,2,2,3,3,4,5,6] 順番に並べ替え
r.sort(reverse=True) // 逆順に並べ替え

s = 'My name is Mike'
to_split = s.split(' ') // 'My', 'name', 'is', 'Mike'
```
## タプル
- 読み込み専用のようなデータを生成する際に使う
- 値の代入ができない
- リストと同じようなメソッドは使える
- `,`を付けないとタプルとして認識されない
```py
定義
t = (1,2,3)
t = ([1,2,3], [4,5,6])
t[0][0] // 1
```
## 辞書型(Map)
```py
d = {'x': 10, 'y': 20}
d['x'] //10
d.keys  //[x, y]
d.values //[10,20]
d.get('x') //10
d2 = {'z': 30}
d.update(d2)  //{'x': 10, 'y': 20, 'z': 30}
```


## 集合型
```py
a = {1,2,3,4,5,6}
b = {4,5,6,7}
a & b //and
a | b //or
 a ^ b //重複していないもの
 sel(list) //listはリスト型
 //重複しないリストを作る
```
