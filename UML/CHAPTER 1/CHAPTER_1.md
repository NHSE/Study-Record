
# 📘 CHAPTER 1 - 클래스 다이어그램 (Class Diagream)

- 시스템의 클래스와 클래스들 간의 관계를 보여준다.
- 코드 수준의 모든 구성 요소들을 속성들과 오퍼레이션들과 함께 나열한다.
- 제한된 타입 정보를 제공하며, 메서드를 어떻게 작성해야 할지를 나타내지는 않는다.
- Use Case의 명사가 클래스가 되면, 동사들은 메서드가 될 수 있다.


## 구조

클래스 다이어그램은 다음과 같은 구조를 가지고 있다. (클래스 이름 / 속성 / 오퍼레이션)

`(참고 : 오퍼레이션은 realization이 아닌 선언 정도, 메서드는 특정 언어로 구현된 실제 코드를 의미)`

<사진첨부>

operation의 밑줄은 static 메서드를 의미하고 기울임 글꼴은 추상 메서드를 의미한다.

attribute 앞의 기호는 visibility를 나타낸다.
| 기호 | 속성 |
|-----|----|
| + | public |
| - | private |
| # | protected |
| ~ | package/internal |

인터페이스와 추상 클래스는 다음과 같이 나타낼 수 있다.

<사진첨부>

##  클래스 관계 (Class Relationships)

클래스 다이어그램에서 클래스 간의 관계는 다양한 형태가 있다.

<사진첨부>

### 📌 Dependency (의존 관계)

클래스가 다른 클래스의 객체를 매개변수로 받거나, 다른 클래스의 멤버 변수를 참조할 때 발생하는 경우,

클래스가 다른 클래스에 의존할 때 의존성이 발생한다.

`classB`는 `FuncB`을 실행할 때, classA를 인자로 받아 `FuncA`을 실행한다.

`classA`는 독립적으로 존재할 수 있고, classB는 classA의 인스턴스를 사용하여 작업을 수행한다.

이런 경우, `classB`가 `classA`에 의존한다고 할 수 있다.

<사진첨부>

`FuncA`이 호출되고 나면, `classA`와 `classB`의 관계는 사라지게 된다.

즉, `classB`는 `classA`의 정보를 볼 수 없다.

<details>
    <summary>C++ 코드</summary>

```cpp
#include <iostream>

using namespace std;

class ClassA
{
public:
	void doSomething() { cout << "ClassA is doing something." << endl; }
};

class ClassB
{
public:
	void performAction(ClassA* a) { a->doSomething(); }
};

int main()
{
	ClassA* objA = new ClassA();
	ClassB* objB = new ClassB();

	objB->performAction(objA);

	return 0;
}
```

</details>


### 📌 Realization, Implementation (실현 관계)

실현 관계는 추상 클래스 또는 인터페이스와 클래스 간의 관계를 의미한다.

클래스가 순수 가상함수(=인터페이스의 동작)들을 실제로 구현할 때 사용된다.

<details>
    <summary>C++ 코드</summary>

```cpp
#include <iostream>

using namespace std;

class Interface
{
public:
	virtual void operation() = 0;
};

class Concrete : public Interface
{
public:
	void operation() override { cout << "Operation Implementation." << endl; }
};

int main()
{
	Concrete* concrete = new Concrete();
	concrete->operation();

	return 0;
```

</details>

### 📌 Association, Navigable Association (연관, 방향성)

연관 관계의 경우 두 클래스 사이의 관계를 보여주며, 방향성을 나타내지 않는다. (양방향 정보 전달)

화살표가 있는 경우(Navigability), 어떤 클래스가 다른 클래스의 인스턴스를 탐색할 수 있는 관계를 나타낸다.

또한 연관 관계는 객체 간의 의존성을 나타내며, 객체 간의 수명 주기의 종속성은 없다.

<사진첨부>

Teacher → Student인 경우, Teacher가 Student의 public 속성이나 operation에 접근할 수 있다.

<details>
    <summary>C++ 코드</summary>

```cpp
class Student { ... };

class Teacher 
{
private:
	vector<Student> students;
public:
	Teacher() = default;
	void addStudent(Student s) { students.push_back(s); }
};
```

</details>

### 📌 Inheritance, Generalization (상속, 일반화)

상속 관계는 한 클래스가 다른 클래스의 특성과 동작을 상속받는 것을 의미한다.

예를 들어, 동물 클래스와 개 클래스는 상속 관계다.

<사진첨부>

<details>
    <summary>C++ 코드</summary>

```cpp
#include <iostream>

using namespace std;

class Animal
{
public:
	void makeSound() { cout << "Some sound" << endl; }
};

class Dog : public Animal
{
public:
	void wagTail() { cout << "Tail wagging" << endl; }
};

int main()
{
	Dog* dog = new Dog();

	dog->makeSound(); 
	dog->wagTail();   

	return 0;
}
```

</details>

### 📌 Aggregation (집합)

Aggregation 관계는 한 객체가 다른 객체를 포함하고, 포함된 객체의 수명 주기는 독립적인 경우에 사용한다.

만약 객체가 사라질 때, 포함된 객체도 같이 사라진다면 Composition 관계가 된다.

집합 관계는 HAS-A 관계이기도 하며, 다중성을 통해 표현된다.

대표적으로 School이 여러 명의 Student를 가지고 있을 수 있다.

`School 객체가 메모리에서 사라진다고 해서 학생의 메모리가 해제되지 않는다`

<사진첨부>

<details>
    <summary>C++ 코드</summary>

```cpp
#include <iostream>
#include <string>
#include <vector>

using namespace std;

class Student
{
public:
	Student(string n) : name(n) {}
	string getName() const { return name; }
private:
	string name;
};

class School
{
public:
	School(string n) : name(n) {}
	void addStudent(Student* student) { students.push_back(student); }
private:
	string name;
	vector<Student*> students;
};

int main()
{
	Student* student1 = new Student("blood");
	Student* student2 = new Student("straw");
	School* school = new School("High School");

	school->addStudent(student1);
	school->addStudent(student2);

	delete school;

	cout << student1->getName() << endl;
	cout << student2->getName() << endl;

	return 0;
}
```

</details>

### 📌 Composition (합성)

합성은 Aggregation(집합)의 특별한 형태로, 더 강한 종속성을 나타낸다.

한 객체의 수명 주기가 다른 객체에 의해 관리되는 경우에 사용한다.

대표적으로 자동차가 엔진을 가지는 관계가 예시가 될 수 있다.

`자동차가 사라지면 엔진도 같이 사라진다`

<사진첨부>

<details>
    <summary>C++ 코드</summary>

```cpp
#include <iostream>

using namespace std;

class Engine 
{
public:
	void start() { cout << "Engine started." << endl; }
};

class Car
{
private:
	Engine engine; 
public:
	void start()
	{
		engine.start();
		cout << "Car started." << endl;
	}
};

int main()
{
	Car* car = new Car();
	car->start();

	delete car;

	return 0;
}
```

</details>

### 📌 Multiplicity, Cardinality On Relations (다중성)

클래스 간의 연관 관계에서 각 인스턴스 간의 관계 중 객체 간의 개수나 범위를 나타낼 수 있다.

예를 들어, Customer는 여러 개의 Ticket을 가지고 있고, Student는 하나 이상의 Course를 가진다.

<사진첨부>
---