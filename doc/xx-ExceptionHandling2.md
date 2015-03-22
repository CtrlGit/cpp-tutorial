# たのしい例外処理 応用編

楽しい例外処理の時間だぞい

### 複数のthrow, catch

`throw`はtryブロックの中で複数書くこともできる。

```c++
try {
    if (sugoi < yabai) {
        throw 10;
    } else {
        throw 100;
    }
}
catch (int num) {
    cout << num << endl; // 10 または 100 を出力する
}
```

`catch`も **違う型ならば** 1つの`try`ブロックに対して複数書くことができる。

```c++
try {
    if (sugoi < yabai) {
        throw 10;
    } else {
        throw "ラーメン二郎";
    }
}
catch (int num) {
    cout << num << endl;
}
catch (string str) {
    cout << str << endl;
}
```

### 禁断の魔術`catch(...)`

今まで`catch`には変数の型をいちいち指定したけど、なんと魔法の呪文によって全ての例外を受け取る処理を書くことも出来るぞ！

(紹介はするけど適当に使うと何が起きるか分からないので、使うときは中身に処理は書かないほうがいいよ)

```c++
catch (...) {
    // 例外が起きた時の処理
}
```

この時には値は渡されず、この中の処理を行うだけ。

以下の例のように、組み合わせて使うことも出来るぞ！

```c++
try {
    if (sugoi < yabai) {
        throw 10;
    } else if (sugoi == yabai) {
        throw "ラーメン二郎";
    } else {
        double array[10];
        throw array[10]; // ちなみに配列をthrowすると、先頭のポインタが渡されます
  }
}
catch (int num) {
    cout << num << endl;
}
catch (...) {
    cout << "int 以外の何か" << endl;
}
```


## STLと合わせて使う

さあ、本題に入ろう。STLでは例外をバシバシ投げてくる。例えば`stoi()`。

```c++
num = stoi("十一万四千五百十四"); // std::invalid_argument
```

ここで`stoi()`は、文字が突っ込まれたから例外を吐いたというのはなんとなく分かるはずだ。

で、じゃあ`std::invalid_argument`って何だろうか。これは**クラス**だ。中身はこんな感じ。

```c++
class invalid_argument : public logic_error {
public:
    explicit invalid_argument(const string& what_arg);
    explicit invalid_argument(const char* what_arg);
};
```

なんかすごい事になってるので詳しくは語らないことにする。とりあえず`std::invalid_argument`の正体はクラスだったのだ。

つまり、`stoi()`関数の中は大体こんな感じなんだろう。

```c++
std::stoi(std::string str){
    int num;
    num = sugoi_syori(); // strをintに変えちゃうすごい処理

    if (num != num_jyanai()) { // num が数字じゃないとき
        std::invalid_argument exception;
        throw exception;
    }

    // なんかごちゃごちゃした処理
}

```

これを見るだけなら`std::invalid_argument`のインスタンスが飛んできただけである。そうだ、きっとそうなのだ。

正体が分かればこっちのものだ。こいつを`catch`してあげればよい。

`<stdexcept>`をインクルードすることで、この例外を受け取ることができる。
```c++
#include <iostream>
#include <string>
#include <stdexcept>

using namespace std;

int main() {
    int num;
    try {
        num = stoi("十一万四千五百十四");
    }
    catch (invalid_argument e) {
        cout << "文字が突っ込まれました。" << endl;  
    }

    return 0;
}
```

### 実行結果
> 文字が突っ込まれました。

`vector`で、範囲外にアクセスしようとすると`std::out_of_range`が返ってくるので同様に、
```c++
#include <iostream>
#include <vector>
#include <stdexcept>

using namespace std;

int main() {
    int num;
    vector<int> array;
    array.push_back(1);

    try {
        num = array.at(1);
    }
    catch (out_of_range e) {
        cout << "範囲外にアクセスしようとしました。" << endl;  
    }

    return 0;
}
```

### 実行結果
> 範囲外にアクセスしようとしました。

ちなみに、例外関係のクラスはすべて`exception`を基底としている。なので、`exception`を指定するだけでも例外を受け取ることが出来る。

この場合だと`<stdexcept>`は必要ない。

```c++
#include <iostream>
#include <string>

using namespace std;

int main() {
    int num;
    try {
        num = stoi("十一万四千五百十四");
    }
    catch (exception e) {
        cout << "Exception : " << e.what() << endl; // e.what() でエラーの内容が出てくる  
    }

    return 0;
}
```

### 実行結果(環境によって多少異なります)

> Exception : invalid stoi argument

例外を細かく分ける必要がないときはこっちのほうが楽だろう。


## 例外処理の落とし穴

さあ、これで君も例外処理の便利さが分かったかな？

vectorと合わせて使えばもうsegmentation faultの心配はないし、`stoi()`で文字列以外が入った時の処理にもう困らなくていい！

さあ、ガンガン`try-catch`していこう！

### とはならないんじゃ

確かに便利なんだけども、便利なだけあって**処理が遅い**です。

数千から数万回繰り返すようなループの中に例外処理を何も考えずに書くと、プログラム全体が重くなってきます。

普通に条件式で書けるところは書いて、どうしようもなさそうな時に例外処理をしましょう。

<[前に戻る](xx-ExceptionHandling.md) - [目次へ](../README.md) - 次へ進む>
