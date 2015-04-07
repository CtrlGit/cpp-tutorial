# 静的メンバ
静的メンバは簡単に言えば、「クラスに関連付けられたグローバル変数・グローバル関数」のこと。

## なにそれ
静的メンバ変数を使えば、クラス全体で共有している変数(グローバル変数みたいなの)を宣言できます。

### 構文

```cpp
class クラス名 {
アクセス指定子:
	static 型名 静的メンバ変数名; // 静的メンバ変数の宣言
	static 戻り値の型 静的メンバ関数名(引数リスト); // 静的メンバ関数のプロトタイプ宣言
};

// 静的メンバ変数の初期化
型名 クラス名::変数名 = 初期値;

// 静的メンバ関数の実装部
戻り値の型 クラス名::静的メンバ関数名(引数リスト) {
	...
}

void hoge() {
	// 静的メンバへのアクセス
	クラス名::静的メンバ名
}
```

### 例

```cpp
#include <iostream>
using namespace std;

class SampleClass {
private:
	// 静的メンバ変数
	// 生成されたオブジェクト数を記録
	static int objectCount;
public:
	// 静的メンバ関数
	// 生成されたオブジェクト数を表示する
	static void printObjectCount();

	// 普通のメンバ変数
	int number;

	// コンストラクタ・デストラクタ
	SampleClass();
	~SampleClass();
};

// 静的メンバ変数の初期化
int SampleClass::objectCount = 0;

// 静的メンバ関数の実装
void SampleClass::printObjectCount() {
	// オブジェクト数を標準出力に出力

	// 静的メンバ関数から、普通のメンバ変数にはアクセスできない
	// この例だと変数 number にはアクセスできない
	cout << "SampleClass のオブジェクト数は " << objectCount << " です。\n";
}

// コンストラクタ
SampleClass::SampleClass() {
	cout << "SampleClass のオブジェクトが生成されました。\n";

	// オブジェクトが生成されるから、静的メンバ変数 objectCount をインクリメント
	objectCount++;
}

// デストラクタ
SampleClass::~SampleClass() {
	cout << "SampleClass のオブジェクトが破棄されました。\n";

	// オブジェクトが破棄されるから、静的メンバ変数 objectCount をデクリメント
	objectCount--;
}

int main() {
	// SampleClass の静的メンバ関数の呼び出し
	SampleClass::printObjectCount();

	// オブジェクト生成
	SampleClass* p1 = new SampleClass();
	SampleClass::printObjectCount();

	// オブジェクトを 5 個生成
	SampleClass* pArray = new SampleClass[5];
	SampleClass::printObjectCount();

	// オブジェクト破棄
	delete p1;
	delete[] pArray;

	SampleClass::printObjectCount();

	return 0;
}
```

### 実行結果
> SampleClass のオブジェクト数は 0 です。  
> SampleClass のオブジェクトが生成されました。  
> SampleClass のオブジェクト数は 1 です。  
> SampleClass のオブジェクトが生成されました。  
> SampleClass のオブジェクトが生成されました。  
> SampleClass のオブジェクトが生成されました。  
> SampleClass のオブジェクトが生成されました。  
> SampleClass のオブジェクトが生成されました。  
> SampleClass のオブジェクト数は 6 です。  
> SampleClass のオブジェクトが破棄されました。  
> SampleClass のオブジェクトが破棄されました。  
> SampleClass のオブジェクトが破棄されました。  
> SampleClass のオブジェクトが破棄されました。  
> SampleClass のオブジェクトが破棄されました。  
> SampleClass のオブジェクトが破棄されました。  
> SampleClass のオブジェクト数は 0 です。

### 解説
メンバに`static`をつけると、そのメンバは静的メンバになります。

静的メンバ変数は、クラスごとに唯一の実体を持っています。この変数は各インスタンスオブジェクト間で共有されています。言ってしまえばクラス内のグローバル変数のようなもの。

静的メンバ関数も、言ってしまえばクラス内のグローバル関数のようなもの。ここで静的メンバ関数について注意。静的メンバ関数内では、通常のメンバ変数・メンバ関数にアクセスすることはできません。

```cpp
class SampleClass {
	// 略
public:
	// 静的メンバ関数
	static void printObjectCount();

	// 普通のメンバ変数
	int number;

	// 略
}

void SampleClass::printObjectCount() {
	// 普通のメンバ変数にはアクセスできないのでコンパイルエラー
	// どのオブジェクトの number にアクセスするの？？？
	cout << number << '\n';
}
```

静的メンバは、クラスに関連付けられているので、オブジェクトがあろうが無かろうが、関係なくアクセスできます。普通のメンバ変数・メンバ関数はオブジェクトに関連付けられているので、静的メンバ関数内からはアクセス出来ません。

<[前に戻る](07-Reference.md) - [目次へ](../../README.md) - 次へ進む>
