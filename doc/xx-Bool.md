# boolのおはなし
`bool`だワッショイ

## #なにそれ
C++で新しく追加された型。値は`true`か`false`のどちらかしか取りません。

`true`と`false`の意味は大体以下の通り。プログラム中でも大体この意味通りにクラスが設計されたりすることが多い。

| C++     | 数値 | 英語 | 日本語 | やわめ | 語録           |
| ------- | ---- | ---- | ------ | ------ | -------------- |
| `true`  | 1    | Yes  | 真     | はい   | そうだよ(便乗) |
| `false` | 0    | No   | 偽     | いいえ | ないです       |

## 使い方
こんな風に`if`文で使ったり。

```cpp
bool b = false;
b = true;

if (b) {
	cout << "b is true\n";
} else {
	cout << "b is false\n";
}
```

> b is true

他にも色々用途はあると思います(適当)

## boolのサイズ
大抵の処理系だったら`bool`型のサイズは **1バイト** です。以下のコードで調べることができます。

```cpp
cout << "sizeof(bool) == " << sizeof(bool) << '\n';
```

> sizeof(bool) == 1

C言語では、「はい」と「いいえ」の値を表現するのに`int`型を使っていました。`0`なら偽、`0`以外なら真です。

`int`型のサイズは **4バイト** です。「はい」と「いいえ」の2通りしか値が無いのに4バイトもメモリを消費する。そんなことしなくていいから(良心)

そこで`bool`。1バイトしかメモリを消費しない！「はい」か「いいえ」しか表せないから意味が分かりやすい！じゃけん`bool`使いましょうね〜

## boolの演算
このセクションでは、`printBool()`関数を以下のように定める。

```cpp
void printBool(bool b) {
	if (b) cout << "true\n";
	else   cout << "false\n";
}
```

### AND
| 左辺  | 右辺  | 結果  |
| ----- | ----- | ----- |
| false | false | false |
| false | true  | false |
| true  | false | false |
| true  | true  | true  |

```cpp
// 演算子は & && * のどれか。&& が一般的
printBool(false && false);
printBool(false && true );
printBool(true  && false);
printBool(true  && true );
```

> false  
> false  
> false  
> true

### OR
| 左辺  | 右辺  | 結果  |
| ----- | ----- | ----- |
| false | false | false |
| false | true  | true  |
| true  | false | true  |
| true  | true  | true  |

```cpp
// 演算子は | || + のどれか。|| が一般的
printBool(false || false);
printBool(false || true );
printBool(true  || false);
printBool(true  || true );
```

> false  
> true  
> true  
> true

### NOT
| 項    | 結果  |
| ----- | ----- |
| false | true  |
| true  | false |

```cpp
// 演算子は !
printBool(!false);
printBool(!true );
```

> true  
> false

### XOR
| 左辺  | 右辺  | 結果  |
| ----- | ----- | ----- |
| false | false | false |
| false | true  | true  |
| true  | false | true  |
| true  | true  | false |

```cpp
// 演算子は ^ - のどれか。^ が一般的
printBool(false ^ false);
printBool(false ^ true );
printBool(true  ^ false);
printBool(true  ^ true );
```

> false  
> true  
> true  
> false

[目次へ](../README.md)
