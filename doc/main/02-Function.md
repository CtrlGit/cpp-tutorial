# 関数と関数テンプレート
基本的な関数の定義の仕方はC言語と同じだけど、やっぱりC++では機能が追加されている。

## 関数のオーバーロード
C言語は同じ名前の関数を定義できなかったけど、C++では条件を満たせばできる。

### 例

```cpp
#include <iostream>
using namespace std;

// a と b のうち大きい方を返す(int版)
int max(int a, int b) {
	cout << "int 版 max 関数が呼ばれました\n";
	if (a > b) {
		return a;
	} else {
		return b;
	}
}

// a と b のうち大きい方を返す(double版)
double max(double a, double b) {
	cout << "double 版 max 関数が呼ばれました\n";
	if (a > b) {
		return a;
	} else {
		return b;
	}
}

int main() {
	int i1 = 810;
	int i2 = 893;
	int m1 = max(i1, i2);
	cout << m1 << '\n';

	double d1 = 11.4;
	double d2 = 51.4;
	double m2 = max(d1, d2);
	cout << m2 << '\n';

	return 0;
}
```

### 実行結果
> int 版 max 関数が呼ばれました  
> 893  
> double 版 max 関数が呼ばれました  
> 51.4

### 解説
C++では、引数が異なる同じ名前の関数を定義してもよいことになっている。引数の数もしくは引数の型が異なればOK。このように、引数が異なる同じ名前の関数を複数定義することを **関数のオーバーロード** という。

しかし、 **戻り値の型のみが異なる関数は複数定義できない。** どれ呼べばいいか分からなくなるからね。

## 関数テンプレート
前節の`max()`関数、型は違うけど処理内容は全く同じである。型ごとの`max()`関数を複数定義するのはとても非効率的。[DRY(Don't repeat yourself)原則](http://ja.wikipedia.org/wiki/Don't_repeat_yourself)は守ったほうがバグ生みにくいし絶対良いはず。

そこで、C++では「扱う型のみが異なる関数をテンプレ化」できるようになっています。

これに関しては実際の例を見たほうが分かりやすいと思うから使用例、ドン！

### 構文

```cpp
template <class テンプレート引数名1, class テンプレート引数名2, ... >
関数の宣言
```

これだけじゃまだ分かんねぇよな。うん。

### 例

```cpp
#include <iostream>
using namespace std;

// a と b のうち大きい方を返す(テンプレート版)
// 型をとりあえず T と宣言する
// (名前は T じゃなくても良い。お好きな名前をどうぞ)
template <class T>
T max_t(T a, T b) {
	if (a > b) {
		return a;
	} else {
		return b;
	}
}

int main() {
	int i1 = 810;
	int i2 = 893;
	int m1 = max_t(i1, i2);
	cout << m1 << '\n';

	double d1 = 11.4;
	double d2 = 51.4;
	double m2 = max_t(d1, d2);
	cout << m2 << '\n';

	return 0;
}
```

お、`max()`関数が1つになったぞ！

### 実行結果
> 893  
> 51.4

やったぜ。

### 解説
1回目の`max_t()`関数の呼び出しで、`int`型の値2つを渡している。そうすると、仮の型`T`が`int`になった以下の内容の`max_t()`関数がコンパイル時に自動的に生成される。

```cpp
int max_t(int a, int b) {
	if (a > b) {
		return a;
	} else {
		return b;
	}
}
```

2回目の`double`型でも同様に関数が自動生成される。`char`でも`long`でも`float`でも`string`でも同様に。スッキリ！

自動生成する型を明示的に指定する場合は、以下のように呼び出す。

```cpp
int i1 = 114;
int i2 = 514;

// T を double 型として max_t 関数を呼ぶ
// i1 と i2 は暗黙的に double 型にキャストされる
double m = max_t<double>(i1, i2);
```

同じ記述を何度もせずに済む。DRY原則に則ってる。ｲｲﾈ!ｲｲﾈ!ｲｲﾈ!ｲｲﾈ!

<[前に戻る](01-Hello.md) - [目次へ](../../README.md) - [次へ進む](03-ClassIntro.md)>
