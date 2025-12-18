
# 📘 CHAPTER 3 - 시퀀스 다이어그램 (Sequence Diagram)

- 객체 간의 동적 상호작용을 시간 순서에 따라 정의
- 다이어그램의 수직 방향이 시간의 흐름을 나타냄
- 객체 사이의 기능, 순서, 시간을 명확하게 표현

<p align="center">
  <img width="800" height="1132" alt="image" src="https://github.com/user-attachments/assets/47613195-0753-4157-9681-9b4dd8fc59d8" />
</p>

## 시퀀스 다이어그램 구성 요소

|요소|	description |
|--------|-------------|
|Object |	 시스템의 객체 또는 시스템이 하는 역할|
|Message |	 객체 간의 상호작용을 나타내는 화살표|
|Lifeline |	 시간의 흐름에 따라 객체의 존재를 보여주는 수직선|
|Participant |	 시퀀스 다이어그램에 참여하는 객체|
|Condition |	 메시지가 전송되기 전 만족해야 하는 조건을 표시|


## 시퀀스 다이어그램 유형

<p align="center">
  <img width="1000" height="319" alt="image" src="https://github.com/user-attachments/assets/c5afd0bb-0d8d-42d3-ac7d-99ff4287f702" />
</p>

## 시퀀스 다이어그램 조건 실행

<img width="500" height="1130" alt="image" src="https://github.com/user-attachments/assets/b3e060b0-67b8-4a39-a6b4-f8d0ac9dc44c" />

<img width="500" height="1039" alt="image" src="https://github.com/user-attachments/assets/17879b93-7c5e-41ab-8015-4fed6f817d79" />


|operator|	description|
|--------|-------------|
|alt|	 switch or if else 문, 조건에 만족하는 경로를 선택한다.|
|opt|	 else가 없는 if문, 조건을 만족하면 실행된다.|
|loop|	 메시지 반복 여부를 나타낸다.|
|break|	 조건을 만족하면 Box를 빠져나간다. Box가 없다면 시나리오가 종료된다.|
|par|	 병렬적으로 실행되는 메시지를 나타낸다.|

---
