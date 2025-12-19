
# 📘 CHAPTER 4 - 상태 다이어그램 (State Diagram)

- 객체나 구성 요소의 다양한 상태, 이벤트에 응답하여 상태 전이를 어떻게 수행하는지를 모델링
- 동적인 동작을 가진 복잡한 시스템의 행동을 모델링하는 데 사용
- 시스템이 주고 받는 이벤트를 순서대로 표현

## 상태 다이어그램 구성 요소

<p align="center">
<img width="800" height="1000" alt="image" src="https://github.com/user-attachments/assets/a1af7a75-4d0e-431c-85f7-25c6755d1d8a" />
</p>


|요소|	description |
|--------|-------------|
|State |	 시스템 또는 객체가 가질 수 있는 다양한 상태를 표시|
|Event |	 시스템 또는 객체가 상태 변화를 일으키는 사건|
|Transition |	 상태 간의 변화|


|state operation | description |
|--------|-------------|
|entry |	 state에 들어오는 경우 실행하는 action|
|do |	 다른 상태로 나갈 때 실행하고 나가는 action|
|exit|	 현재 state를 유지하고 있을 때 실행하는 action|


<details>
    <summary>예시</summary>

<p align="center">
<img width="600" height="203" alt="image" src="https://github.com/user-attachments/assets/a01bd009-ac57-4394-bb51-5c448ac30baf" />
</p>

상기 그림에서 S1 상태로 진입하게 된다면 entry에 의해 x = 1로 설정된다.

그리고 event가 발생한다고 가정하자.

event 조건 x <= 2를 만족하기 때문에 S1 상태에서 exit 하게 되어 x++에 의해 x = 2가 된다.

그리고 event의 action으로 x = x * 2가 되어 x = 4가 된다.

event에 의해 S2 상태로 진입하였으므로, entry x += 5에 의해 최종적으로 x = 9가 된다.

</details>

## OR State

한 시점에 하나의 하위 상태만 활성화됨

**📌 상태 다이어그램 개념**

상위 상태: Machine

하위 상태: Idle, Running, Error

동시에 하나만 존재 가능

<p align="center">
<img width="250" height="1050" alt="image" src="https://github.com/user-attachments/assets/1945c996-487c-4fa6-b312-0a8e94f2bf99" />
</p>

<details>
    <summary>C++ 코드</summary>
    
```cpp
#include <iostream>

class Machine {
public:
    enum class State {
        Idle,
        Running,
        Error
    };

    Machine() : currentState(State::Idle) {}

    void handleEvent(int event) {
        switch (currentState) {
        case State::Idle:
            if (event == 1) {
                currentState = State::Running;
            }
            break;

        case State::Running:
            if (event == 2) {
                currentState = State::Error;
            }
            break;

        case State::Error:
            if (event == 3) {
                currentState = State::Idle;
            }
            break;
        }
    }

    void printState() const {
        switch (currentState) {
        case State::Idle:
            std::cout << "Idle\n";
            break;
        case State::Running:
            std::cout << "Running\n";
            break;
        case State::Error:
            std::cout << "Error\n";
            break;
        }
    }

private:
    State currentState;  // OR state
};
```

</details>

## AND State

여러 하위 상태가 동시에 활성화됨

📌 상태 다이어그램 개념

상위 상태: System

병렬 상태 영역:

MotorState : On / Off

DoorState : Open / Closed

<p align="center">
<img width="400" height="1125" alt="image" src="https://github.com/user-attachments/assets/40e66266-1396-4a38-b550-cba8789cc699" />
</p>

<details>
    <summary>C++ 코드</summary>
    
```cpp
#include <iostream>

class System {
public:
    enum class MotorState {
        On,
        Off
    };

    enum class DoorState {
        Open,
        Closed
    };

    System()
        : motorState(MotorState::Off),
          doorState(DoorState::Closed) {}

    void motorOn() {
        motorState = MotorState::On;
    }

    void motorOff() {
        motorState = MotorState::Off;
    }

    void openDoor() {
        doorState = DoorState::Open;
    }

    void closeDoor() {
        doorState = DoorState::Closed;
    }

    void printState() const {
        std::cout << "Motor: "
                  << (motorState == MotorState::On ? "On" : "Off")
                  << ", Door: "
                  << (doorState == DoorState::Open ? "Open" : "Closed")
                  << '\n';
    }

private:
    MotorState motorState;  // AND state ①
    DoorState doorState;   // AND state ②
};
```

</details>

---
