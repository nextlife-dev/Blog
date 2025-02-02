---
title: public 다형 상속에 대한 이해
description: 
aliases: ["Seungwoo Ham"] 
tags: []
date: 2025-01-30
---
`public`에서 메서드를 선언할 때 `virtual`로 선언하면, 이 메서드는 모든 파생 클래스에서 자동으로 가상이 됩니다. 이때, 파생 클래스에서 기초 클래스의 가상 메서드를 재정의할 수 있습니다.

가상 메서드는 런타임에 올바른 함수 버전을 호출하도록 보장합니다. 이를 ==동적 바인딩== 또는 ==지연 바인딩==이라고 합니다.

```cpp
class Base {
public:
    virtual void foo() { std::cout << "Base::foo()" << std::endl; }
};

class Derived : public Base {
public:
    void foo() override { std::cout << "Derived::foo()" << std::endl; }
};

int main() {
    Base* ptr = new Derived();
    ptr->foo(); // 출력: "Derived::foo()"
    delete ptr;
    return 0;
}
```

이 예시에서 `ptr`은 `Base` 타입이지만, `Derived`객체를 가리킵니다. `foo()` 호출 시 `Dervied::foo()`가 실행되는데, 이는 가상 함수의 동적 바인딩 때문입니다.

자바의 상속 시스템과 유사한 점이, ‘is-a’ 관계를 표현하기 위해 상속을 사용합니다. 그리고 파생 클래스에서 기본 클래스의 메서드를 재정의할 수 있다는 점도 유사합니다.

다만, 자바보다 C++가 더 많은 유연성을 제공하고 복잡성이 많습니다:

- C++ 다중 상속 지원하지만, 자바는 단일 클래스 상속만 허용
- C++ `public`, `protected`, `private` 상속을 지원하지만, 자바는 `public` 상속만 허용
- C++ 명시적으로 `virtual` 키워드를 사용해야 하지만, 자바는 모든 메서드가 기본적으로 가상임
- C++ 순수 가상 함수를 사용하여 구현 가능하지만, 자바는 `interface` 키워드를 사용하여 정의
- 자바는 모든 클래스가 자동으로 `Object` 클래스를 상속받지만, C++는 그런 개념이 없음

## 만약 `virtual` 사용하지 않으면?

C++에서 `virtual` 키워드를 사용하지 않고도 메서드를 재정의할 수 있습니다. 하지만 동적 바인딩이 아니라, ==정적 바인딩==이 발생하며, 기본 클래스 포인터나 참조를 통해 객체에 접근할 때, 실제 객체의 타입에 관계없이 **항상 기본 클래스의 메서드가 호출됩니다.**

```cpp
class Base {
public:
    void print() { std::cout << "Base"; }
};

class Derived : public Base {
public:
    void print() { std::cout << "Derived"; }
};

int main() {
    Base* ptr = new Derived();
    ptr->print(); // 출력: "Base"
    delete ptr;
    return 0;
}
```

이 경우, `ptr->print()`는 `Base::print()`를 호출합니다. `virtual` 키워드를 사용했다면 `Derived::print()`가 호출되었을 것입니다.

따라서, 메서드를 재정의할 수 있지만 `virtual` 키워드 없이는 다형성과 동적 바인딩의 이점을 얻을 수 없습니다. 런타임 다형성이 필요한 경우에는 반드시 `virtual` 키워드를 사용해야 합니다.