
# 📘 CHAPTER 2 - 패키지 다이어그램 (Package Diagram)

- 패키지 : UML의 구성 요소를 더 높은 수준에서 묶을 수 있는 단위
- 다양한 다이어그램에서 활용이 가능하며 상위 수준 구조 분석에 용이함
- 패키지 다이어그램은 정적인 구조를 표현
- 전체 모듈의 구조적 관계 파악에 용이
- 관련된 것들을 묶어서 표현하기에 용이

<사진첨부>

## 패키지 간 관계

<사진 첨부>

패키지 간 관계도 의존, 상속등으로 표현할 수 있으며, 스테레오 타입을 사용할 수 있음

`global의 경우 스테레오 타입을 설정하여 모든 패키지 간 연결이 되어있음을 명시할 수 있음`

| UML 개념 | C++ 표현 |
|----------|---------|
|Package	|디렉토리 + `namespace`|
|Dependency	|`#include`|
|<<global>>	| static utility / 공용 모듈 |
|api → app |	Controller → Service |
|app → domain |	Service → Entity |

<details>
    <summary>구조 및 C++ 코드</summary>

```cpp
// 디렉토리 구조
project/
├── api/
│   └── MemberController.h
├── member/
│   ├── app/
│   │   └── MemberService.h
│   └── domain/
│       └── Member.h
└── common/
    └── Logger.h

// <<global>> common 패키지
#pragma once
#include <string>
#include <iostream>

namespace common {

class Logger {
public:
    static void info(const std::string& msg) {
        std::cout << "[INFO] " << msg << std::endl;
    }
};

}

// domain 패키지
#pragma once
#include <string>

namespace member::domain {

class Member {
public:
    Member(int id, std::string name)
        : id_(id), name_(std::move(name)) {}

    int id() const { return id_; }
    const std::string& name() const { return name_; }

private:
    int id_;
    std::string name_;
};

}

// app 패키지
#pragma once
#include "member/domain/Member.h"
#include "common/Logger.h"

namespace member::app {

class MemberService {
public:
    member::domain::Member createMember(int id, const std::string& name) {
        common::Logger::info("Create Member");
        return member::domain::Member(id, name);
    }
};

}

// api 패키지
#pragma once
#include "member/app/MemberService.h"
#include "common/Logger.h"

namespace api {

class MemberController {
public:
    void create() {
        common::Logger::info("API Request");
        member::app::MemberService service;
        auto member = service.createMember(1, "Hyunseok");
    }
};

}
```

</details>
---
