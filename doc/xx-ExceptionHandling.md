# たのしい例外処理 基礎編

STLを使いこなすためにも例外処理を覚えよう！

## `atoi()`関数の問題点
C言語には`stdlib.h`に`atoi()`っていう文字をint型の数値にして返す関数があった。

### 例
```c
#include <stdio.h>
#include <stdlib.h>

int main(){
  int num;
  num = atoi("100");

  printf("%d", num);

  return 0;
}
```

### 実行結果
> 100

ところで、この関数に数値以外の文字列を渡すとどうなるんだろうか…

### 例
```c
#include <stdio.h>
#include <stdlib.h>

int main(){
  int num;
  num = atoi("十一万四千五百十四");

  printf("%d", num);

  return 0;
}
```

### 実行結果
> 0


0になった。このatoi()関数に数値以外の文字列を渡すと**とりあえず0を返す** という事が分かった。

つまり、この関数に0を渡しても数値以外の文字列を渡しても**同じ0が返ってくる** のだ。適当すぎる。

「atoi()の中身をどこかで厳密に監視すればいいんじゃね？」　ごもっともです。けど面倒な時もあります。

たとえば、これに **文字と数字のちゃんぽん** を渡すとこうなるんですよ。

```c
#include <stdio.h>
#include <stdlib.h>

int main(){
  int num;
  num = atoi("11万4千5百十4");

  printf("%d", num);

  return 0;
}
```

### 実行結果
> 11

**お分かりいただけただろうか。** 文字になるまでとりあえず数値化している。

この関数では、全て数値を渡した時以外にどんな動作を起こすかわからない。

## ここで、`stoi()`関数を見てみましょう。

`stoi()`関数は`string`をインクルードすることで使うことができる。

見た目で`atoi()`と違うのは引数が`string`型だというところだ。変数で文字列を渡すときは注意してほしい。

### 例
```c++
#include <iostream>
#include <string>
using namespace std;

int main(){
  int num;
  num = stoi("100");

  cout << num << endl;

  return 0;
}
```

### 実行結果
> 100

`atoi()`と見た目動作はさほど変わらない。

では、ここで数字以外の文字列を渡してみましょう。


### 例
```c++
#include <iostream>
#include <string>
using namespace std;

int main(){
  int num;
  num = stoi("十一万四千五百十四");

  cout << num << endl;

  return 0;
}
```

### コンパイル結果例(g++)
> Runtime Error

> terminate called after throwing an instance of 'std::invalid_argument'

>  what():  stoi

### コンパイル結果例(Visual Studio)
> ハンドルされない例外が 0x751A3D67 で発生しました

>(Project1.exe 内): Microsoft C++ の例外: std::invalid_argument (メモリの場所 0x00BEFC80)。

**落ちた。**

**確かに`atoi()`は曖昧な関数だったかもしれないけど`stoi()`は文字突っ込んだらプログラムごと落ちたじゃねえか！どこが使える関数なんだよハゲ！**

と思っているかもしれないが、とりあえず落ち着いてエラーメッセージを見てほしい。

> ハンドルされない例外が 0x751A3D67 で発生しました

>(Project1.exe 内): Microsoft C++ の例外: std::invalid_argument (メモリの場所 0x00BEFC80)。

これは、「`std::invalid_argument`というエラーが出てきたけど、このエラーが来たときの対策が書いてないからプログラム落とすね」

というとても優しいメッセージなのだ。

つまり、このエラーが来た時に対策を教えてあげればいい。これが**例外処理**である。


## 例外処理をしよう

「例外処理」という物騒な名前だが、やることは非常に単純だ。

プログラムに「このへん例外があるかも」と教えてあげて、「この例外はこうしてね」と処理を書いてあげればよい。

ちなみに`stoi()`の例外処理の話は当分先なので、今は`std::invalid_argument`のことは忘れてほしい。

### tryブロック

「このへん例外があるかも」と教えてあげるには、`try`ブロックを使う。

```c++
try{
  // 例外が起こるかもしれない処理
}
```

何も考えずに、ここに例外が起こるかもしれない処理を書いてあげるだけだ。

```c++
try{
  num = stoi("十一万四千五百十四");  // std::invalid_argument
}
```

### throw

`throw`を使えば、例外を発生させることが出来る。

```c++
throw exception; // exceptionは任意の値
```

こんな感じで、

```c++
try{
  // なんか処理
  throw 10;
}
```

で、`int`の10が例外として投げられる。

`stoi()`関数の中身にはこのthrowが記述されているので、例外が発生するのだ。

なお、`throw`が書いてある行に入った瞬間、プログラムは後述の`catch`ブロックを探し、そこへ処理を飛ばそうとする。

### catchブロック

さて、`throw`の例の処理では`10`を例外として返してくる。次はその例外をキャッチしてあげよう。

「この例外はこうしてね」と教えてあげるには、`catch`ブロックを使う。

```c++
catch(type arg){
  // 例外が起きた時の処理
}
```

typeにはthrowされた値の型、argにはthrowされた値が入る変数を書く。分かりやすく言うなら関数の引数と一緒だ。

`throw 10;`をcatchしてあげたいなら、次のように書けばよい。

```c++
catch(int num){
    // なんか処理
}
```

このcatchブロックは、**tryブロックの直後に書かなければいけない。**

-----

ここで一度、おさらいしてみよう。

```c++
#include <iostream>
using namespace std;

int main(){
  try{
    throw 10;       // 例外発生
  }
  catch(int num){   // num には throw された 10 が入る
    cout << num << endl;
  }

  return 0;
}
```
実行結果
> 10

さあ、例外処理の楽しさが分かってきたかな？　どんどん進もう！

<前に戻る - [目次へ](../README.md) - [次へ進む](xx-ExceptionHandling2.md)>
