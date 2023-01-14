---
description: MySQL id컬럼의 data type에 대하여
---

# \[mysql] MySQL id컬럼의 data type (INT vs BIGINT)

## 1. 상황

ID컬럼이 INT 타입인 DB에 맞춰 typescript로 개발한 app이 있는데, BIGINT 타입으로 변경했더니 많은 에러가 발생함.\
(변경 이유는 생략)

## 2. 원인

BIGINT는 INT와 달리 Number type이 아닌 별도의 타입(bigint)을 사용함.\
전부 Number type으로 개발되어 있고, 비지니스 로직에서 계속해서 분리하여 사용해야 하는 번거로움이 발생\
따라서, BIGINT가 아닌 INT를 사용하여 번거로움을 줄이고자 INT와 BIGINT의 차이를 확인함.

## 3. 개념

- INT / BIGINT 특징

  | Type   | Maximum Value Signed | Minimum Value Signed   | Maximum Value Unsigned | Minimum Value Unsigned | Storage(Bytes) |
  | ------ | -------------------- | ---------------------- | ---------------------- | ---------------------- | -------------- |
  | INT    | 2147483647 (약 21억) | -2147483648 (약 -21억) | 4294967295 (약 43억)   | 0                      | 4              |
  | BIGINT | 약 922경             | 약 -922경              | 0                      | 약 1844경              | 8              |

  (INT, BIGINT 외의 타입에 대한 특징은 맨 아래 참고 사이트 참고)

- id 컬럼에 적용 시 고려할 사항
  - BIGINT 사용 시, TypeScript에서 Number Type이 아닌, BIGINT Type을 사용해야 함.
    - 비지니스 로직에서 Number와 BIGINT 타입으로 분리하여 사용해야 하는 번거로움 발생
  - id 값이 43억을 넘어가는 아주 큰 DB라면 무조건 BIGINT를 사용
  - 43억이 넘지 않다면, 가능한 작은 타입을 선택하는 것이 속도 측면에서 조금 더 유리해 보임

## 4. 정리

컬럼 수가 43억개 이상 넘어갈 정도로 큰 서비스 규모가 아니고, 속도 차이와 type 지정의 번거로움을 줄이기 위해 INT 데이터타입을 사용하기로 하였다.

```
참고 자료: https://dev.mysql.com/doc/refman/8.0/en/integer-types.html
```
