---
title: "Deep Dive"
date: 2025-03-27 20:40:00 +/-TTTT
categories: [Book, 한 권으로 읽는 컴퓨터 구조와 프로그래밍]
tags: [CS]
math: true
toc: true
# pin: true
---
## 3주차
### 프로그램이 실행되는 과정
책을 보다 데이터 경로와 제어 신호 부분에서 큰 이미지 자료가 딱 나오자마자 눈과 뇌가 받아들이질 못해서 직접 찾아봤다...  
  
#### 초기화
1. 프로그램 코드와 초기 데이터가 메모리에 저장된다.
2. 프로그램 카운터가 실행할 첫 번째 메모리 주소를 가리킨다.
3. 주소 버스를 통해 프로그램 카운터가 가리키는 주소를 메모리에 전달한다.
4. 데이터 버스를 통해 해당 주소에서 명령어를 가져와 CPU로 전달한다.
5. 명령어 레지스터에 가져온 명령어를 저장한다.
  
#### 명령어 사이클 : FETCH
1. 프로그램 카운터가 현재 실행할 명령어의 메모리 주소를 카리킨다.
2. 주소 버스 &rarr; 메모리 &rarr; 데이터 버스 &rarr; CPU &rarr; 명령어 레지스터
3. 프로그램 카운터가 다음 명령어를 가리키게 증가한다.
   
#### 명령어 사이클 : DECODE
1. 명령어 레지스터에 저장된 명령어를 디코딩해 어떤 작업을 수행할지 결정한다.
2. 피보나치의 경우 이전과 현재 값을 더해 다음 값을 계산한다 같은 작업 내용을 얻는다.
  
#### 데이터 읽기 및 연산 준비
1. 간접 주소 레지스터에서 계산에 필요한 데이터를 저장한 메모리 주소를 간접적으로 참조한다.
2. 주소 버스를 통해 간접 주소 레지스터가 가리키는 메모리 주소에 데이터를 요청한다.
3. 데이터 버스를 통해 메모리에서 데이터를 읽고 누산기에 올린다.
  
#### 연산 수행
1. ALU가 연산을 수행한다.
2. 연산 결과를 누산기에 저장한다.
3. 연산 결과에 따라 조건 코드 레지스터에 플래그를 설정한다.  
  
플래그는 오버플로 발생 여부 같은 것을 의미한다.
  
#### 결과 저장
1. 계산된 값을 데이터 버스를 통해 다시 메모리에 저장한다.
2. 현재 계산된 값을 다음 피보나치 수 계산에 다시 사용할 수 있도록 메모리나 레지스터에 저장한다.
  
#### 반복 및 종료
1. 프로그램 카운터가 다음 명령어로 이동하여 반복적으로 피보나치 값을 계산한다.
2. 종료 조건이 충족될 때까지 반복하고 종료한다.

#### 요약
1. 프로그램 카운터가 명령어를 가져온다
2. 명령어 레지스터에서 이를 해석하고, 필요한 데이터를 메모리에서 읽는다.
3. ALU가 연산을 수행하고 결과를 누산기에 저장한다.
4. 결과는 메모리에 기록되고, 조건 코드 레지스터가 상태를 업데이트한다.
5. 위 과정을 종료 조건까지 반복한다.
  
### 비트 플래그
정수의 각 비트를 상태를 나타내는 플래그로 사용하는 기법으로, 여러 상태를 하나의 변수로 관리할 수 있다.  
  
#### 사용자 권한 관리
사용자 권한이 다음과 같이 있을 때때
- 0001(1) : 읽기
- 0010(2) : 쓰기
- 0100(4) : 실행
- 1000(8) : 관리
  
권한 설정
```python
permissions = 0

permissions |= 1
permissions |= 2

print(bin(permissions))  # 출력: 0b11 (읽기 + 쓰기)
``` 
  
권한 확인
```python
if permissions & 2:  
    print("쓰기 권한 있음")
else:
    print("쓰기 권한 없음")
```
  
권한 제거 (and not)
```python
permissions &= ~2  

print(bin(permissions))  # 출력: 0b1 (읽기)
```
  
여러 권한 확인
```python
permissions = 1 | 2 | 4  

print(bin(permissions))  # 출력: 0b111 (읽기 + 쓰기 + 실행)
```
  
플래그 설정 = 왼쪽 시프트 연산
```python
# 읽기 권한 플래그 설정 (1번째 비트)
READ_PERMISSION = 1 << 0  # 1

# 쓰기 권한 플래그 설정 (2번째 비트)
WRITE_PERMISSION = 1 << 1  # 2

# 실행 권한 플래그 설정 (3번째 비트)
EXECUTE_PERMISSION = 1 << 2  # 4
```
  
플래그 확인 = 오른쪽 시프트 연산
```python
permissions = READ_PERMISSION | WRITE_PERMISSION  # 3 (0b11)

if (permissions >> 1) & 1:
    print("쓰기 권한 있음")
else:
    print("쓰기 권한 없음")

```
  
플래그 해제
```python
permissions &= ~(1 << 1)  # 1 (0b1)

print(bin(permissions))  # 0b1 (읽기 권한만 남음)
```
  
[관련 SQL 문제](https://school.programmers.co.kr/learn/courses/30/lessons/301647)

### GPU와 비트코인
비트 코인을 채굴할 때는 블록체인 네트워크에서 해시 함수를 반복적으로 계산해 블록을 유효화 하는 작업이 필요하고, 해당 과정에서 많은 수의 단순한 연산이 필요하다.  
  
GPU는 단순한 ALU 수백 수천 개를 내장하고, 병렬 처리에 특화되어 있기 때문에 이러한 부분에서 CPU보다 훨씬 더 많은 연산을 동시에 처리할 수 있어서 채굴에 사용되었다.
  
## 4주차
### 복잡도와 지역성
시간 복잡도는 알고리즘 해결에 얼마나 많은 시간이 소요되는지라면, 공간 복잡도는 얼마나 많은 메모리가 필요한지에 대한 복잡도다. 최근 하드웨어들은 메모리 용량이 크기 때문에 공간 복잡도가 크게 의미는 없지만, 임베디드 시스템처럼 제한된 성능에서는 중요해진다.  
  
시간 지역성 : 최근에 참조된 주소의 내용은 곧 다음에 다시 참조된다.  
공간 지역성 : 기억장치 내 서로 인접해 저장되어 있는 데이터가 연속적으로 액세스될 가능성이 높아진다.  
  
캐시 히트율은 지역성에 영향을 미치는데 시간 지역성의 경우 최근 접근한 데이터를 재사용하면 캐시 히트율이 높아 `O(n)` 알고리즘이 실제로는 더 짧은 `O(n/캐시 하인 크기)`로 동작할 수 있게 영향을 끼치고, 공간 지역성의 경우에는 인접 데이터를 순차 접근해 캐시 미스율이 낮아지면 알고리즘이 메모리에 접근하는 패널티가 줄어드는데 영향을 준다.  
  
시간 지역성이 좋은 경우는 반복문 내 변수처럼 최근에 접근했던 데이터에 바로 다시 접근하는 경우가 해당하고, 공간 지역성이 좋은 경우는 배열처럼 연속된 공간이 할당된 데이터를 순차적으로 읽을 때가 해당된다.  
  
#### 예시
```java
for (int i = 0; i < 1001; ++i) {
  for (int j = 0; j < 1001; ++j) {
    matrix[i][j] = 1;
  }
}

for (int i = 0; i < 1001; ++i) {
  for (int j = 0; j < 1001; ++j) {
    matrix[j][i] = 1;
  }
}
```
첫 번째 반복문처럼 같은 블록을 연속적으로 읽으면 공간 지역성이 좋지만, 두 번째 반복문처럼 여러 블록을 왔다갔다 하는 경우에는 공간 지역성이 좋지 않다. 시간 복잡도는 같은 코드지만 실제 수행 시간에 차이가 생기는 이유다.  
  
```java
// 기존: O(n)
class XYZ {
  int x;
  int y;
  int z;
}
XYZ[] arr = new XYZ[1000];

int[] x = new int[1000];
int[] y = new int[1000];
int[] z = new int[1000];
```
XYZ 배열보다는 연속된 공간을 사용할 수 있게 같은 타입끼리 배열을 사용해서 묶는 방식으로 데이터 구조를 재구성하면 공가 지역성 개선으로 캐시 히트율을 높일 수 있다.  
  
### 캐시 일관성을 유지하기 위한 방법
캐시 일관성은 공유 메모리 시스템에서 여로 프로세서가 각자의 로컬 캐시를 사용할 때, 동일한 데이터에 대해 일관된 값을 유지해야 되는 것으로, 프로세서 A가 변수 X를 수정하면, 프로세서 B가 변수 X를 읽으면 수정된 값을 가져와야 하고, 만약 일관성이 없다면 B는 수정 전 데이터를 읽을 수도 있다.  
  
이를 해결하기 위한 방식으로는 컴파일 시 문제를 검출하는 소프트웨어적 해결방식과 런타임 시 문제를 검출하는 하드웨어적 해결방식이 존재한다.  
  
#### 소프트웨어적 해결방식
운영체제와 컴파일러를 사용하는 해결방식으로, 안전하게 공유 변수를 사용할 수 있도록 주기를 설정하거나, 아예 공유 데이터 변수를 캐시에 저장하지 않도록 설정하는 방법 등이 존재한다.  
```java
public class CacheCoherenceExample {
    private volatile int sharedCounter = 0;

    // 값 증가
    public void increment() {
        sharedCounter++;
    }

    // 값 읽기
    public int getCounter() {
        return sharedCounter;
    }

    public static void main(String[] args) {
        CacheCoherenceExample example = new CacheCoherenceExample();

        // 쓰레드 생성 및 실행
        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                example.increment();
            }
        });

        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                example.increment();
            }
        });

        t1.start();
        t2.start();

        try {
            t1.join();
            t2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        // 최종 결과 출력
        System.out.println("Final Counter Value: " + example.getCounter());
    }
}
```
자바의 경우에는 위와 같이 `volatile` 키워드를 사용해 변수를 메인 메모리에 직접 저장하고 읽도록 강제할 수 있다.  
  
#### 하드웨어적 해결방식
**스누피 프로토콜 방식**  
공유 버스를 통해 모든 캐시가 트랜잭션을 감시하며 일관성을 유지한다. 캐시 컨트롤러가 버스에서 발생하는 모드 I/O 요청을 감시해 캐시 라인의 상태를 구분하고, 쓰기 작업 시 다른 캐시의 복사본을 무효화한다.  
  
예를 들어 프로세서 A가 주소 X를 수정하면 버스에 신호를 보내 상태를 바꾸면, 다른 프로세서가 주소 X를 읽으려하면 프로세서 A가 최신 데이터를 제공해준다.  
  
**디렉토리 프로토콜**  
중앙 디렉토리가 메모리 블록의 상태와 소유자를 추적해 일관성을 유지한다. 각 메모리 블록은 `Uncached`, `Shared`, `Exclusive` 상태를 가지고, 스누피와 다르게 브로드캐스트 방식이 아닌 관련된 노드만 통신해서 트래픽이 최소화된다.  
  
예를 들어, 프로세서 A가 주소 X를 수정하려면 디렉토리에 `Exclusive` 권한을 요청하고, 디렉토리는 주소 X를 공유 중인 다른 노드들에 무효화 신호를 보낸다.  
  
스누피는 버스 기반 시스템처럼 소규모에서 실시간으로 일관성을 관리하고, 디렉토리는 분산 환경에서 효율적으로 데이터를 관리해준다.  
  
### 캐시 일관성과 오라클 데이터베이스
위에서 살펴봤듯이 일관성을 유지하기 위해 브로드캐스트, 디렉토리, 동기화 등 다양한 방식을 사용하는데, 오라클에서도 공유 메모리 영역인 SGA에 존재하는 버퍼 캐시의 일관성을 래치를 통해 관리하고 있다.  
  
메모리 내 데이터 블록의 일관성은 다음과 같은 순서로 관리된다.  
  
1. 각 데이터 블록의 주소를 해시 함수로 계산한다.
2. 계산한 값으로 특정 해시 버킷에 매핑한다.
3. 동일 버킷에 속한 블록들은 연결 리스트(체인)로 관리된다.
4. 이 체인에 접근할 때 래치로 보호한다.
5. 프로세스들은 체인에 대한 래치를 획득해야 접근할 수 있다.
6. 한 번에 하나의 프로세스만 체인에 접근할 수 있다.
  
반면에 오라클에는 프로세스별로 독립적인 PGA라는 공간도 존재하는데, 독립적이기에 하나의 프로세스만 사용하는 공간이라 이러한 래치 획득 과정, 즉 캐시 일관성 유지 과정이 없기 때문에 빠른 속도로 처리할 수 있다.  
  
### 부품 간 전력 소모 순위
1. GPU (그래픽 카드) : 200W ~ 1000W
2. CPU : 고급 CPU는 작업 부하가 높을 때 130~150W 까지도 소모한다.
3. 메인보드 : 25~100W를 소모한다.
4. HDD : 0.7W ~ 9W, SSD보다 약간 더 높은 전력을 소모한다.
5. SSD : 0.6W ~ 3W를 소모한다.
6. RAM : 2W ~ 5.5W를 소모한다.
  
### 데이터 센터의 전력 소모 문제
데이터 센터는 전 세계 전기 소비량의 약 1~2%를 차지하고, 대부분이 화석 연료 기반 에너지로 수급하기 때문에 탄소 배출량 증가와 기후 변화 문제에도 영향을 끼친다.  
  
데이터 센터에서는 초고성능 컴퓨팅 및 서버를 24시간 가동시켜야 하기 때문에 피할 수 없는 많은 전력 소모가 발생하고, 해당 부분이 데이터 센터의 전력 소모량 40%를 차지한다.  
  
이런 초고성능 컴퓨팅 및 서버를 가동시키면 발생하는 열도 엄청나기 때문에, 이를 위한 냉각 시스템에 사용하는 전력오 40% 정도를 차지한다. 그외에 네트워크 및 스토리지 장비와 전력 관리 등이 20%를 차지한다.  
  
## 5주차
### 패킷과 패킷 공격
패킷은 큰 데이터를 여러 개의 작은 조각으로 나눈 데이터다.  
  
**헤더**  
패킷의 앞부분에 해당하며, 발신지와 목적지 정보, 데이터의 순서 등을 포함  
  
**페이로드**  
실제 데이터가 들어있는 부분  
  
여러 조각의 패킷은 개별적으로 목적지로 보내지고, 목적지에서 모여서 원래의 데이터로 조립된다.  
  
#### 패킷의 장점
1. 패킷은 독립적으로 전송되어 전체가 아닌 문제가 생긴 패킷만 재전송하면 된다.
2. 네트워크 노드 간 최적의 경로를 찾아 이동해서 회선 교환 방식보다 빠르다.
3. 작은 단위의 패킷을 여러 경로로 동시에 전송이 가능해 대역폭을 효율적으로 사용한다.  
    (대용량 데이터를 보내면 그동안 다른 데이터들은 대기해야 하는 단점이 없다.)
4. UDP와 같은 프로토콜을 사용해 연결 설정 없이 빠르게 데이터 전송이 가능하다.  
   (실시간성이 중요한 스트리밍과 게임 서비스에 적합하다.)
  
#### 패킷의 단점
1. 여러 경로를 통해 전송되기에 도착 시간에 차이가 있을 수 있다.
2. 패킷 손실이 발생하면 재전송이 필요해 지연이 발생한다.
3. 데이터의 순서와 도착 여부를 보장하지 않는다.
  
#### 패킷 공격
**DDos**  
대량의 패킷을 보내 과부하를 유발하는 공격으로, TCP 요청을 대량으로 보내 서버가 응답을 계속해서 기다리게해서 자원을 소모시키는 `SYN Flood`와 UDP 패킷을 무작위 포트로 보내 트래픽 처리 과부하를 유발하는 `UDP Flood` 공격이 있다.  
  
**패킷 스니핑**  
네트워크에서 전송되는 패킷을 가로채 정보를 훔치는 공격  
  
**패킷 크래프팅**  
특정 필드를 조작한 패킷을 생성해 보안을 우회하거나 취약점을 악용하는 공격  
  
### 오디오 샘플링
샘플링은 전체에서 대표할 수 있는 데이터들만 얻는 과정이다. 예를 들면, 선거 기간에 모든 사람을 대상으로 여론조사를 할 수는 없으니, 특정 지역과 연령대에서 일부를 대상으로 여론조사를 진행해 대략적인 분석을 진행하는 것과 같다.  
  
**오디오 샘플링**  
연속적인 아날로그 신호를 일정 시간 간격으로 나누어 각 지점의 소리 크기를 숫자로 기록하는 작업  
  
**샘플링 레이트**  
1초 동안 소리를 몇 번 측정할지를 나타내는 값으로 헤르츠(Hz)라는 단위를 사용한다.  
   
헤르츠가 높다면 그만큼 많은 소리를 측정했다는 것이니 품질이 좋고, 용량을 클것이고  
헤르츠가 낮다면 품질은 떨어지겠지만, 용량은 작아진다.  
  
**비트 깊이**  
샘플링한 각 샘플을 몇 비트로 표현할지를 나타내는 정도로, 소리 크기의 세부 정보를 얼마나 정밀하게 기록할지를 결정한다.  
  
**디지털 &rarr; 아날로그(DAC)**  
디지털 데이터는 사람이 들을 수 없는 데이터이기에 이를 다시 아날로그로 변환해야 한다.  
  
1. DAC가 디지털 데이터를 입력 받는다.
2. 각 디지털 값에 대응하는 전압 또는 전류를 생성한다.
3. 생성된 신호를 필터를 통해 부드럽게 처리해 연속적인 아날로그 신호로 출력한다.
  
이때 입력되는 디지털 데이터의 비트수(비트 깊이)가 많을 수록 더 세밀한 아날로그 신호를 생성할 수 있고, 헤르츠가 높을수록 더 자주 데이터를 변환해 더 자연스러운 소리를 만든다.  
  
**푸리에 변환**  
소리는 여러 가지 파동이 섞인 것인데, 푸리에 변환을 사용해 이러한 다양한 파동을 각각의 간단한 파동으로 쪼갤 수 있다. 예를 들면, 오케스트라 연구에서 각각의 악기의 소리를 분류해내는 것과 같다.  
  
푸리에 변환의 과정을 완성된 레고 기차(아날로그 소리)를 분해하고 조립하는 과정으로 살펴보자  
1. 기차를 분해해서 어떤 레고 블록들이 있는지 확인한다.
   - 블록의 종류로 소리의 높고 낮음을, 블록의 크기로 소리의 크기를 알 수 있다.
   - 원하는 종류의 블록만 골라낸다면 특정 종류의 소리만 추출할 수 있다.
2. 기차를 분해하면 최종적으로 블록의 크기(진폭), 블록의 원래 위치(위상)를 알 수 있다.
3. 이 정보를 토대로 다시 조립을 하면 원래 레고 기차를 그대로 다시 만들 수 있다.
  
이렇게 푸리에 변환을 사용하면 다음과 같은 작업을 처리할 수 있다.
- 곡에서 악기별 소리를 분리한다.
- 좋지 않은 소리를 제거하거나, 향상시켜 음질을 개선한다.
- 사진에서 노이즈를 제거하거나 선명도를 높인다.
- 전화나 인터넷 신호에서 잡음을 줄인다.
  
## 6주차
### 배열에 같은 타입의 원소만 저장하면 좋은 점점
1. 같은 크기의 데이터를 메모리 상에 연속적으로 저장하기 때문에 시작 주소와 인덱스를 알면 `시작 주소 + (3 * 데이터의 크기)`로 원하는 인덱스의 주소를 쉽게 계산할 수 있다.
2. 같은 타입의 요소가 연속적이면 데이터를 읽을 때 인접한 다른 원소들도 같이 캐시에 올리게 되어서 배열을 연속적으로 읽을 때 캐시를 활용률이 높지만, 타입이 다르거나 불연속적으로 저장되어 있으면 캐시를 효율적으로 활용할 수 없다.
3. 같은 타입만 들어있기 때문에 반복문을 통해 일괄적으로 처리가 편해진다.
4. 같은 타입만 저장되는걸 컴파일러가 알기 때문에 타입 체크가 가능하다.
  
### 연결 리스트의 시간 복잡도  
- 조회 : 항상 처음부터 탐색하면서 n번째 인덱스까지 접근해야 한다. O(n)
- 탐색 : 특정 값을 찾을 때도 마찬가지로 처음부터 끝까지 탐색해야 한다. O(n)
- 삽입 : 포인터를 알고 있다면 O(1), 그렇지 않다면 O(n)
- 삭제 : 포인터를 알고 있다면 O(1), 그렇지 않다면 O(n)

#### 이터레이터
```java
public class LinkedListIteratorExample {
    public static void main(String[] args) {
        LinkedList<Integer> linkedList = new LinkedList<>();
        for (int i = 1; i <= 5; i++) {
            linkedList.add(i);
        }

        // 일반 반복문 사용용
        for (int i = 1; i <= 5; i++) {
            System.out.println(linkedList.get(i));
        }

        // Iterator 사용
        Iterator<Integer> iterator = linkedList.iterator();

        while (iterator.hasNext()) {
            int number = iterator.next();
            System.out.println(number);
        }
    }
}
```
이터레이터를 사용하면 항상 처음으로 돌아가지 않고 포인터를 기억해 작업을 이어할 수 있다. 따라서, 일반 반복문으로 연결 리스트를 순회하면 매번 0번부터 i-1번까지 순차대로 탐색하지만, 이터레이터를 사용하면 이전 위치부터 시작할 수 있다.  
  
### AVL 트리  
트리에서는 트리의 높이만큼 탐색이 발생하기 때문에 AVL 트리는 트리의 높이 차이를 조정해 탐색 효율을 유지한다.  
  
#### 불균형 유형 및 해결 방법
**Left-Left**  
```text
     A
    /
   B
  /
 C

     B
    / \
   C   A
```
왼쪽 자식의 왼쪽 서브트리가 과도하게 높은 유형으로 단일 우회전을 수행해 불균형을 해결한다.  
  
**Right-Right**  
오른쪽 자식의 오른쪽 서브트리가 과도하게 높은 경우로 단일 좌회전을 수행한다. LL과 정반대라고 생각하면 된다.  
  
**Left-Right**  
```text
기존 구조 (불균형):
     A (BF=+2)
    /
   B (BF=-1)
    \
     C

1단계: B를 기준으로 좌회전 →
     A
    /
   C
  /
 B

2단계: A를 기준으로 우회전 →
     C
    / \
   B   A
```
왼쪽 자식의 오른쪽 서브트리가 높은 경우로 좌회전을 수행해 LL 유형으로 변환 후 LL 불균형 해결 방식으로 처리한다.  
  
**Right-Left**  
오른쪽 자식의 왼쪽 서브트리가 높은 경우로 LR 방식의 반대로 처리하면 된다.  
  
### 자바의 정렬 알고리즘  
```java
// Dual-Pivot QuickSort
Array.sort(기본타입 배열)

// TimSort
Array.sort(객체 배열)

// Arrays.sort(객체 배열) = TimSort
Collections.sort(List)
```
**Dual-Pivot QuickSort**  
- 퀵 정렬의 변형
- 두 개의 피벗을 사용해 분할 효율을 높임
- 평균 O(n log n), 최악 O(n²)
  
**TimSort**  
- 병합 정렬과 삽입 정렬을 결합한 알고리즘
- 이미 정렬되어 있거나 부분적으로 정렬된 데이터에 매우 효율적
- 평균 O(n log n), 최악 O(n²)
  
## 7주차
### 중간 언어  
인터프리터 언어 중 일부에서 실행 효율을 높이기 위해 중간 언어로 컴파일되는 하이브리드 방식을 사용한다. 인터프리터 방식의 단점인 느린 실행 속도를 보완하고, 장점인 플랫폼 독립성을 유지할 수 있다.  
  
대표적인 중간 언어로는 파이썬, 자바, 자바스크립트가 있다.  
(인터프리터 방식만 사용하는 언어들은 아니고 혼합해서 사용한다...)
  
#### 파이썬
1. 파이썬 컴파일러가 소스 코드를 파이썬 바이트 코드인 pyc 파일로 컴파일한다.
2. 해당 바이트 코드는 파이썬 가상 머신(PVM)에서 실행된다.
  
#### 자바
1. 자바 소스 코드를 바이트 코드로 컴파일한다.(.java &rarr; .class)
2. JVM에서 인터프리터에 의해 실행된다.
  
### 자바 파일과 JVM  
#### 자바 컴파일러
개발자가 작성한 `java` 파일은 JVM이 인식할 수 없기에 자바 컴파일러가 해당 파일을 자바 바이트 코드로 이루어진 `class` 파일로 변환한다.  
![빌드결과](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb4m5TI%2FbtsFml5lojC%2FR5jfPHPN1d7RUvHBPnoxvk%2Fimg.png)  
IDE에서 실행하거나 javac 명령어를 통해 자바 컴파일러로 컴파일을 하면 사진처럼 out 폴더에 class 파일이 생성되는 것을 확인할 수 있다.  
  
#### 런타임 환경
JVM의 클래스 로더가 동적으로 필요한 클래스들을 로딩하고 링크해서 JVM의 메모리인 런타임 데이터 영역에 올려 놓으면, 실행 엔진이 메모리에 올라온 바이트 코드를 명령어 단위로 가져와 실행한다.  
  
이때 인터프리터와 JIT 컴파일러가 사용된다.  
  
#### 클래스 로더의 세부 동작
로드 &rarr; 링크(검증, 준비, 분석) &rarr; 초기화  
  
1. 클래스 파일을 메모리에 로드
2. 자바 언어 명세와 JVM 명세를 잘 지켰는지 검증
3. 필드, 메서드, 인터페이스와 같이 클래스가 필요로 하는 메모리 준비(할당)
4. 클래스의 상수 풀 내 모든 심볼릭 레퍼런스를 다이렉트 레퍼런스로 변경(분석)
5. 클래스 변수들을 적절한 값으로 초기화  
  
#### 자바 인터프리터
자바는 기본적으로 빠른 실행을 위해 런타임 시에 한 줄씩 읽으면서 이진 코드로 번역해 실행하는 인터프리터 방식을 사용하고, 일정 기준이 넘어가면 JIT 컴파일 방식을 사용한다.  
  
코드를 한 번에 컴파일 해두고 재사용하는 것이 아니라 같은 코드를 여러번 실행하면 매번 인터프리터가 작동해야해서 느리다.  
  
### JVM Warm Up 
서버 배포 직후 API 요청을 보냈을 때 속도가 배포 후 시간이 지난 뒤의 API 요청보다 느린 현상을 발견할 수 있는데, 이는 클래스 로더의 지연 로딩과 JIT 컴파일러의 동작 방식 때문이다.  
  
#### 클래스 로더의 지연 로딩
클래스 로더는 자바 애플리케이션이 시작될 때 모두 메모리에 적재하지 않고, 각 클래스들이 직접적으로 필요한 시점에 로딩을 한다. 즉, 서버 배포 직후에는 요청을 처리하기 위해 필요한 클래스들이 아직 메모리에 적재가 되지 않은 상태기 때문에 초기 지연이 발생한다.  
  
#### JVM에서 컴파일 방식을 사용하는 시점
자바는 인터프리팅과 컴파일 방식을 모두 사용하는데, 애플리케이션 실행 시에 모든 class 파일을 컴파일 후에 실행하면 속도는 빠르지만 실행에 많은 시간이 소요될 수 밖에 없다.  
  
그래서 JIT 컴파일러는 실행 시 모든 코드를 컴파일 하지 않고 실행 중 동적으로 컴파일을 진행한다.  
  
#### JIT 컴파일러
즐겨찾기 방식으로 컴파일을 진행하는데 애플리케이션 실행 중 자주 실행되는 부분을 핫스팟이라 하고, 해당 부분을 컴파일을 진행한다.  
  
GC가 age를 기록하는 것과 비슷하게 JIT 컴파일러도 프로파일링 과정을 통해 실행 중인 애플리케이션의 동작을 분석하고 기록해 핫스팟을 식별한다. 식별된 핫스팟을 컴파일해 코드 캐시라는 곳에 저장해 매번 컴파일 할 필요 없이 저장된 것을 꺼내 쓸 수 있다.  
  
#### JIT 컴파일러 동작 방식
JIT 컴파일러는 Tiered Compliation이라는 여러 단계로 나뉜 컴파일 과정으로 동작하며 인터프리터와 C1, C2 두 개의 컴파일러로 이루어져 있다.  
  
##### C1
가능한 빠른 실행 속도를 목적으로 하지만 최적화와 컴파일도 가능한 빠르게 하기 위해 제한된 수준으로만 최적화를 진행하는 컴파일러다.  
  
##### C2
C1 메서드로 최적화 및 컴파일 된 특정 메서드가 일정치 이상 호출되면 C2 컴파일러에 의해 C1보다 높은 수준의 최적화를 거쳐 컴파일이 진행된다.
  
C1과 C2 컴파일러로 컴파일된 코드는 모두 동일하게 코드 캐시에 저장된다.  
  
##### Tiered Compliation
Tiered Compliation 과정은 0 ~ 4 Level로 구분된다.  
  
- Level 0 : Interpreted Code
  - 애플리케이션 실행 초기에는 모든 코드를 인터프리터를 통해 실행한다.
  - 당연히 컴파일 방식보다 성능이 낮다.  
  
- Level 1 : Simple C1 Compiled Code
  - 복잡도가 낮은 메서드를 대상으로 컴파일한다.
  - C2 컴파일러로 최적화 및 컴파일을 해도 유의미한 성능 향상이 기대되지 않는다.
  - 따라서 프로파일링도 진행하지 않는다.
  
- Level 2 : Limited C1 Compiled Code
  - C2 컴파일러의 큐가 가득찬 경우 제한된 수준의 프로파일링과 최적화를 한다.
  
- Level 3 : Full C1 Compiled Code
  - 일반적인 상황에서 수행되는 단계이다.
  - 최대 수준의 프로파일링과 최적화를 한다.
  
- Level 4 : C2 Compiled Code
  - 장기적 성능 향상을 목적으로 C2 컴파일러가 최적화를 수행한다.
  - 이 단계에서 최적회된 코드는 완전한 최적화가 이루어져 프로파일링을 하지 않는다.
  
#### JVM Warm Up
Latency가 발생하는 두 가지 원인을 확인했으니 어떻게 해결할지는 단순하다.  
  
- 클래스 로더의 지연 로딩이 발생하지 않게 필요한 클래스들을 미리 사용해둔다.
- C1, C2 컴파일러를 사용하기 위해 메서드를 미리 일정치 이상 호출해둔다.
  
결국 Warm Up은 후라이팬을 사용하기 이전에 예열을 하는 것처럼 JVM을 예열 시키는 방법이라고 볼 수 있다.  
  
코드를 모두 실행해서 모든 클래스를 적재하고 메서드들도 예열시키기는 무리가 있기 때문에 자주 사용될 것으로 예상되는 부분들을 위주로 Warm Up을 진행해주면 된다.  
  
각 컴파일러의 컴파일 임계치(Compile Threshold)는 C1은 1,500회, C2는 10,000회이다.  
  
## 8주차
### 각 언어에서 버퍼를 사용하는 방법
#### JAVA
```java
public class BufferedIOExample {
  public static void main(String[] args) {
    try {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        System.out.print("문자열을 입력하세요: ");
        String input = br.readLine();
        
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        bw.write("입력한 문자열: " + input);
        bw.newLine(); // 줄바꿈
        bw.flush(); // 버퍼 비우기 == 출력
        
        br.close(); 
        bw.close();
    } catch (IOException e) {
        e.printStackTrace();
    }
  }
}
```
**BufferedReader**  
- Enter를 경계로 입력값을 인식한다.
- Scanner는 Space도 인식한다.
- 항상 String으로 반환한다.
  
**BufferedWriter**  
- 버퍼에 데이터를 모아두었다가 한 번에 출력한다.
- 버퍼가 가득차면 자동으로 출력한다.
- 버퍼가 가득차지 않아도 flush를 호출하면 강제로 출력할 수 있다.
- close에서도 내부적으로 flush를 호출한다.
  
#### Python
```python
line = sys.stdin.readline()
sys.stdout.write(line)
sys.stdout.flush()
```
- 자바의 BufferedReader/BufferedWriter와 비슷하게 사용할 수 있다.  
- 입력값도 똑같이 문자열로 받는다.
- sys.stdin.buffer/sys.stdout.buffer를 사용하면 입출력을 바이트로 처리한다.  
  
#### C/C++
```c
#include <stdio.h>

int main() {
    char buffer[1024];

    printf("문자열을 입력하세요: ");
    fflush(stdout);

    if (fgets(buffer, sizeof(buffer), stdin) != NULL) {
        fputs("입력한 문자열: ", stdout);
        fputs(buffer, stdout);
    }

    return 0;
}

```
- `fgets()`로 `stdin`에서 라인 단위로 입력을 처리한다.
- `fputs()`로 `stdout`으로 버퍼링된 출력을 한다.
- `fflush()`로 즉시 버퍼에서 출력한다.
- `scanf()`와 `printf()`가 성능적으로 더 빠르다고 함
- C++에도 함수가 동일하게 존재하고 문법만 다르다.
  
### 표준 입출력을 동일하게 처리할 수 있다는게 무슨 말일까?
```c
// 콘솔에서 입력 받기
void readFromConsole() {
    char buffer[100];
    printf("콘솔에 입력하세요: ");
    scanf("%s", buffer);
    printf("입력한 내용: %s\n", buffer);
}

// 파일에서 읽기
void readFromFile() {
    FILE *file = fopen("data.txt", "r");
    if (file != NULL) {
        char buffer[100];
        fscanf(file, "%s", buffer);
        printf("파일 내용: %s\n", buffer);
        fclose(file);
    }
}

// 파일에 쓰기
void writeToFile(char *data) {
    FILE *file = fopen("data.txt", "w");
    if (file != NULL) {
        fprintf(file, "%s", data);
        fclose(file);
    }
}
```
C의 경우에는 위와 같이 `scanf()`와 `fscanf()`, `printf()`와 `fprintf()`처럼 콘솔과 파일 모두 유사한 함수를 사용해 일관된 방식으로 처리할 수 있다.
```javascript
// 콘솔(브라우저 프롬프트)에서 입력 받기
function readFromConsole() {
    const input = prompt("콘솔에 입력하세요:");
    console.log("입력한 내용:", input);
    return input;
}

// 로컬 스토리지에 데이터 저장 (파일 시스템 직접 접근 불가)
function saveData(data) {
    localStorage.setItem("savedData", data);
    console.log("데이터가 저장되었습니다.");
}

// 로컬 스토리지에서 데이터 읽기
function loadData() {
    const data = localStorage.getItem("savedData");
    console.log("저장된 데이터:", data);
    return data;
}

// 파일 시스템 접근
function nodeJsFileExample() {
    const fs = require('fs');
    
    // 파일에 쓰기
    fs.writeFileSync('data.txt', '안녕하세요', 'utf8');
    
    // 파일에서 읽기
    const data = fs.readFileSync('data.txt', 'utf8');
    console.log("파일 내용:", data);
}
```
반면에 자바스크립트의 경우에는 브라우저에서는 파일 시스템에 직접 접근할 수 없어서 로컬 스토리지 등을 사용해야해서 완전히 다른 API를 사용하고, Node.js 같은 경우에는 파일 시스템에 접근이 가능하지만 이 때도 브라우저 API와 다른 API를 사용한다.  
  
즉, C는 다양한 입출력 장치를 모두 파일로 취급해(장치의 추상화) `fxxxx()` 함수를 사용해 다양한 장치와 상호작용을 할 수 있지만, 자바스크립트는 각 기능별로 별도의 인터페이스를 제공해 동일하게 처리할 수가 없다.  
  
### C는 왜 입출력 장치를 모두 파일로 추상화 했을까?
C는 유닉스와 함께 개발된 언어라서 유닉스의 철학의 영향을 많이 받았는데, 그중에서도 "모든 것은 파일이다"라는 철학에 따라 진짜 모든 것을 파일로 추상화 해버렸다.  
  
이런 철학 덕분에(?) 유닉스에서는 일반 파일뿐만 아니라 다음과 같은 항목들도 파일처럼 접근하고 다룰 수가 있어졌다.  
- 장치(키보드, 모니터, 프린터, 마우스 등)
- 네트워크 소켓, 파이프 등의 통신 채널
- 프로세스 정보
  
```c
#include <stdio.h>

void processStream(FILE* stream) {
    char buffer[100];
    
    // 어떤 스트림이든 동일한 함수로 처리
    if (fscanf(stream, "%s", buffer) > 0) {
        printf("읽은 데이터: %s\n", buffer);
    }
}

int main() {
    printf("키보드에서 입력하세요: ");
    
    // 키보드 입력 처리
    processStream(stdin);
    
    // 파일 입력 처리
    FILE* file = fopen("test.txt", "r");
    if (file != NULL) {
        processStream(file);
        fclose(file);
    }
    
    return 0;
}
```
그래서 C에서는 위와 같이 키보드나 파일이나 같은 파일이기 때문에 동일하게 처리가 가능하게 만들어진 것이다.  
  
## 9주차
### 자바스크립트는 데이터베이스와의 통신에서 스레드 환경 차이를 어떻게 극복할까?
#### 구조적 차이
자바스크립트는 싱글 스레드 기반의 이벤트 루프 구조로 물리적으로 비동기를 지원하는 것이 아닌 비동기 함수를 통해 논리적으로 동시성을 구현한다. 반면에 데이터베이스는 멀티 스레드로 여러 트랜잭션을 동시에 처리할 수 있다.  
  
이러한 구조적 차이로 인해 동시에 여러 비동기 요청이 DB에 발생하면 경쟁 상태나 트랜잭션 불일치 문제가 발생할 수 있다.  
  
#### 비동기 함수와 트랜잭션
`Promise.all()` 함수는 여러 비동기 작업을 동시에 실행하고, 모두 성공해야 성공이다. 트랜잭션도 마찬가지로 하나라도 실패하면 롤백, 모두 성공해야 커밋을 진행한다.  
  
둘은 비슷한 점이 있지만 연결에 대한 부분이 많이 다르다. 트랜잭션은 같은 커넥션에서 실행되지만, `Promise.all()` 함수의 경우에는 병렬로 실행되어 동시에 여러 요청이 서로 다른 커넥션에서 실행될 수 있어서 트랜잭션 범위 밖에서 작업이 실행되는 문제가 발생할 수 있다.  
  
#### 발생할 수 있는 문제
- 동시에 같은 데이터를 수정할 때 실행 순서에 따라 데이터가 예상과는 다르게 변경될 수 있다. (Race Condition)
- 일부 작업만 반영되는 트랜잭션 불일치가 발생할 수 있다.
  
#### ORM 기술에서는 트랜잭션을 어떻게 관리할까?
```typescript
const queryRunner = connection.createQueryRunner();
await queryRunner.connect();
await queryRunner.startTransaction();
try {
  await queryRunner.manager.save(...);
  await queryRunner.commitTransaction();
} catch (e) {
  await queryRunner.rollbackTransaction();
} finally {
  await queryRunner.release();
}
```
TypeORM의 경우 반드시 동일한 쿼리 러너(커넥션)에서 모든 쿼리를 실행해야 트랜잭션이 동작한다.  
```javascript
await prisma.$transaction(async (tx) => {
  await tx.model.create(...);
  await tx.model.update(...);
});
```
Prisma, Sequelize 같은 경우는 트랜잭션을 전달 받아 쿼리를 실행한다.  
  
#### Race Condition 방지 전략
- 순차 실행 : 반드시 await으로 순차적으로 실행하거나, 같은 커넥션에서 모든 쿼리를 실행해서 트랜잭션을 하나로 관리한다.
- 락 사용 : DB에서 비관적 락 혹은 낙관적 락을 활용한다.
- 트랜잭션 격리 수준 조정 : 격리 레벨을 높여 동시성 이슈를 줄인다.
- 중복 요청 취소 : 프론트 레벨에서 중복 요청을 막는다.
  
## 10주차
### 스프링에서 평문을 암호화 하는 방식
#### BCryptPasswordEncoder
```java
BCryptPasswordEncoder encoder = new BCryptPasswordEncoder(10);
String encodedPassword = encoder.encode("myPassword");
boolean isMatch = encoder.matches("myPassword", encodedPassword);
```
- 단방향 해시 함수로 주로 비밀번호를 저장 및 검증 할 때 사용한다.
- 내부적으로 salt를 자동 생성하고, 생성자 파라미터는 해싱의 강도를 설정한다.
    
#### Pbkdf2PasswordEncoder
```java
Pbkdf2PasswordEncoder encoder = Pbkdf2PasswordEncoder.defaultsForSpringSecurity_v5_8();
String encodedPassword = encoder.encode("myPassword");
```
- salt와 반복 횟수를 사용해 반복적으로 해싱한다.
- 의도적으로 속도가 느리게 만들어진 방식이라 무차별 대입 공격에 강하다.
  
#### Encryptors.stronger / Encryptors.standard
```java
BytesEncryptor stronger = Encryptors.stronger("myPassword", "salt");
BytesEncryptor standard = Encryptors.standard("myPassword", "salt");
```
- 대칭키 암호화 방식이다.
- 각각 AES-256, AES-128 방식을 사용한다.
- 비밀번호와 salt를 이용해 키를 생성하고, 데이터를 암호화/복호화 한다.
- 키가 유출되면 복호화가 가능해 키 관리가 중요하다.
- 비밀번호는 복호화가 불가능해야 안전하기 때문에 파일이나 설정 값 같은 데이터를 보호할 때 적합하다.
  
#### Custom
```java
PasswordEncoder encoder = new PasswordEncoder() {
    String salt = KeyGenerators.string().generateKey();

    PasswordEncoder encoder = new PasswordEncoder() {
        @Override
        public String encode(CharSequence rawPassword) {
            return salt + ":" + rawPassword.toString();
        }

        @Override
        public boolean matches(CharSequence rawPassword, String encodedPassword) {
            String[] parts = encodedPassword.split(":", 2);
            if (parts.length != 2) return false;
            String storedSalt = parts[0];
            String encoded = storedSalt + ":" + rawPassword.toString();
            return encoded.equals(encodedPassword);
        }
    };

    String rawPassword = "myPassword";
    String encodedPassword = encoder.encode(rawPassword);

    System.out.println("원본 비밀번호: " + rawPassword);
    System.out.println("암호화된 비밀번호: " + encodedPassword);

    boolean isMatch = encoder.matches(rawPassword, encodedPassword);
    System.out.println("비밀번호 일치 여부: " + isMatch);
};
```
- 직접 PasswordEncoder를 구현할 수도 있지만, 이미 검증된 방식들을 사용하는게 좋다.
  
### 정보를 어떻게 최소한으로 유지할까?
1. 서비스 제공이나 법적으로 필요한 필수 정보만 수집하고, 선택 정보는 별도로 구분해 동의를 받는다.
2. 정보마다 보관 기간을 정하거나 사용 목적을 달성한 정보는 삭제한다.
3. 연락처나 주민등록번호 등 마스킹을 적용해 일부만 보여준다.
4. 통계 등에 데이터를 사용할 때 개인을 특정하지 못하게 익명으로 처리한다.
