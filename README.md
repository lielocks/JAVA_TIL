# 원티드 프리온보딩 백엔드 챌린지 6월 복습 및 java 개인 공부 과정 기록

### 1-1 Java의 정의와 동작 방식
[ 키워드 ] Java, JRE, JDK, JVM, AOT, JIT, Java Bytecode, Code Cache, ByteBuddy
* Java의 구성 요소와 동작 방식
* 클래스 로더와 클래스 로딩
* Java 바이트코드와 코드 캐시
* 바이트코드를 컴파일하는 AOT, JIT 컴파일러

### 1-2 JVM의 정의와 구조, 메모리
[ 키워드 ] JVM, JMM(Java Memory Model), Memory Leak, Thread dump, Heap dump
* JVM과 메모리 구조
* Java 메모리 모델과 메모리 누수
* 스레드덤프를 통한 스레드의 상태 정보 확인
* 힙덤프를 통한 힙 메모리 확인

### 1-3 GC(Garbage Collection)의 정의와 Java GC 알고리즘
[ 키워드 ] SerialGC, Parallel GC, CMS GC, G1 GC, Shenandoah GC, ZGC, Epsilon GC
* 가비지 컬렉션의 정의와 가비지 컬렉터가 처리하는 Heap 영역
* Heap 영역을 제외한 GC 처리 영역
* Java에서 지원하는 GC 알고리즘

### 1-4 동시성 처리를 위한 스레드 동기화
[ 키워드 ] Thread Synchronization, Semaphore, Mutex, volatile, synchronized, CAS, java.util.concurrent package, Virtual Thread
* 스레드 동기화와 동시성
* 멀티 스레드 환경에서 발생하는 스레드 동기화 문제
* Java에서 스레드 동기화를 위해 제공하는 기능
* JDK 19에 추가된 가상 스레드
