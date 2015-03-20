# クラスのアクセス制限とファイル分割
クラスについてもう少し踏み込んでいくよ。

## アクセス指定子
前回はとりあえず`public`って書いたあのアクセス指定子。実は全部で3種類あります。

| アクセス指定子 | アクセスできる範囲                             |
| -------------- | ---------------------------------------------- |
| `public`       | どこからでもアクセス可能                       |
| `protected`    | クラス内とそのクラスの派生クラスがアクセス可能 |
| `private`      | クラス内でのみアクセス可能                     |

`protected`に関しては、後回しで説明するね。理解にはクラスの継承の知識が必要。というわけで、今回は`public`と`private`に限って話を進めていくよ。

前回作った`Person`クラスの定義をおさらい。

```cpp
// 人のデータを表すクラス
class Person {
public:          // アクセス指定子
	string name; // 名前
	string job;  // 職業
	int old;     // 年齢

	// Person クラスに所属している print 関数のプロトタイプ宣言
	void print();
};
```

アクセス指定子は`public`が指定されている。ここで実験。変数のアクセス指定子を`private`にしてみよう。

```cpp
#include <iostream>
#include <string>
using namespace std;

class Person {
private:          // public から private に変える
	string name;
	string job;
	int old;

public:           // 関数は public
	void print();
};

void Person::print() {
	cout << name << "「" << old << "歳、" << job << "です。」\n";
}

int main() {
	Person pYJSNPI;

	// private な変数にアクセスしてみる
	pYJSNPI.name = "田所浩二";
	pYJSNPI.job = "学生";
	pYJSNPI.old = 24;

	pYJSNPI.print();

	return 0;
}
```

コンパイルしよう。はい、エラー出ました。`private`にするとアクセスできなくなりました。

## なんで`private`なんてあるのさ

たとえばこの`Person`クラス。年齢を表す変数`old`は`int`型。もし`public`だったら、年齢なのに負の値を代入できてしまう。 ~~`unsigned`にしろとか言うのは無しな。~~ このような変数の不正なアクセスがあると当然バグバグする。

こんなときは、「`public`な関数経由で変数を設定する」ようにすればよい。`private`メンバは所属する関数内からはアクセスできる。不正な値が設定されそうなときに対処できるよ。例見よう

### 例

```cpp
#include <iostream>
#include <string>
using namespace std;

class Person {
private:
	string name;
	string job;
	int old;

public:
	// private 変数に値を設定する関数
	void setName(string n);
	void setJob(string j);
	void setOld(int o);

	void print();
};

// name を設定する関数
void Person::setName(string n) {
	name = n;
}

// job を設定する関数
void Person::setJob(string j) {
	job = j;
}

// old を設定する関数(チェックつき)
void Person::setOld(int o) {
	if (o < 0) {
		// 設定しようとした値が 0 より小さい時、エラーメッセージを出す
		cout << "エラー: old に不正な値が設定されそうだったよ。\n";
		// とりあえず 0 を設定しておく
		old = 0;
	} else {
		// それ以外は普通に設定する
		old = o;
	}
}

void Person::print() {
	cout << name << "「" << old << "歳、" << job << "です。」\n";
}

int main() {
	Person pYJSNPI;

	// 関数を使ってメンバ変数を設定
	pYJSNPI.setName("田所浩二");
	pYJSNPI.setJob("学生");

	// 不正な値を設定してみる
	pYJSNPI.setOld(-114514);

	// 出力
	pYJSNPI.print();

	// 正しい値をセットしてみる
	pYJSNPI.setOld(24);

	// 出力
	pYJSNPI.print();

	return 0;
}
```

### 実行結果

> エラー: old に不正な値が設定されそうだったよ。  
> 田所浩二「0歳、学生です。」  
> 田所浩二「24歳、学生です。」

### 解説
変数を直接外に晒すのではなく、関数でクッションを挟むことにより、不正な代入を回避することができる。適切な値が入っていることが保証されるから、バグが少なくなる。やったぜ。

## ファイルを分けよう
例で使っている`Person`クラスがだんだんでかくなってきた。別ファイルに分けてスッキリさせよう。

クラスを別ファイルに分けるとき、基本的にクラス1つにつき、2つのファイルに分けられる。実際に分けてみよう。

```cpp
//
// Person.h
//

// include ガード。多重 include を防止できる
// #pragma once をサポートしていないコンパイラもあるので注意
#pragma once

// クラスの定義に string があるから include
#include <string>

using namespace std;

class Person {
private:
	string name;
	string job;
	int old;

public:
	void setName(string n);
	void setJob(string j);
	void setOld(int o);
	void print();
};
```

```cpp
//
// Person.cpp
//

// cout を使うため include
#include <iostream>
// Person クラスの定義部の参照に必要
#include "Person.h"

// string は Person ですでに include されている

using namespace std;

void Person::setName(string n) {
	name = n;
}

void Person::setJob(string j) {
	job = j;
}

void Person::setOld(int o) {
	if (o < 0) {
		cout << "エラー: old に不正な値が設定されそうだったよ。\n";
		old = 0;
	} else {
		old = o;
	}
}

void Person::print() {
	cout << name << "「" << old << "歳、" << job << "です。」\n";
}
```

```cpp
//
// main.cpp
//

// Person クラスを使うときはヘッダファイルのみの include で OK
#include "Person.h"

int main() {
	Person pYJSNPI;
	pYJSNPI.setName("田所浩二");
	pYJSNPI.setJob("学生");
	pYJSNPI.setOld(24);
	pYJSNPI.print();

	return 0;
}
```

> 田所浩二「24歳、学生です。」

`main.cpp`があり得ないくらいスッキリしました。C++での開発はこのようにクラスを2つのファイルに分けて書くのが普通です。

## 練習問題
### I
[前回の練習問題](03-ClassIntro.md#練習問題)の`Car`クラスを2つのファイルに分けて書きなさい(`Car.h`と`Car.cpp`)。

### II
`Car`クラスの変数を`private`に指定しなさい。また、変数を設定する関数と変数を取得する関数を新しく書きなさい(いずれも`public`)。なお、`capacity`に値を設定する関数は、1より小さい値を設定しようとした時にエラーメッセージを出すこと。

[練習問題の回答](04-ClassAccessibility-Answer.md)

<[前に戻る](03-ClassIntro.md) - [目次へ](../README.md) - [次へ進む](05-ConstructorDestructor.md)>
