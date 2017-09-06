# lambda（ラムダ）について
プログラム言語には、関数に似たものがもうひとつあります。それは「ラムダ式」です。  
ラムダという名前は数学由来ですが、ラムダ式自体はそれほど難しいものではありません。簡単にいえば、関数の別の書き方です。  

## ラムダ式ってどんなもの？
ラムダ式は、別名「匿名関数」とも呼ばれる名前がない関数のようなものです。    
関数型プログラミングから取り入れられた機能であり、オブジェクト指向プログラミングと併用することでプログラミングの幅が広がります。  
ラムダ式は関数に近いものですが、関数とは異なる特性を持っています。  
次に、ラムダ式と関数との違いについてみていきましょう。  

## ラムダ式と関数の違い  
### ラムダ式は”式”であり、”文”ではない  
最も重要な違いは、ラムダ式はあくまで”式”であり、”文”ではないことです。   
つまり、ラムダ式は単一行の式しか持つことができず、複数行のブロックを持つことはできません。  
このため、ラムダ式で複雑な処理を行うことは困難です。  
ラムダ式は、関数で書くと煩雑になりがちな処理をインラインで書くために使われることが多いでしょう。  

### 関数の引数として直接渡せる  
関数の引数として関数を渡すことができます。  
しかし、関数の場合は直接引数として渡すことはできず、どこか別の場所で定義してから渡す必要があります。  
一方、ラムダ式は関数の引数としてインラインで記述することができ、よりシンプルに記述することが可能です。  

### ラムダ式に名前はない  
冒頭でもお伝えしましたが、ラムダ式自体には名前はありません。  
関数のように定義時に名前をつけることはなく、無名のまま直接関数などに渡される場合が多いです。  
または、変数に代入して、擬似的に名前をつけることもできます。  

## Ruby においてのlambdaとは  
Rubyではほぼすべてがオブジェクトですがブロックはオブジェクトではありません。  
そしてブロックをオブジェクトとしたものがProc.newです。  
ここではlambdaの例を挙げて行きます。  
lambda{~}でlambdaを使う事でき変数に代入する事も出来ます。  
これはProcのインスタンスを生成して返却しているからです。  
なので評価もcallメソッドで行えます。  
では一体なにがProc.newと違うのか。  

example.rb  
def method_lambda  
  lambda1 = lambda{ p "lambda" }#Proc.new が返却  
  lambda1.call  
end  
method_lambda  
#実行結果  
=>"lambda"  

### 違い１つ目    
lambdaはブロックの引数の数が違うとエラーを起こしてくれます。  
Proc.newで数の違う引数を渡す場合  
このように足りない引数の部分はnilが入ります。   

example.rb  
proc = Proc.new{ |a,b,c| p "#{a},#{b},#{c}" }  
proc.call(2, 4)  
#実行結果  
=> 2,4,nil  
lambdaで数の違う引数を渡す場合  
このようにエラーが発生します。  
なのでlambdaのほうが厳密となります。  

example.rb  
lambda1 = lambda{ |a,b,c| p "#{a},#{b},#{c}" }  
lambda1.call(2, 4)  
#実行結果  
=> wrong number of arguments (2 for 3) (ArgumentError)  

### 違い２つ目  
明示的にreturnやbreakを行った場合の挙動が違います。  

Proc.newの場合  
example.rb  
def method_proc  
  proc = Proc.new { return p "proc"}  
  proc.call  
  p "method_proc"  
end  
#実行結果  
=>"proc"  

lambdaの場合  
example.rb  
def method_lambda  
  lambda1 = lambda{ return p "lambda"}  
  lambda1.call  
  p "method_lambda"  
end  
method_lambda  
#実行結果  
=>"lambda"  
  "method_lambda"  

Proc.newの方はcallメソッドを実行した後の処理が評価されてません。それに対してlambdaの方は最後まで評価されています。  
これはProc.newの場合、returnを書くとmethod_procメソッド自体を抜けてしまうからです。  
lambdaの方はreturnした後、method_lambdaメソッドに戻ります。  
breakの場合、Proc.newでは例外が発生します。lambdaの方は同じように手続きオブジェクトを抜けます。  
ただし明示的にreturnを書かなければその後も評価されます。  

## まとめ  
ブロックはオブジェクトではありません。  
ブロックをオブジェクトとしたものがProc.newです。  
Proc.newに対してlambdaの方が引数に関しては厳密です。  
returnなどの動作に関してはProcとlambdaでは異なりlambda方はメソッドに近い動きです。  


参考URL：http://programming-study.com/technology/python-use-lambda-ceremony/  
title:Python超入門その１５〜かっこいいラムダ式を使いこなそう〜  

参考URL：http://qiita.com/ryo-ma/items/24c46592b45775e8644d  
title:Rubyの ブロック、Proc.new、lambdaの違い  

参考URL：http://d.hatena.ne.jp/mirichi/20141008/p1
title:手続き脳のためのラムダ式のはなし
