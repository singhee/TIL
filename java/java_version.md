## JAVA 5,6,7,8 비교

### 1. JAVA version history
- Java5 (2004~2009)
- Java6 (2006~2013)
- Java7 (2011~)
- Java8 (2014~)
- Java9 (2016예정)
- Java10 (2018예정)


### 2. Java version 주요기능 
#### Java5 (Tiger)
- Generics
- Enhanced for Loop (foreach)
- Autoboxing/Unboxing
- Type-safe Enums
- Varargs
- Static Import
- Annotions (Metadata)
- Formatted IO
- Concurrent API (java.util.concurrent)
- Thread Priority Changes
- Garbage collection ergonomics
- StringBuilder class
	- StringBuilder is almost always faster than StringBuffer
	- StringBuffer unsynchronized version

#### Java6 (Mustang)
- JAX-WS (Web Services Client)
- javax.swing.GroupLayout
- Password prompting
- Free disk-space API
- Classpath wildcards
- Annotation processing done by javac
- Solaris Dynamic Tracing (DTrace) Support)
- jhat QQL (Jmap for heap dump)
- JConsole plugin API
- Monitoring and Management for the Java Platform
	- OOME Handling
- Script Langauge Support
- JDBC 4.0 API

#### Java7 (Dolphin)
- Garbage-First Collector
- Java Programming Language
	- Binary Literals
	- Strings in switch Statements
	- The try-with-resource Statement
	- Catching Multiple Exception Types and Rethrowing Exceptions with Improved Type Checking
	- Underscores in Numeric Literals
	- Type Inference For Generic Instance Creation
	- Improved Compiler Warnings and Errors When Using Non-Reifiable Formal Parameters with Varargs Methods
- NIO 2.0
- Fork-Join
- Dynamic Language Support
- JDBC 4.1 API

#### Java8
- Remove the Permanent Generation
- Small VM
- Define a standard API for Base64 encoding and decoding
- Provide stronger Password-Based-Encryption (PBE) algorithm implementations in the SunJCE provider
- Parallel array sorting
- Interface improvements (static method, defender methods)
- Functional interfaces
- Lambdas
- Stream
- New date/time
- Generic type inference improvements
- Nashorn JavaScript Engine

##### Java8의 특징 중 주목할만한 것들은 다음과 같다.
- 람다식(Lambda Expression)
	- Stream API
	- Default Method
- Nashorn JavaScript Engine
- Joda Time 방식의 새 날짜 API 변경
- 자바 FX8
- meta data 지원 보안
- 동시성 API개선
- IO/NIO 확장
- Heap에서 Permanent Generation 제거