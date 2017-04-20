# 데이터베이스
## 데이터베이스의 종류
#### 1. 계층형 데이터베이스
계층형 데이터 베이스는 데이터의 관계를 트리 구조로 정의하고, 부모, 자식 형태를 갖는 구조이다. 즉, 상위에 레코드가 복수의 하위 레코드를 갖는 구조이다. 하지만 이는 데이터의 중복 문제가 생긴다.
#### 2. 네트워크형 데이터베이스
네트워크형 데이터 베이스는 계층형 데이터의 데이터 중복 문제를 해결했고, 레코드간의 다양한 관계를 그물처럼 갖는 구조이다. 하지만 복잡한 구조 때문에 추후에 구조를 변경한다면 많은 어려움이 따른다. 
#### 3. 관계형 데이터베이스
관계형 데이터 베이스는 우리가 흔히 표현하는 행(Column), 열(Record)로 구성된 Table간의 관계를 나타낼 때 사용한다. 우리는 이렇게 표현된 데이터를 SQL(Structured Query Language)을 사용하여 데이터 관리 및 접근을 한다. 
#### 4. NoSQL 데이터베이스
NoSQL 데이터 베이스는 관계형 데이터 베이스보다 덜 제한적인 일관성 모델을 이용한다. 키(key)와 값(value)형태로 저장되고, 키를 사용해 데이터 관리 및 접근을 한다. 


여기서 흔히 사용하는 관계형 데이터베이스(SQL)과 NoSQL 데이터베이스에 대해 더 알아보도록 하겠다. 

* 관계형 데이터베이스(SQL)
	* 장점
		* 다양한 용도로 사용이 가능하고, 일반적으로 높은 성능을 보여주고 있다.(범용적/고성능) 
		* 데이터의 일관성을 보증한다.
		* 정규화에 따른 갱신 비용 최소화
	* 단점
		* 대량의 데이터 입력 처리
		* 갱신이 발생한 테이블의 인덱스 생성 및 스키마 변경
		* 컬럼의 확장이 어려움
		* 단순히 빠른 결과 
	* 종류
		* Oracle / Oracle
		* MS-SQL Server / Microsoft
		* MySQL / Oracle (SunMicroSystems)
		* DB2 / IBM
		* Infomix / IBM
		* Sybase / Sybase
		* Derby / APache
		* SQLite / Opensource

* NoSQL 데이터베이스
: SQL을 사용하지 않는다는 의미로, Not Only SQL(SQL의 개선/보안의 의미) 
: Non-Relational Operational Database SQL (관계형 데이터베이스가 아니다.)
	* 장점
		* 대용량 데이터
		* 데이터 분산 처리
		* Cloud Computing
		* 빠른 읽기/쓰기 속도
		* 유연한 데이터 모델링

	* 종류
		* Key-Value : 휘발성/영속성 
			* Memchached, Tokyo Tyrant, Flare, Roma, Redis
		* Document : 스키마 정의 없음
			* MongoDB, CouchDB
		* Big Table(Column 형)DB : 뛰어난 확장성, 검색에 유리
			* Hbase, Casandara. Hypertable


우리가 흔히 사용하는 SQL 데이터베이스와 NoSQL의 데이터베이스는 언제 어떻게 사용하느냐가 굉장한 성능에 영향을 준다. 데이터 사이에 관계가 존재하면 SQL 데이터베이스를 사용하고, 데이터 사이에 관계가 필요 없으면 NoSQL 데이터베이스를 사용하면 된다. 

















