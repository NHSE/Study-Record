
# 📘 CHAPTER 6 - 컴포넌트 다이어그램 (Component Diagram)

- 컴포넌트 : 시스템의 기능을 캡슐화하고, 명확하게 정의된 인터페이스로 다른 컴포넌트와 상호작용하는 독립적인 단위
- 컴포넌트는 시스템의 기능적 단위이며, 독립적으로 배포 가능하고 재사용 가능
- 클래스는 상태와 동작을 정의, 컴포넌트는 시스템의 더 큰 단위로 여러 클래스와 리소스, 더 높은 수준의 추상화를 포함
- 각 시스템의 물리적 구성 요소 간의 의존성 표현

<p align="center">
<img width="500" height="640" alt="image" src="https://github.com/user-attachments/assets/e8d7b8b3-884e-4e39-b5c8-0de3f03eeee3" />
</p>

##  컴포넌트 다이어그램 표현

**📌 컴포넌트 간 인터페이스 기반 의존 관계 표현**

<p align="center">
<img width="700" height="411" alt="image" src="https://github.com/user-attachments/assets/f715d7ec-b041-4458-94e3-f9ead43d0880" />
</p>

<details>
    <summary>C++ 코드</summary>
    
```cpp
// 제공되는 인터페이스
class ITrackingQuery {
public:
    virtual ~ITrackingQuery() = default;
    virtual void queryStatus(int orderId) = 0;
};

// 요구되는 인터페이스
class ITrackingUpdate {
public:
    virtual ~ITrackingUpdate() = default;
    virtual void updateStatus(int orderId, const std::string& status) = 0;
};

class DeliveryTracking : public ITrackingQuery {
private:
    ITrackingUpdate* updater; // 요구 인터페이스

public:
    DeliveryTracking(ITrackingUpdate* updater)
        : updater(updater) {}

    void queryStatus(int orderId) override {
        // 배송 상태 조회 로직
    }

    void changeStatus(int orderId, const std::string& status) {
        updater->updateStatus(orderId, status);
    }
};

class Address {
public:
    std::string city;
};

class Benefit {
public:
    void apply() {}
};

class Order {
private:
    Address address;                 // association
    ITrackingQuery* trackingQuery;   // 제공 인터페이스 사용

public:
    Order(ITrackingQuery* tracking)
        : trackingQuery(tracking) {}

    void checkDelivery(int orderId) {
        trackingQuery->queryStatus(orderId);
    }

    void applyBenefit() {
        Benefit benefit;  // dependency (일시적 사용)
        benefit.apply();
    }
};
```
</details>

**📌 프로세스간 관계 표현**

<p align="center">
<img width="500" height="564" alt="image" src="https://github.com/user-attachments/assets/ea420eb4-0e61-457f-a415-288dbe944911" />
</p>

<details>
    <summary>C++ 코드</summary>

```cpp
// 외부 PG
struct IExternalPG {
    virtual void pay() = 0;
};

// 모바일 API
struct IMobileAPI {
    virtual void process() = 0;
};

// 포인트
struct IPointService {
    virtual void usePoint() = 0;
};

// 모바일 API 구현
class MobileAPI : public IMobileAPI {
    IExternalPG* pg;   // 외부 PG 의존
    IPointService* point;   // 포인트 의존
public:
    MobileAPI(IExternalPG* pg,  IPointService* point) : pg(pg), point(point) {}
    void process() override {}
};

// API 프록시
class ApiProxy {
    IMobileAPI* mobileApi;
    IPointService* point;

public:
    ApiProxy(IMobileAPI* api, IPointService* pt)
        : mobileApi(api), point(pt) {}
};

// 모바일 앱
class MobileApp {
    ApiProxy* proxy;  // 내부 시스템 직접 접근 X
public:
    MobileApp(ApiProxy* proxy) : proxy(proxy) {}
};
```
</details>

---
