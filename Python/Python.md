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

## リスト型
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
