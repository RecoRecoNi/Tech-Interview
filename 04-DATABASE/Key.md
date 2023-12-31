# Key
## 💡 Key (기본키, 후보키, 슈퍼키 등등...) 에 대해 설명해 주세요.
**`키(key)`** 는 테이블(Relation)에서 특정 레코드(행)를 식별하거나 검색하기 위해 사용되는 **식별자**입니다. 각 레코드는 여러 개의 필드(열)로 구성되어 있는데, 이 중 하나의 열을 키로 지정하여 그 값을 사용해 해당 레코드를 식별하고 접근할 수 있습니다. 동시에 각 테이블 간의 관계를 말해주는 연결고리이기도 합니다.

**키의 종류**
  <p align="center">
    <img src="https://github.com/RecoRecoNi/Tech-Interview/assets/70088803/78612954-6d19-487c-b87b-1517214e6e05" align="center" width="50%">
    <img src="https://github.com/RecoRecoNi/Tech-Interview/assets/70088803/e23cf834-7975-4214-8871-90ddcf6a0097" align="center" width="40%">
  </p>

- `슈퍼키(Super Key)`: 테이블 내의 행을 유일하게 식별할 수 있는 하나의 속성 또는 속성의 집합  
   (ex. {학번}, {학번 + 이름}, {주민번호 + 학번}) - **유일성**
- `복합키(Composite Key)`: 2개 이상의 속성(attribute)를 사용한 키
- `후보키(Candidate key)`: 튜플을 유일하게 식별할 수 있는 속성의 최소 집합. 기본키가 될 수 있는 후보이기 때문에 후보키라고 불립니다. (ex. 주민번호, 학번 등) - **유일성, 최소성**
- `기본키(Primary key)`: 후보 키에서 선택된 키. **NULL값이 들어갈 수 없으며**, 기본키로 선택된 속성(Attribute)은 **중복된 값이 들어갈 수가 없습니다**. 한 테이블 내에 하나의 기본키가 정의됩니다.
- `대체키(Alternate key)`: 기본키로 선택되지 않은 후보키
- `외래키(Foreign Key)`**:** 다른 테이블(Relation)의 기본 키(Primary key)를 참조하는 속성. 테이블(Relation)들 간의 참조관계를 나타내기 위해서 사용됩니다.
</br>

## 📑 꼬리질문
### 기본키는 수정이 가능한가요?
- 기본키는 기본적으로 **유일성**과 **최소성**을 만족해야하기 때문에 **기본키 값은 수정해서는 안됩니다.** 만약 기본키가 변경되면 해당 레코드를 참조하는 다른 테이블의 **무결성을 해치는 문제가 발생**할 수 있습니다.
- 그럼에도 기본키 값을 변경해야 한다면, 보통은 레코드를 새로운 값으로 업데이트하는 대신 해당 레코드를 삭제하고 새로운 레코드를 삽입하는 방식으로 사용합니다.
<p align="center"><img src="https://github.com/RecoRecoNi/Tech-Interview/assets/70088803/b6b83c87-4137-4f9f-b5d0-94758e096cc9" height="200" width="500"></p>

```
이제 Customers 테이블에서 특정 고객의 CustomerID를 변경해야 한다고 가정해보겠습니다. 
만약 이 고객이 관련된 주문 데이터가 있다면, 그 주문 데이터도 수정되어야 합니다.

혹시 모를 사항에 대비하기 위해 변경하기 전 데이터베이스의 테이블을 백업합니다.

1. 기존 과정: 기본키 값을 수정
- 고객 A의 CustomerID를 236에서 456으로 변경하려고 합니다.
- 하지만 Customers 테이블에서 기본키 값은 변경되지 않아야 하므로 이 방법은 사용해서는 안됩니다.

2. 새로운 절차: 레코드 삭제 및 삽입
- 고객 A의 정보를 변경하려면 다음과 같이 진행합니다.
- Customers 테이블에서 CustomerID가 236인 레코드를 찾아 삭제합니다.
- 그리고 새로운 레코드(CustomerID=456)를 Customers 테이블에 삽입합니다.
- 만약 이 고객에 대한 주문 정보가 있다면, 해당 주문 정보의 CustomerID도 새로운 값으로 수정합니다.
```
</br>

### 사실 MySQL의 경우, 기본키를 설정하지 않아도 테이블이 만들어집니다. 어떻게 이게 가능한 걸까요?
- MySQL의 경우 버전 8부터 세팅에 따라 Generated Invisible Primary Keys라는 특성을 통해 기본 키를 설정하지 않아도 자동으로 생성되는 기본 키를 생성할 수 도 있고 기본 키 없이 테이블을 생성할 수도 있습니다. 
- 기본 키 없이 테이블을 생성할 때에는 DBMS의 동작 방식과 데이터베이스 디자인에 관련된 문제입니다.
  - **`유연성`** 과 **`선택권`**: 데이터베이스 설계자에게 기본키를 어떻게 구성할지에 대한 유연성과 선택권을 제공합니다. 어떤 경우에는 테이블의 각 레코드를 고유하게 식별할 필요가 없거나, 기본 키 대신 다른 유니크한 인덱스나 조건을 사용하고자 할 수 있습니다.
</br>

### 외래키 값은 NULL이 들어올 수 있나요?
<p align="center"><img src="https://github.com/RecoRecoNi/Tech-Interview/assets/70088803/1085edbb-4174-440a-877a-525907f4e0bc" height="200" width="500"></p>

- 네, 참조 테이블의 외래키(Foreign Key) 값은 명시적으로 NOTNULL을 지정하지 않으면 일반적으로 NULL 값이 들어올 수 있습니다. 외래키는 (다른 or 자기신의) 테이블 기본키를 참조하는데, 이 때 해당 외래키 값이 참조하는 레코드가 없는 경우에는 NULL 값을 가질 수 있습니다.
</br>

### 어떤 칼럼의 정의에 UNIQUE 키워드가 붙는다고 가정해 봅시다. 이 칼럼을 활용한 쿼리의 성능은 그렇지 않은 것과 비교해서 어떻게 다를까요?

1. **`검색 및 조회 성능`**
    - **UNIQUE 키워드**가 붙은 칼럼은 해당 값들이 중복되지 않도록 보장되며, 이때 **UNIQUE index**가 생성되게 됩니다.
    - 이때 데이터 조회 시(자세히는 Where절에 조건이 들어오는 경우) DB 옵티마이저는 전체 테이블을 참조하는 Full Scan 방식이 아닌 인덱스를 참조하는 **Index Unique Scan** 방식을 사용해 조회 성능을 향상시킵니다.
2. **`조인 성능`**
    - **UNIQUE index**가 있는 칼럼을 다른 테이블과 조인할 때 성능이 향상됩니다. **조인 연산은 인덱스를 기반**으로 이루어지므로 중복된 값이 없는 경우 조인 결과를 빠르게 얻을 수 있습니다.
3. **`입력 및 갱신 성능`**
    - **UNIQUE** **제약 조건**이 있는 칼럼에 새로운 값을 삽입하거나 값이 변경되는 경우, 데이터베이스 시스템은 중복 여부를 확인해야 하므로 약간의 오버헤드가 발생할 수 있습니다. 하지만 이 오버헤드는 일반적으로 데이터 무결성의 이점에 비해 미미합니다.
4. **`인덱스 관리 및 저장 공간`**
    - **UNIQUE index**는 중복된 값이 없는 경우에만 생성되므로 인덱스의 크기가 더 작아질 수 있습니다. 또한 중복된 값이 없기 때문에 인덱스의 관리도 간단해지며 저장 공간도 효율적으로 사용될 수 있습니다.
</br>

## 🐍 꼬꼬무
### 기본키를 다른 속성으로 변경 가능한가요?

- 해당 테이블의 기존 기본키 제약조건을 없앤 다음 다른 속성으로 기본키 제약조건을 지정하면 됩니다. 마찬가지로 데이터의 무결성을 위해 지양해야하는 방법이라고 생각합니다.

### MySQL에서 PK를 명시적으로 정의하지 않아도, PK를 설정하게 하는 방법에는 어떤 것이 있을까요?

- MySQL의 경우 명시적으로 Primary Key를 지정하지 않으면 내부적으로 사용자에게 노출하지 않는 기본키(**Generated Invisible Primary Key, GIPK**) 를 생성해 활용합니다.
- 기본키가 아니더라도 UNIQUE 제약을 만족하는 모든 Column을 찾아서 기본 키의 역할을 수행하게 할 수 있습니다.

### 유일성, 최소성에 대해서 설명해주세요

- **`유일성`** : 하나의 키 값으로 유일한 하나의 레코드를 찾아낼 수 있어야 한다.
- **`최소성`** : 키를 구성하는 속성들 중 꼭 필요한 최소한의 속성들로만 키를 구성해야한다.

### 개체 무결성, 참조 무결성, 도메인 무결성에 대해서 설명해주세요

- **`개체 무결성`** : 기본 키는 널 값, 중복 값을 가질 수 없다. (NOTNULL, UNIQUE)
- **`참조 무결성`** : 참조 테이블의 외래키 값은 NULL이거나 피참조 테이블의 기본키 값과 동일해야 한다.
- **`도메인 무결성`** : 속성의 값이 속성에 정의된 도메인에 속한 값이어야 한다. (성별의 경우 ‘남’, ‘여’)

</br>

## 📚 Reference
[MySQL Docs - create table gipks](https://dev.mysql.com/doc/refman/8.0/en/create-table-gipks.html)

[티스토리 - [DataBase] 키(Key)의 개념 및 종류](https://limkydev.tistory.com/108)

[깃허브 - Table 작성 시 PK를 무조건 사용해야 하는 이유](https://hodongman.github.io/2019/01/14/Database-PK를-사용해야-하는-이유.html)

[티스토리 - Oracle SQL - Index Unique Scan](https://muscleking3426.tistory.com/entry/Oracle-SQL-Index-Unique-Scan)

[티스토리 - [DB] 키의 개념과 종류](https://ggop-n.tistory.com/78)

[티스토리 - [DB] 📚 데이터베이스 키(KEY) 종류 🕵️ 정리](https://inpa.tistory.com/entry/DB-📚-키KEY-종류-🕵️-정리)

[벨로그 - Unique 제약조건과 조회시 성능상의 이점](https://velog.io/@jurlring/Unique-%EC%A0%9C%EC%95%BD%EC%A1%B0%EA%B1%B4%EA%B3%BC-%EC%A1%B0%ED%9A%8C%EC%8B%9C-%EC%84%B1%EB%8A%A5%EC%83%81%EC%9D%98-%EC%9D%B4%EC%A0%90)

[도서 - MySQL로 배우는 데이터베이스 개론과 실습](https://www.yes24.com/Product/Goods/77724190)
