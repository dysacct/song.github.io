---
title: Python 双下划线实用方法
date: 2025-07-10 14:42:12
tags:
  - Python双下划线
categories:
  - Python
---

# 五个双下划线方法

```python
class Fruit:
    def __init__(self, *, name: str, grams: float) -> None: # 用*号强制用户必须传入name和grams
        self.name = name
        self.grams = grams

    def __eq__(self, other: Self) -> bool:
        return self.name == other.name and self.grams == other.grams
        return self.__dict__ == other.__dict__

def main() -> None:
    f1: Fruit = Fruit(name="Apple", grams=100.0)
    f2: Fruit = Fruit(name="Orange", grams=150.0)
    f3: Fruit = Fruit(name="Apple", grams=100.0)

    print(f1 == f2) # False
    print(f1 == f3) # False 加了__ep__就是True
# 比较类的内存地址

# __eq__方法,可以比较类的哪些部分


if __name__ == "__main__":
    main()
```

第二种

```python
class Fruit:
    def __init__(self, *, name: str, grams: float) -> None:
        self.name = name
        self.grams = grams

    def __format__(self, format_spec: str) -> str:
        match format_spec:
            case 'kg':
                return f'{self.grams / 1000:.2f}kg'
            case _:
                raise ValueError('Unknown format specifier...')

def main() -> None:
    apple: Fruit = Fruit(name="Apple", grams=2500.0)
    print(f'{apple:kg}')

if __name__ == "__main__":
    main()
```

第三种

```python
class Fruit:
    def __init__(self, *, name: str, grams: float) -> None:
        self.name = name
        self.grams = grams

def main() -> None:
    d1: dict = {1:'a', 2:'b'}
    d2: dict = {3:'c', 4:'d'}
    print(d1 | d2) # {1:'a', 2:'b', 3:'c', 4:'d'}

    s1: set = {1, 2}
    s2: set = {3, 4}
    print(s1 | s2) # {1, 2, 3, 4}

if __name__ == "__main__":
    main()

# 使用类来实现
class Fruit:
    def __init__(self, *, name: str, grams: float) -> None:
        self.name = name
        self.grams = grams

    def __or__(self, other: Self) -> Self:
        new_name: str = f'{self.name} & {other.name}'
        new_grams: float = self.grams + other.grams

        return Fruit(name=new_name, grams=new_grams)

def main() -> None:
    apple: Fruit = Fruit(name="Apple", grams=2500.0)
    orange: Fruit = Fruit(name="Orange", grams=1000.0)
    banana: Fruit = Fruit(name="Banana", grams=1500.0)

    combined: Fruit = apple | orange
    print(combined)

if __name__ == "__main__":
    main()
```
