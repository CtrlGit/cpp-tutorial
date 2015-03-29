# クラスの基礎 練習問題 回答
## I

```cpp
#include <iostream>
using namespace std;

class Car {
public:
	int number;
	int capacity;
	void show();
};

void Car::show() {
	cout << "この車のナンバーは" << number << "で" << capacity << "人乗りです。\n";
}

int main() {
	return 0;
}
```

## II
`main()`関数より上の部分は I と同じなので省略。

```cpp
int main() {
	Car c;
	c.number = 114514;
	c.capacity = 810;
	c.show();

	return 0;
}
```

> この車のナンバーは114514で810人乗りです。

[戻る](03-ClassIntro.md)
