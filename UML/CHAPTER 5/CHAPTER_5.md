
# 📘 CHAPTER 5 - 활동 다이어그램 (Activity Diagram)

- 로직, 절차, 흐름을 기술 (플로우 차트와 유사)
- 업무 프로세스, 코드 실행 로직을 표현할 때 주로 사용

<사진첨부>

<details>
    <summary>C++ 코드</summary>

```cpp
#include <iostream>

class SignUpActivity {
public:
    void start() {
        initialNode();
        inputBasicInfo();
        inputPaymentInfo();
        requestSignUp();
        finalNode();
    }

private:
    void initialNode() {
        std::cout << "[Initial Node] 시작\n";
    }

    void inputBasicInfo() {
        std::cout << "[Action] 기본 정보 입력\n";
    }

    void inputPaymentInfo() {
        std::cout << "[Action] 결제 정보 입력\n";
    }

    void requestSignUp() {
        std::cout << "[Action] 회원 가입 요청\n";
    }

    void finalNode() {
        std::cout << "[Final Node] 종료\n";
    }
};

int main() {
    SignUpActivity activity;
    activity.start();
    return 0;
}
```
</details>

## 📌 분기 처리

<사진첨부>

- Decision Node에서 조건에 따라 흐름이 분기

- 각 분기에는 Guard Condition이 붙음

- 분기된 흐름은 Merge Node에서 다시 하나로 합류 (대부분 구현하지 않음)

<details>
    <summary>C++ 코드</summary>

```cpp
#include <iostream>
#include <string>

// 조건 값 가정용 함수들
bool isTokenValid() {
    return true; // false 로 바꾸면 "유효시간 지남" 흐름
}

bool isCorporateMember() {
    return false; // true = 기업회원
}

int main() {
    // Initial Node
    std::cout << "[Initial Node] 시작\n";

    // Action: 토큰 값 조회
    std::cout << "[Action] 토큰 값 조회\n";

    // Decision Node 1: 토큰 유효성
    if (!isTokenValid()) {  // [유효시간 지남]
        std::cout << "[Guard] 유효시간 지남\n";
        std::cout << "[Action] 오류 안내 메일 발송\n";
        std::cout << "[Final Node] 종료\n";
        return 0;
    }

    // [유효함]
    std::cout << "[Guard] 유효함\n";
    std::cout << "[Action] 가입 승인\n";

    // Decision Node 2: 기업회원 여부
    if (isCorporateMember()) {  // [기업회원]
        std::cout << "[Guard] 기업회원\n";
        std::cout << "[Action] 청구 대상 추가\n";
    } else {                    // [else]
        std::cout << "[Guard] 일반회원\n";
        std::cout << "[Action] 자동이체 등록\n";
    }

    // Merge Node
    std::cout << "[Merge Node] 처리 결과 병합\n";

    // Action: 완료 안내 메일
    std::cout << "[Action] 완료 안내 메일 발송\n";

    // Final Node
    std::cout << "[Final Node] 종료\n";

    return 0;
}
```
</details>

## 📌 병렬 처리

<사진첨부>

- Fork Node에서 하나의 흐름을 여러 개의 병렬 흐름으로 분기

- 각 Action은 동시에 실행

- Join Node에서 모든 병렬 작업이 끝날 때까지 대기

<details>
    <summary>C++ 코드</summary>
    
```cpp
#include <iostream>
#include <thread>

// Action: 완료 이메일 발송
void sendCompletionEmail() {
    std::cout << "[Action] 완료 이메일 발송\n";
}

// Action: 가입 포인트 지급
void giveSignupPoint() {
    std::cout << "[Action] 가입 포인트 지급\n";
}

int main() {
    // Initial Node
    std::cout << "[Initial Node] 시작\n";

    // Action: 회원 등록
    std::cout << "[Action] 회원 등록\n";

    // ===== Fork Node =====
    std::cout << "[Fork Node] 병렬 처리 시작\n";

    std::thread emailThread(sendCompletionEmail);
    std::thread pointThread(giveSignupPoint);

    // ===== Join Node =====
    emailThread.join();
    pointThread.join();

    std::cout << "[Join Node] 병렬 처리 종료\n";

    // Final Node
    std::cout << "[Final Node] 종료\n";

    return 0;
}

```

</details>

## 📌 파티션 처리

<사진첨부>

- 파티션은 “누가 이 작업을 수행하는가” 를 표현

- 같은 흐름이라도 책임 주체에 따라 구분

- 시스템 경계, 역할 분리를 명확히 함

<details>
    <summary>C++ 코드</summary>
    
```cpp
#include <iostream>

class Operator;  // forward declaration

class Customer {
public:
    void submitApplication(Operator& op);
    void submitAdditionalDocuments() {
        std::cout << "[고객] 추가 서류 제출\n";
    }
};

class Operator {
public:
    void reviewEligibility() {
        std::cout << "[운영자] 대상 여부 검토\n";
    }

    void registerAndNotify(Customer& customer) {
        std::cout << "[운영자] 대상 등록 및 통지\n";
        customer.submitAdditionalDocuments();
    }
};

void Customer::submitApplication(Operator& op) {
    std::cout << "[고객] 신청서 제출\n";
    op.reviewEligibility();
    op.registerAndNotify(*this);
}

int main() {
    std::cout << "[Initial Node] 시작\n";

    Customer customer;
    Operator operatorUser;

    customer.submitApplication(operatorUser);

    std::cout << "[Final Node] 종료\n";
    return 0;
}
```

</details>

---
