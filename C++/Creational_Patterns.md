
# 📘 생성 패턴 (Creational Patterns)

- 객체 생성 방식을 캡슐화하여 코드 결합도를 낮추고 객체 생성 로직을 유연하게 만드는 패턴

## 싱글톤 (Singleton)

> **특정 클래스에 대해 객체 인스턴스가 하나만 만들어지도록 하고, 전역 접근을 제공하는 패턴**
>
> **전역 변수에 객체를 대입하면 Application이 시작되고 종료되기까지 자원을 차지하지만, 
  싱글턴 패턴은 필요할 때만 객체를 만들 수 있다.**
>
> **실제로 객체가 필요하면 인스턴스를 직접 만들지 않고, 인스턴스를 요청하도록 구현한다.**

### ✅ 장점

- 인스턴스의 유일성 보장
- 싱글톤 인스턴스에 대한 전역 접근 제공
- 필요할 때 생성되는 지연 초기화(Lazy Initialization) 가능

### ❌ 단점

- 단일 책임 원칙(SRP) 위반 가능
- 멀티 스레드 환경에서 동기화 고려 필요
- 상속이 어렵거나 제한적
- 전역 상태 관리로 인해 의존성 증가 및 테스트 어려움

### C++ 예제

```cpp
#include <iostream>

class Singleton {
private:
    Singleton() {}  // 생성자 private

public:
    static Singleton& getInstance() {
        static Singleton instance;
        return instance;
    }

    void print() {
        std::cout << "Singleton instance\n";
    }

    // 복사 방지
    Singleton(const Singleton&) = delete;
    Singleton& operator=(const Singleton&) = delete;
};

int main() {
    Singleton& s1 = Singleton::getInstance();
    Singleton& s2 = Singleton::getInstance();

    s1.print();
    s2.print();
}
```

---

## 팩토리 메서드 (Factory Method)

> **객체를 생성하는 과정을 서브 클래스에서 결정할 수 있도록 하는 패턴**
>
> **객체 생성에 대한 구체적인 구현을 서브 클래스로 미룬다.**
>
> **클라이언트는 객체 생성에 대한 구체적인 클래스를 알 필요 없이, 추상 클래스를 통해 인터페이스에만 의존할 수 있다.**

### ✅ 장점

- SRP(단일 책임 원칙), 제품 생성 코드를 한 곳에서만 관리하여 유지보수가 쉽다.
- OCP(개방-폐쇄 원칙), 객체 생성 코드를 변경하거나 확장할 때, 기존 코드를 수정하지 않고 새로운 제품을 추가한다.
- DIP(의존 역전 원칙), 객체 생성을 위한 팩토리가 추상화되어 있어 구체적인 클래스에 의존되지 않는다.
- 클라이언트는 구체적인 클래스에 의존하지 않고 추상 클래스에 의존하므로 유연성이 높아진다.


### ❌ 단점

- 클래스의 수가 늘어날 수 있고, 클래스 간의 관계가 복잡해진다.
- 각각의 Concrete Creator 클래스마다 Concrete Product를 생성하는 메서드를 구현하므로 코드 중복이 발생한다.

### C++ 예제

```cpp
#include <iostream>
using namespace std;

class Product {
public:
    virtual void use() = 0;
    virtual ~Product() {}
};

class ConcreteProductA : public Product {
public:
    void use() override {
        cout << "Using Product A\n";
    }
};

class ConcreteProductB : public Product {
public:
    void use() override {
        cout << "Using Product B\n";
    }
};

class Factory {
public:
    static Product* createProduct(int type) {
        if (type == 1) return new ConcreteProductA();
        if (type == 2) return new ConcreteProductB();
        return nullptr;
    }
};

int main() {
    Product* p = Factory::createProduct(1);
    p->use();
    delete p;
}
```
---
## 추상 팩토리 (Abstract Factory)

> **관련 있는 객체들의 생성을 추상화하여 객체 생성을 캡슐화하는 방법을 제공하는 패턴**
>
> **클라이언트가 구체적인 클래스를 알지 못해도, 해당 클래스와 상호작용을 할 수 있도록 도와준다.**

### ✅ 장점

- SRP(단일 책임 원칙), 제품 생성 코드를 한 곳에서만 관리하여 유지보수가 쉽다.
- OCP(개방-폐쇄 원칙), 제품군을 추가/변경할 때, 클라이언트 코드를 수정하지 않고 팩토리만 수정하면 된다.
- 클라이언트는 구체적인 제품 클래스를 알 필요가 없으며, 팩토리를 통해 객체를 생성할 수 있다.
- 팩토리에서 생성되는 제품들은 상호 호환이 가능하다.

### ❌ 단점

- 새로운 제품군을 추가할 때마다 추상 팩토리, 구체적인 팩토리, 제품 클래스들을 모두 추가해야 한다.
  또한, 인터페이스도 변경되어야 하므로 제품군이 자주 변경되는 경우 유연성이 떨어진다.
- 제품군이 복잡한 경우 클래스의 수가 급격히 증가한다.


### C++ 예제

```cpp
#include <iostream>
using namespace std;

class Button {
public:
    virtual void paint() = 0;
};

class WinButton : public Button {
public:
    void paint() override { cout << "Windows Button\n"; }
};

class LinuxButton : public Button {
public:
    void paint() override { cout << "Linux Button\n"; }
};

class GUIFactory {
public:
    virtual Button* createButton() = 0;
};

class WinFactory : public GUIFactory {
public:
    Button* createButton() override {
        return new WinButton();
    }
};

class LinuxFactory : public GUIFactory {
public:
    Button* createButton() override {
        return new LinuxButton();
    }
};

int main() {
    GUIFactory* factory = new WinFactory();
    Button* btn = factory->createButton();
    btn->paint();

    delete btn;
    delete factory;
}
```
---

## 빌더 (Builder)

> **객체 생성 과정을 추상화하여 복잡한 객체를 조립하는 패턴**
>
> **객체를 생성하는 방법을 클라이언트로부터 숨기고, 생성 과정을 단계적으로 나눈다.**

### ✅ 장점

- SRP(단일 책임 원칙), 제품 생성 코드를 분리할 수 있다.
- 객체의 생성 프로세스가 명확하게 표현되어 가독성이 향상된다.
- Director 클래스가 객체 생성 과정을 제어하기 때문에, 객체의 구성을 변경하거나 다양한 방식으로 조립할 수 있다.

### ❌ 단점

- 클래스의 수가 증가한다.
- 객체 생성이 단순한 경우, 빌더 패턴을 적용하면 오버헤드가 증가한다.
- 팩토리 메서드보다 클라이언트가 객체에 대해 더 많이 알아야 한다.

### C++ 예제

```cpp
#include <iostream>
using namespace std;

class Computer {
public:
    string cpu;
    string ram;
    string storage;

    void show() {
        cout << cpu << ", " << ram << ", " << storage << endl;
    }
};

class ComputerBuilder {
private:
    Computer computer;

public:
    ComputerBuilder& setCPU(string cpu) {
        computer.cpu = cpu;
        return *this;
    }

    ComputerBuilder& setRAM(string ram) {
        computer.ram = ram;
        return *this;
    }

    ComputerBuilder& setStorage(string storage) {
        computer.storage = storage;
        return *this;
    }

    Computer build() {
        return computer;
    }
};

int main() {
    Computer pc = ComputerBuilder()
                    .setCPU("i7")
                    .setRAM("16GB")
                    .setStorage("1TB")
                    .build();

    pc.show();
}
```
---
## 프로토타입 (Prototype)

> **초기 객체 생성 비용이 많이 드는 경우 사용하는 패턴**
>
> **객체를 생성할 때 기존 객체의 복사를 통해 생성한다.**
>
> **생성할 객체의 유형에 대한 세부 정보를 숨기면서 복잡한 객체를 만들 수 있는 방법을 제공한다.**

### ✅ 장점

- 객체를 생성할 때, 기존 객체를 복사하기 때문에 간단하게 객체를 생성한다.
- 클라이언트가 객체 생성에 필요한 정보를 구체적으로 알 필요가 없다.
- 최초 객체 생성 비용이 많은 경우, 복사를 통해 객체를 만드는 것이 효율적이다.

### ❌ 단점

- 객체가 다른 객체들과 복잡한 관계에 있다면, 객체의 복사 기능 구현이 복잡할 수 있다.

### C++ 예제

```cpp
#include <iostream>
using namespace std;

class Prototype {
public:
    virtual Prototype* clone() = 0;
    virtual void print() = 0;
};

class ConcretePrototype : public Prototype {
private:
    int value;

public:
    ConcretePrototype(int v) : value(v) {}

    Prototype* clone() override {
        return new ConcretePrototype(*this);
    }

    void print() override {
        cout << "Value: " << value << endl;
    }
};

int main() {
    Prototype* original = new ConcretePrototype(10);
    Prototype* copy = original->clone();

    original->print();
    copy->print();

    delete original;
    delete copy;
}
```
---
