# クラスの基礎
C++の目玉機能であるクラスについて解説していくよ。

## クラス #とは
**すごい構造体。** これに尽きる。早速定義の仕方を見てみよう。

### 構文

```cpp
class クラス名 {
アクセス指定子:
	変数の宣言;
	...
	関数のプロトタイプ宣言;
	...
}; // <- 最後のセミコロンを忘れずに！
```

`クラス名` → 構造体名みたいなもの → わかる:thumbsup:  
`変数の宣言` → 構造体と同じ → わかる:thumbsup:  
`アクセス指定子` → は？:anger:  
`関数のプロトタイプ宣言` → は？:anger:

これもう分かんねぇな。とりあえず構造体みたいに使ってみよう。

### 例 (構造体っぽい使い方)

```cpp
#include <iostream>
#include <string>
using namespace std;

// 人のデータを表すクラス(構造体)
class Person {
public:          // アクセス指定子はとりあえず public にしとけ(おまじない)
	string name; // 名前
	string job;  // 職業
	int old;     // 年齢
};

// Person 型の情報を使って自己紹介するグローバル関数
void print_person(Person p) {
	cout << p.name << "「" << p.old << "歳、" << p.job << "です。」\n";
}

int main() {
	// Person 型の変数 pYJSNPI を宣言
	Person pYJSNPI;

	// pYJSNPI の各メンバ変数を設定
	pYJSNPI.name = "田所浩二";
	pYJSNPI.job = "学生";
	pYJSNPI.old = 24;

	// print_person 関数に pYJSNPI を渡す
	print_person(pYJSNPI);


	// Person 型の変数 pTNOK を宣言
	Person pTNOK;

	// pTNOK の各メンバ変数を設定
	pTNOK.name = "谷岡俊一";
	pTNOK.job = "ヤクザ";
	pTNOK.old = 26;

	// print_person 関数に pTNOK を渡す
	print_person(pTNOK);


	return 0;
}
```

### 実行結果
> 田所浩二「24歳、学生です。」  
> 谷岡俊一「26歳、ヤクザです。」

### 解説
`pYJSNPI`や`pTNOK`みたいな変数は「`Person`クラスの **インスタンス** 」とか「`Person`クラスの **オブジェクト** 」とか言ったりする。

構造体とほぼ同じ。ここまでは。

## クラスに関数を持たせよう
構造体は、関連性のある変数をひとまとめにして新しい型を作り出すことができる。クラスは、関数もひとまとめにできる。前節の`print_person()`関数って、`Person`型に関係する関数だよね。

まとめちゃおう！！`Person`クラスをバージョンアップだ！

### 構文
クラスに関数を宣言することに焦点を当てていくよ。

```cpp
class クラス名 {
アクセス修飾子:
	// 関数プロトタイプ宣言
	関数の戻り値 関数名(引数リスト);
	...
};

// 関数の本体
関数の戻り値 クラス名::関数名(引数リスト) {
	// 処理
}
```

### 例

```cpp
#include <iostream>
#include <string>
using namespace std;

// 人のデータを表すクラス
class Person {
public:          // アクセス指定子はとりあえず public にしとけ(おまじない)
	string name; // 名前
	string job;  // 職業
	int old;     // 年齢

	// Person クラスに所属している print 関数のプロトタイプ宣言
	void print();
};

// Person クラスの print 関数の本体
void Person::print() {
	// ここの name, old, job は Person クラスの中の変数を表している。
	// この print 関数は Person クラスに所属しているからね
	cout << name << "「" << old << "歳、" << job << "です。」\n";
}

int main() {
	// Person 型の変数 pYJSNPI を宣言(インスタンス生成)
	Person pYJSNPI;

	// pYJSNPI の各メンバ変数を設定
	pYJSNPI.name = "田所浩二";
	pYJSNPI.job = "学生";
	pYJSNPI.old = 24;

	// pYJSNPI の print 関数を呼び出す
	pYJSNPI.print();


	// Person 型の変数 pTNOK を宣言(インスタンス生成)
	Person pTNOK;

	// pTNOK の各メンバ変数を設定
	pTNOK.name = "谷岡俊一";
	pTNOK.job = "ヤクザ";
	pTNOK.old = 26;

	// pTNOK の print 関数を呼び出す
	pTNOK.print();


	return 0;
}
```

### 実行結果
> 田所浩二「24歳、学生です。」  
> 谷岡俊一「26歳、ヤクザです。」

### 解説
メンバ変数に値を代入するとき、例で言うと

```cpp
pYJSNPI.name = "田所浩二";
```

のようなコードは、日本語にすると

> `pYJSNPI`の`name`に`"田所浩二"`を代入する

と言った風に翻訳できる。関数に関しても同じで、

```cpp
pYJSNPI.print();
```

は、

> `pYJSNPI`の`print()`関数を呼び出す

と翻訳できる。両方とも、  
**「`pYJSNPI`という物体に対して何かしている」**  
という風にとらえることができる。考え方を統一できる。何の変哲もないところにグローバルな関数を置くのとは大違い。

別の視点でメリットを考えてみよう。次のようなシチュエーション。

**「`Person`クラスのインスタンスを標準出力に出力する関数どれだっけ・・・？関数名忘れちゃった」**  

グローバルな関数だったら、ソースコード中に目を通してやっと発見できるといったところ。一方、ちゃんとクラスにまとめておけば、クラスの定義部分を見ればすぐに分かります。

このような例だったら短いし、探すのには苦労しないけど、実際はこんな超小規模のプログラムなんて作らないよね。もっと大規模になるはず。大規模になればなるほど探すのが大変になる。そう考えれば、関数をクラスにまとめておくメリットは十二分にあると思うよ。だからどんどんまとめよう！！

## 練習問題
### I
車を表すクラス`Car`を作成しなさい。ただし以下のメンバ変数と関数を含むこと(アクセス指定子は`public`)。
- 車のナンバーを表す`int`型の変数`number`
- 車が最大何人乗れるかどうかを表す`int`型の変数`capacity`
- 上の2変数を標準出力に出力する関数`show()`。出力する文字列はある程度整形すること  
	**表示例:** この車のナンバーは1234で5人乗りです。

### II
I で作成した`Car`クラスのインスタンスを作成し、ナンバーに114514を、最大乗車人数を810人に設定し、`show()`関数を呼んで出力を確かめなさい。

[練習問題の回答](03-ClassIntro-Answer.md)

<[前に戻る](02-Function.md) - [目次へ](../README.md) - 次へ進む>
