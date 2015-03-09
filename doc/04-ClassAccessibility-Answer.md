# クラスのアクセス制限とファイル分割 練習問題 回答
## I

```cpp
//
// Car.h
//

class Car {
public:
	int number;
	int capacity;
	void show();
};
```

```cpp
//
// Car.cpp
//

#include <iostream>
#include "Car.h"

void Car::show() {
	cout << "この車のナンバーは" << number << "で" << capacity << "人乗りです。\n";
}
```

## II

```cpp
//
// Car.h
//

class Car {
private:
	int number;
	int capacity;

public:
	void setNumber(int n);
	void setCapacity(int c);
	int getNumber();
	int getCapacity();
	void show();
};
```

```cpp
//
// Car.cpp
//

#include <iostream>
#include "Car.h"

void Car::setNumber(int n) {
	number = n;
}

void Car::setCapacity(int c) {
	if (c < 1) {
		cout << "キャパおかしいだろ！いい加減にしろ！\n";
		capacity = 1;
	} else {
		capacity = c;
	}
}

int Car::getNumber() {
	return number;
}

int Car::getCapacity() {
	return capacity;
}

void Car::show() {
	cout << "この車のナンバーは" << number << "で" << capacity << "人乗りです。\n";
}
```

[戻る](04-ClassAccessibility.md)
