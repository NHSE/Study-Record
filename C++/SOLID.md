
# 📘 SOLID 원칙

- SOLID 원칙은 객체지향 설계에서 유지보수성과 확장성을 높이기 위해 꼭 지켜야 할 다섯 가지 원칙

## 단일 책임 원칙 (Single Responsibility Principle, SRP)

> **하나의 클래스는 하나의 책임만 가져야 한다**  

### ❌ 단일 책임 원칙을 어긴 C++ 예제

```cpp
#include <iostream>

class Report {
public:
    void createReport() {
        std::cout << "Report created\n";
    }

    void saveToFile() {
        std::cout << "Report saved to file\n";
    }

    void printReport() {
        std::cout << "Printing report\n";
    }
};
```

👉 하나의 클래스가 여러 책임을 가짐
👉 변경 이유가 여러 개 → SRP 위반

### ✅ 단일 책임 원칙을 지킨 C++ 예제
1. 생성
```cpp
#include <iostream>

class Report {
public:
    void create() {
        std::cout << "Report created\n";
    }
};
```

2. 저장
```cpp
class ReportSaver {
public:
    void save(const Report& report) {
        std::cout << "Report saved to file\n";
    }
};
```
3. 출력
```cpp
class ReportPrinter {
public:
    void print(const Report& report) {
        std::cout << "Printing report\n";
    }
};
```
---

## 개방-폐쇄 원칙 (Open-Closed Principle, OCP)

> **확장에는 열려 있고(Open)**  **변경에는 닫혀 있어야(Closed) 한다**


👉 **기존 코드를 수정하지 않고 기능을 추가할 수 있어야 한다**

### ❌ 개방-폐쇄 원칙을 어긴 C++ 예제

```cpp
#include <iostream>

class Shape {
public:
    enum Type { CIRCLE, RECTANGLE };
    Type type;

    Shape(Type t) : type(t) {}

    void draw() {
        if (type == CIRCLE) {
            std::cout << "Draw Circle\n";
        }
        else if (type == RECTANGLE) {
            std::cout << "Draw Rectangle\n";
        }
    }
};
```

👉 새로운 도형 추가 시 → draw() 내부 if / else 수정 필요
👉 기능 추가 = 기존 코드 변경

### ✅ 개방-폐쇄 원칙을 지킨 C++ 예제
1. 공통 인터페이스(추상 클래스)
```cpp
class Shape {
public:
    virtual void draw() const = 0;
    virtual ~Shape() = default;
};
```

2. 기능 확장 (기존 코드 수정 없음)
```cpp
#include <iostream>

class Circle : public Shape {
public:
    void draw() const override {
        std::cout << "Draw Circle\n";
    }
};
```

```cpp
class Rectangle : public Shape {
public:
    void draw() const override {
        std::cout << "Draw Rectangle\n";
    }
};
```
3. 사용 코드
```cpp
void render(const Shape& shape) {
    shape.draw();
}
```

👉 새로운 도형 추가 시 → Triangle, Hexagon 클래스만 추가

👉 확장만 있고 변경은 없음 ✅

## 리스코프 치환 원칙 (Liskov Substitution Principle, LSP)

> **자식 클래스는 언제나 부모 클래스를 대체할 수 있어야 한다**
 
👉 **부모 타입을 사용하는 코드가 자식 타입으로 바뀌어도 동작이 깨지면 안 된다**

### ❌ 리스코프 원칙을 어긴 C++ 예제

```cpp
class Bird {
public:
    virtual void fly() {
        // 새는 날 수 있다
    }
};
```

```cpp
class Penguin : public Bird {
public:
    void fly() override {
        // 펭귄은 날 수 없음
        throw std::runtime_error("Penguins can't fly");
    }
};
```

```cpp
void makeBirdFly(Bird& bird) {
    bird.fly();  // Penguin 전달 시 예외 발생
}
```

👉 Bird를 사용하는 코드는 fly()가 가능하다고 가정 → **Penguin은 그 계약을 깨뜨림**

### ✅ 리스코프 원칙을 지킨 예제
1. 역할을 명확히 분리
```cpp
class Bird {
public:
    virtual ~Bird() = default;
};

class FlyingBird : public Bird {
public:
    virtual void fly() = 0;
};
```

2. 올바른 상속 구조
```cpp
class Sparrow : public FlyingBird {
public:
    void fly() override {
        // 정상 비행
    }
};

class Penguin : public Bird {
    // 날지 않음
};
```

3. 사용 코드
```cpp
void makeBirdFly(FlyingBird& bird) {
    bird.fly();  // 모든 FlyingBird는 안전
}
```
👉 FlyingBird 타입은 fly 가능이라는 계약을 가짐 → **행동 규약이 깨지지 않음**

## 인터페이스 분리 원칙 (Interface Segregation Principle, ISP)

> **클라이언트는 자신이 사용하지 않는 메서드에 의존하도록 강요받아서는 안 된다**
 
👉 **큰 인터페이스 하나보다 작고 목적이 명확한 인터페이스 여러 개가 낫다**

### ❌ 인터페이스 분리 원칙을 어긴 C++ 예제

```cpp
class Machine {
public:
    virtual void print() = 0;
    virtual void scan() = 0;
    virtual void fax() = 0;
};
```
```cpp
class Printer : public Machine {
public:
    void print() override {
        // 프린트 기능
    }

    void scan() override {
        // 사용하지 않음
    }

    void fax() override {
        // 사용하지 않음
    }
};
```
👉 Printer는 print만 필요 → **사용하지 않는 기능에 의존**

### ✅ 인터페이스 분리 원칙을 지킨 예제
1. 역할별 인터페이스 분리
```cpp
class Printable {
public:
    virtual void print() = 0;
    virtual ~Printable() = default;
};

class Scannable {
public:
    virtual void scan() = 0;
    virtual ~Scannable() = default;
};

class Faxable {
public:
    virtual void fax() = 0;
    virtual ~Faxable() = default;
};
```

2. 필요한 인터페이스만 구현
```cpp
class Printer : public Printable {
public:
    void print() override {
        // 프린트 기능
    }
};

class MultiFunctionMachine 
    : public Printable, public Scannable, public Faxable {
public:
    void print() override { }
    void scan() override { }
    void fax() override { }
};
```

👉 각 클래스는 필요한 기능만 의존 → **결합도** 감소

## 의존 역전 원칙 (Dependency Inversion Principle, DIP)

> **고수준 모듈은 저수준 모듈에 의존하면 안 된다**  
> **둘 다 추상화(인터페이스)에 의존해야 한다**  
>  
> **추상화는 세부사항에 의존하면 안 되고  
> 세부사항이 추상화에 의존해야 한다**

## ❌ 의존 역전 원칙을 어긴 C++ 예제

### 저수준 구현에 직접 의존

```cpp
#include <iostream>

class FileLogger {
public:
    void log(const std::string& message) {
        std::cout << "File Log: " << message << "\n";
    }
};

class Application {
public:
    Application() {
        logger = new FileLogger();  // 직접 생성
    }

    void run() {
        logger->log("Application started");
    }

private:
    FileLogger* logger;
};
```

👉 **Application**(고수준)이 **FileLogger**(저수준 구현)에 직접 의존 → DIP 위반

### ✅ 의존 역전 원칙을 지킨 C++ 예제
1. 추상화(인터페이스) 정의
```cpp
class Logger {
public:
    virtual void log(const std::string& message) = 0;
    virtual ~Logger() = default;
};
```

2. 저수준 구현은 추상화에 의존
```cpp
#include <iostream>

class FileLogger : public Logger {
public:
    void log(const std::string& message) override {
        std::cout << "File Log: " << message << "\n";
    }
};

class ConsoleLogger : public Logger {
public:
    void log(const std::string& message) override {
        std::cout << "Console Log: " << message << "\n";
    }
};
```

3. 고수준 모듈도 추상화에 의존
```cpp
class Application {
public:
    Application(Logger& logger) : logger(logger) {}

    void run() {
        logger.log("Application started");
    }

private:
    Logger& logger;
};
```
4. 사용 코드
```cpp
int main() {
    FileLogger fileLogger;
    Application app(fileLogger);
    app.run();
}
```
👉 **Application**은 **Logger**인터페이스만 앎 → **의존성**이 역전됨

---
