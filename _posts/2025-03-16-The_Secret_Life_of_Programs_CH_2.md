---
title: "전자 회로의 조합 논리"
date: 2025-03-16 16:24:00 +/-TTTT
categories: [Book, 한 권으로 읽는 컴퓨터 구조와 프로그래밍]
tags: [CS]
math: true
toc: true
# pin: true
---
# 디지털 컴퓨터의 사례
기계적인 계산 장치 : 톱니바퀴, 안티키테라, 화기 제어 컴퓨터  
톱니바퀴를 사용하지 않는 기계식 컴퓨터 : 계산자  
  
> **계산자**  
> 고정된 x의 로그 눈금과 움직이는 y로그 눈금의 기준선을 맞춰 두 수의 곱을 계산할 수 있다.  
> 요즘도 비행 컴퓨터라고 부르는 동그란 계산자를 사용하기도 한다.  
  
역사적으로 계산 장치의 주된 용도는 숫자를 세는 것이었는데, 기원전 18,000년 전부터  
탤리 막대라는 것을 사용하기도 했고, 근대에는 차분 기관이라는 복잡한 10진 기계식 계산기 등이 등장했다.  
  
## 아날로그와 디지털의 차이
계산자는 수학적으로 연속적이기 때문에 `1.1`과 같은 수를 쉽게 표현할 수 있지만,  
손가락은 하나하나 다른 존재로 구분되는 이산적인 성질을 가져 정수만 표현할 수 있다.  
  
전자기술에 대해 이야기할 때 아날로그는 연속적인 것을 뜻하고, 디지털은 이산적인 것을 뜻한다.
(라탄어로 손가락이 digitus라고 한다! 디지털의 어원...)  
  
아날로그는 실수를 표현할 수 있기에 계산에는 더 적합할 수도 있지만, 정밀도의 문제가 있다.  
계산자의 크기를 더 크게 만들면 정밀도를 올릴 수 있겠지만 너무 큰 계산자는 불편할 뿐이다.  
  
## 하드웨어에서 크기가 중요한 이유
오늘날 컴퓨터 클록 속도는 4GHz고, 이는 1초에 40억 가지 계산을 처리할 수 있고,  
40억 분의 1초 동안 전자가 이동할 수 있는 거리는 75밀리미터다.  
  
CPU의 한 면이 18밀리미터면 전자는 40억 분의 1초 동안 2번 왕복할 수 있다.  
만약에 CPU가 더 작다면 더 많이 왕복할 수 있을 것이다.  
  
즉, 왕복을 더 많이 할 수 있게 하려면 전자의 속도를 올리거나 CPU를 더 작게 하거나인데  
전자의 속도를 올린다는건 말이 안되는 소리니 하드웨어를 작게 만들 수 밖에 없다.  
  
또한, 하드웨어가 작아질 수록 결국 전자의 이동 거리가 줄어들고, 이동 거리가 줄면  
필요한 에너지가 줄어드는 것이기 때문에 저전력, 열감소 효과가 생긴다.  
  
## 디지털을 사용하면 더 안정적인 장치를 만들 수 있다
하드웨어를 작게 만들면 속도와 효율은 좋아지지만, 물체간 간섭이 쉬워진다.  
  
계산자와 계량컵과 같은 아날로그 장치는 정확한 값을 얻으려면 흔들림이 없어야 하지만  
아날로그 장치는 작은 외부의 무언가에도 영향을 쉽게 받는다.  
  
반면에 디지털 장치는 판정 기준이 있기 때문에 외부로 인한 영향을 받아도  
계산 결과에 영향을 받지 않지만, 판정 기준으로 인해 어떤 범위의 값을 표현할 가능성이 사라진다.  
  
## 아날로그 세계에서 디지털 만들기
**전이 함수**  
실제 세계에서 벌어지는 현상을 표현하는 함수로 X축의 입력값을 Y축의 출력값으로 변환한다.  
  
**디지털 카메라 센서의 전이 함수**  
빛이 전이 함수 그래프의의 상단부(포화)나 하단부(차단)에 많이 닿기보단  
직선부에 많이 닿게 만들어야 현실을 잘 반영하는 이미지를 얻을 수 있다.  
  
**게인**  
입력 신호의 증폭 비율
  
**문턱값**  
출력값을 이산적으로 나눌 수 있는 기준이 되는 게인 값  
= 특정 수준 이하의 신호를 무시하거나 조정하는 기준점  
= 소음 제거  
  
## 10진 숫자 대신 비트를 사용하는 이유
1. 컴퓨터는 손가락이 없고, 손가락은 하나의 숫자씩만 표현할 수 밖에 없다.  
2. 전이 함수를 각기 다른 10가지 문턱값으로 구분할 수 있는 간단한 방법이기 때문이다.  
   
# 간단한 전기 이론 가이드
## 전기는 수도 배관과 유사하다
직렬 연결 : 한 쪽의 출력이 다른 쪽의 입력에 연결된 경우  
병렬 연결 : 서로 다른 쪽의 입력을 함께 연결하고, 양쪽의 출력을 같은 곳에 연결하는 경우  
  
전기선 = 내부(도체) + 외부(부도체)  
스위치 : 전기의 흐름을 제어할 수 있는 밸브 역할  
옴의 법칙 : 전류 = 전압 / 저항  
  
## 전기 스위치
스위치를 만드는 일은 도체 사이에 부도체를 삽입하거나 제거하는 간단한 일이다.  
전기 시스템은 회로라고 부르는데 이는 에너지 근원에서 나온 전기가 회로 구성요소를 지나  
다시 근원으로 돌아오기 때문이다.  
  
# 비트를 처리하기 위한 하드웨어
## 릴레이
선을 둥글게 감아 코일로 만든 후 전기를 흘려보내면 코일이 전자석이 되고,  
전자석은 켜고 끌 수 있기 때문에 자기장을 이용해 물건을 움직일 때 사용할 수 있다.  
  
릴레이는 스위치를 움직이기 위해 전자석을 사용하는 장치로  
전기 신호를 이용해 전원을 끄거나 켜는 스위치다.  
  
전류가 흐른다 &rarr; 전자석 작동 &rarr; 스위치 켜짐  
전류가 끊긴다 &rarr; 전자석 꺼짐 &rarr; 스위치 꺼짐  
  
릴레이를 사용해 NOT 함수를 구현하는 인버터를 만들 수 있는데  
AND 회로의 출력을 OR 회로의 입력으로 사용할 수 있는 것처럼  
스위치가 다른 스위치를 제어하게 만들 수 있어 복잡한 논리를 만들 수 있어진다.  
  
추가로 릴레이에서 전이 함수의 문턱값은 수직인데, 이는 코일에 전압을 아무리 천천히 높여도  
스위치는 항상 다른 위치로 순식간에 움직이기 때문이다.  
  
하지만 릴레이는 느리고 전기를 많이 소모할 뿐만 아니라 먼지 같은 것들이  
스위치 접점에 있으면 오작동을 하는 단점 등이 있다.  
  
## 진공관
물체를 충분히 가열하면 전자가 튀어나오는 열전자 방출이라는 현상을 기반으로 만들어짐  
  
삼극관(캐소드, 그리드, 애노드)  
캐소드 = 투수, 그리드 = 타자, 애노드 = 포수  
히터 : 캐소드를 가열하는 역할

히터가 캐소드를 가열해 전자가 튀어나가게 하면 그리드가 방해하지 않는한 애노드에 도달하기 때문에  
투수가 던진 공(전자)이 타자(그리드)가 치지 못해 포수(애노드)에 간다고 이해할 수 있고  
이런 부분에서 그리드를 스위치 손잡이라고 생각할 수 있다.  

장점 : 움직이는 부분이 없어 릴레이보다 빠르다  
단점 : 아주 뜨겁고 깨지기 쉽다  
  
## 트랜지스터
도체와 부도체 사이를 오갈 수 있는 반도체를 사용해 만든 전송 저항이라는 말을 줄인 것이다.  
트랜지스터를 아주 작게 만들면 당연히 좋지만, 도체가 얇고 작아질수록 저항이 늘어나 열이 발생하고  
반도체는 쉽게 녹을 수 있기 때문에 트랜지스터에서 열을 제거하는 일은 중요하다.  
  
트랜지스터는 반도체 물질로 이루어진 기판 또는 슬랩 위에 만들어지고, 보통 실리콘이 기판 재료로    
광식각이라는 트랜지스터 그림을 실리콘 웨이퍼 위에 투영해서 대량 생산하는 작업을 통해 생산된다.  
  
트랜지스터는 쌍극 접합 트랜지스터(BJT)와 전계 효과 트랜지스터(FET) 두 유형이 있고  
제조 공정에서 기판 물질의 성질을 변화시키기 위해 p와 n 유형의 물질으로 이루어진  
영역을 만들어내는 도핑 과정이 들어간다.  
  
게이트와 베이스는 스위치 손잡이 역할이고, 손잡이가 올라가면 전기가 위에서 아래로 흐른다.  
  
쌍극 접합 트랜지스터
- 전기가 한 방향으로만 흐름
  
전계 효과 트랜지스터
- 정전기 효과를 사용해 스위치를 움직인다.
- MOSFET라는 전력 소모가 적어 현대 컴퓨터 칩에서 가장 많이 사용되는 것이 있다.
- N채널과 P채널 MOSFET를 보완하기 위해 한 쌍으로 묶어 사용하는 CMOS가 있다.  
  
## 집적 회로
트랜지스터는 작고, 빠르고, 신뢰할 수 있고, 전력 소모가 적은 좋은 논리 회로지만  
간단한 회로를 만들 때도 부품이 너무 많이 필요하다.  
  
이를 위해 복잡한 시스템을 트랜지스터 하나를 만드는 정도의 비용으로 만들 수 있는  
집적 회로라는 것이 등장했고, 생긴 모양 때문에 칩이라고 부른다.  
  
# 논리 게이트
논리 게이트 : 논리 연산을 수행하는 회로가 미리 들어간 집적 회로(칩)  
(ADN 게이트, OR 게이트, XOR 게이트, 인버터, 버퍼(입력을 출력으로 전달))

게이트를 사용하면 하드웨어 설계자가 처음부터 모든 회로를 설계할 필요 없이  
복잡한 회로를 쉽고 빠르게 만들 수 있게 되었다.  
  
AND와 OR 게이트가 가장 효율적인 게이트는 아니고, 가장 단순한 회로는 NAND와 NOR인데,  
이는 NAND와 NOR은 TTL 혹은 CMOS를 사용하지만, AND와 OR은 NAND와 OR에 트랜지스터를 덧붙여  
출력을 반전시켜야 해서 더 비싸고, 반응 속도도 느리고, 전력도 더 소모된다.  
  
그래서 디지털 회로 설계시 NAND와 OR 게이트가 가장 기본적으로 사용되고,  
NAND와 OR 게이트만 있어도 OR, AND, NOT으로 표현할 수 있는 모든 논리를 표현할 수 있다.  
  
## 이력 현상을 활용한 잡음 내성 현상
현실에서 사용 중인 신호 중에는 천천히 변하는 신호가 많다.  
  
**글리치**  
입력 신호에 잡음이 존재해 입력 신호가 문턱값을 여러 번 오락가락 하는 현상  
  
**이력현상**  
판정 기준이 이력에 따라 달라지는 현상, 이를 이용해 글리치를 방지할 수 있다.  
각 신호에 대해 다른 전이 함수를 사용해 서로 다른 문턱값을 만들 수 있고,  
이는 입력 신호가 다른 문턱값을 지나가며 출력이 반전되려면 값이 상당히 많이 변해야 한다는 것이고  
잡음 내성이 커진다는 말이다.  
  
**슈미트 트리거**  
이력을 사용하는 게이트, 일반적인 게이트보다 복잡하고 비쌈  
  
## 차동 신호
이력을 도입해도 잡음 내성이 충분하지 않은 경우에도 보호를 받고 싶은 경우에 사용한다.  
  
2인조의 경우 본인이 왼쪽에 있으면 0, 오른쪽에 있으면 1일 때,  
둘이 한 쪽으로 밀려나도 본인과 다른 한 명의 상대적인 위치 값은 변하지 않는 것처럼  
측정하는 값이 서로 반전관계인 신호 쌍의 차이를 측정하는 방식이다.  
  
드라이버 : 입력신호를 반전관계 출력으로 변환  
리시버 : 반전관계 입력을 받아 단일 신호로 변환  
  
잡음이 너무 많은 경우에 한계가 있는데, 공통 모드 판별피라는 부품 정격 중 하나로  
처리 가능한 잡음의 양을 넘어서는 경우에는 잡읍을 처리하기 힘들다.  
  
**연선 케이블링**  
한 쌍의 선을 서로 꼬아 전기적으로 연인이 서로 허리에 손을 두르는 것 같은 효과를 만드는 것으로  
두 쌍이 연결이 강해져서 큰 잡음에도 견고하게 유지할 수 있다.  
  
## 전파 지연
입력의 변화가 출력에 영향을 미칠 때까지 걸리는 시간으로  
정확한 측정값이 아닌 제조 과정과 온도에 따라 생기는 편차와  
게이트 출력에 도달하기까지 연결된 구성 부품의 수에 따라 결정되는 통계적 측정값  
  
알고리즘의 시간 복잡도처럼 전파 지연에도 최소/최대 지연이 존재하고  
실제 지연은 둘 사이의 어떤 값인데, 설계자들은 최악의 경우를 가정해 최대 지연도 생각해야 한다.  
  
PLH는 0(Low) &rarr 1(High)에 걸리는 지연 시간이고, PHL은 1 &rarr 0에 걸리는 지연 시간이다.  
  
## 출력 유형
### 토템폴 출력
일반적인 게이트 출력으로 트랜지스터가 세로로 나란히 늘어서 있는 형태다.  
  
### 오픈 컬렉터 / 오픈 드레인 출력
원하는 출력이 0이라면 아무 문제 없지만 1이라면 그냥 떠 있고 출력값은 알 수 없다.  
  
### 트라이스테이트 출력
상태가 세 가지인 출력으로, 세 번째 상태는 꺼진 상태다.  
이를 켜고 끄기 위한 추가 입력인 활성화가 존재한다.  
  
# 게이트를 조합한 복잡한 회로
소규모, 중간 규모, 대규모, 초대규모 집적회로 등으로 불리는 패키지들을 사용해  
개별 부품을 사용해 만들 때보다 필요한 부품의 수를 줄여 더 저렴하고 작은 시스템을 만들 수 있다.  
  
## 가산기
두 비트의 덧셈은 XOR, 올림은 AND 게이트인데, 이를 이용해 두 개의 입력으로  
비트를 더하는 연산을 수행하는 반 가산기를 만들 수 있다.  
  
반이 붙은 이유는 다른 자리에서 올라오는 올림을 처리하려면 세 번째 입력이 필요하기 때문이다.  
  
그래서 각 비트의 합을 계산하기 위해 올림 처리용 입력을 추가해  
세 입력 중 2개 이상이 1일 때 올림이 발생하는 전가산기가 있다.  
  
## 디코더
비트를 정수로 바꾸어주는데 이를 이용해 디스플레이 제어에 사용할 수 있다.  
  
## 디멀티플렉서
디코더를 사용해 만들 수 있는 것으로 디먹스라고도 부른다.  
입력을 몇 가지 출력 중 한 곳으로만 전달하는 역할을 한다.  
즉, 입력에 따라 여러 출력 중 한 곳으로 전달한다.  
  
## 실렉터 (멀티플렉서, 먹스)
여러 입력 중 한 입력을 선택하는 것으로 실행활에서 볼 수 있는데  
토스터 오븐의 여러 선택지가 있는 다이얼을 실렉터 스위치라고 볼 수 있다.
  
# 간단 흐름 정리
## 디지털 컴퓨터의 사례
1. 아날로그는 연속적인 값을 표현할 수 있다.
2. 하지만 아날로그의 표현값의 범위는 물리적인 크기에 비례한다.
3. 무지막지하게 큰 아날로그 계산기를 들고 다닐 수 없다.
4. 사용한다 해도 관리 비용이 엄청날 것이다.
5. 그래서 연산을 쉽게 할 수 있고 전력 소모가 적은 이산적인 디지털을 사용하게 되었다.
6. 하드웨어 내부에서는 전자가 움직인다.
7. 전자의 속도는 고정된 값이기에 이를 올릴 수는 없다.
8. 그래서 조절할 수 있는 하드웨어의 크기를 작게 만든다.
9. 하드웨어가 작아지면 그만큼 전자가 이동할 거리가 줄어든다.
10. 이는 전자 이동에 소모되는 에너지가 감소해 전력 소모가 줄고, 열 발생도 감소한다는 것이다.
11. 즉, 하드웨어가 작아질수록 계산 속도와 효율이 증가하다.
12. 하지만 하드웨어가 작을 수록 내부에서 간섭이 발생할 가능성이 올라간다.
13. 이는 멀리 떨어진 물체에도 영향을 주는 전자의 전자기력이라는 성질 때문이다.
14. 작아질수록 내부의 제한된 공간으로 인해 구성 요소들의 간격이 가까워진다.
15. 따라서 간섭 현상인 누화 현상 방지를 위해 잡음 내성이 필요하다.
16. 그래서 판정 기준을 활용하는 디지털 회로를 사용하게 되었다.
17. 하지만 입력은 대부분 현실 세계의 값인 아날로그 값들이다.
18. 그래서 아날로그 값을 디지털 값으로 바꿔줘야 한다.
19. 그래서 입력에 대한 출력을 표현하는 전이함수가 있고
20. 전이 함수에서 게인을 높이면 출력이 바뀌게 되는데
21. 전이함수가 좀 더 가파르게 변하고 더 높이면 확 변하게 된다.
22. 즉, 입력에 따른 출력의 변화가 확 달라지게 되는데
23. 이는 게인이 낮을 때는 아날로그에 가깝고 높으면 디지털에 가까워진단 것
24. = 아날로그 값을 디지털 값으로 바꾸는 것
25. 전이함수에서 아날로그값에서 디지털값으로 변하는 구간을 문턱값이라 한다.
26. 그리고 10진수의 문턱값을 다 만드는 것보단 2진수의 문턱값을 만드는게 간단하다
27. 그래서 2비트를 사용하는 것이다.
    
## 간단한 전기 이론 가이드
1. 스위치가 닫힌 상태가 전기가 흐르고, 열린 상태가 전기가 흐르지 않는다.
2. 전기가 전파되는데 시간이 소요되는 현상이 전파지연이다.
3. 선 내부의 전자가 있는 금속을 도체, 도체를 둘러싼 외부의 부도체라고 한다.
4. 이 전선에서 전자의 이동 흐름을 제어하는 것이 스위치다.
5. 전압에 의해 전기의 이동 세기가 달라진다. (단위는 볼트)
6. 전기가 흐르는 양은 전류라고 한다. (단위는 암페어)
7. 전기 선의 굵기 또는 스위치의 구성처럼 전류의 흐름에 영향을 미치는 것을 저항이라 한다. (단위는 옴)
8. 전류 = 전압/저항
9. 극은 하나로 연결되어 이동하는 스위치의 수를 의미한다. (손)
10. 투는 접점을 의미한다. (선택지)
11. 근원에서 나온 전기는 구성 요소를 지나 다시 근원으로 돌아가 전기 시스템을 회로라고 부른다.
  
## 비트를 처리하기 위한 하드웨어
1. 전기와 자기 사이의 관례를 활용한 릴레이가 있다.
2. 릴레이는 전자석을 이용해 스위치를 움직인다.
3. 릴레이로 인버터를 구현할 수 있다.
4. 인버터로 모든 논리 연산을 표현할 수 있다.
5. 그래서 복잡한 논리를 만들 수 있다.
6. 릴레이의 전이함수는 문턱값 부분이 수직이라 디지털 값을 만들 수 있다.
7. 하지만 느리고 전기를 많이 소모하고, 먼지나 이물질 등으로 동작에 문제가 생길 수 있다.
8. 물체를 충분히 가열하면 생기는 열전자 방출 현상으로 진공관을 만들었다.
9. 진공관은 투수(캐소드와 히터), 타자(그리드), 포수(애노드)로 구성되어 있다.
10. 여기서 타자가 치냐 못치냐에 따라 포수한테 공이 가냐 안가냐가 정해진다.
11. 따라서 그리드가 스위치의 역할이라고 볼 수 있다.
12. 움직이는 부분이 없기 때문에 릴레이보다 훨씬 빠르다.
13. 하지만 가열을 해야하기 때문에 뜨겁고 깨지기 쉽다.
14. 반도체는 도체와 부도체 사이를 오갈 수 있다.
15. 이를 이용해 만든게 트랜지스터다.
16. 도체가 가늘고 얇아지면 저항이 늘어나고, 저항이 커지면 열이 발생한다.
17. 열이 발생하면 반도체는 쉽게 녹을 수 있다.
18. 그래서 트랜지스터를 작게 만들 때 열을 제거하는 문제가 매우 중요하다.
19. 트랜지스터는 반도체 물질로 이루어진 기판 또는 슬랩 위에서 만들어진다.
20. 실리콘 기판 위에 대량의 트랜지스터를 투영해 현상해서 개별적인 트랜지스터로 잘라낼 수 있다.
21. 그래서 대량 생산이 가능하다.
22. 간단한 연산을 수행하는 회로를 트랜지스터로 만드려면 많은 부품이 필요하다.
23. 하지만 집적 회로를 사용하면 복잡한 시스템을 트랜지스터 하나를 만드는 비용으로 만들 수 있다.
24. 생긴 모양 때문에 집적 회로를 칩이라고 부른다.
  
## 논리 게이트
1. 칩에 미리 들어가 있는 회로를 논리 게이트라 한다.
2. 논리 게이트가 들어간 칩을 선으로 연결해 복잡한 회로를 쉽게 만들 수 있다.
3. 악...

# 추가 자료
## 1. 잡음이 오히려 필요한 경우
**난수 생성**  
암호학이나 머신러닝에서 난수를 생성해야 할 때, 예측 불가능한 진짜 랜덤 값을 얻고 싶다면  
전자적 잡음이나 방사선 변동 같은 물리적 잡음을 이용해 난수를 만들기도 한다.  
  
**디더링**  
아날로그를 디지털로 변환할 때 고의로 약간의 잡음을 추가해 부드러운 결과를 얻을 수 있는 기술로  
이미지의 경우에는 색깔 간의 차이의 변화를 부드럽게 보이게 하기 위해 작은 점들을 랜덤하게 배치하고  
오디오의 경우에는 높은 비트에서 낮은 비트로 줄일 때 생기는 노이즈나 왜곡을 자연스럽게 하기 위해  
소리 사이에 잡음을 넣어 기존 소리와 비슷하게 만들 수 있다.  
  
## 2. 디지털보다 아날로그가 더 안정적인 사례
**고온/저온의 환경**  
디지털 장치는 온도에 예민하다보니 디지털 장치에 문제가 생길 정도의 온도에서는  
오히려 아날로그 장치가 더 안정적인 경우도 존재한다.  
(예를 들면, 극한의 환경인 산업용에서는 디지털 센서가 아닌 아날로그 센서를 사용하기도 한다.)  
  
**전자기 간섭 환경**  
디지털 신호는 특정 주파수에서 강한 간섭을 받아 신호 왜곡이 발생하거나  
영화 같은 곳에서 나오는 EMP(전자기 펄스) 공격 같은 것에 취약하다.  
  
반면에 아날로그는 신호가 연속적이라 간섭에 극단적으로 영향을 받지 않는다.
  
## 3. 전파 지연을 장점으로 활용한 사례
**GPS**  
위성과 수신기 간의 전파 지연 시간을 이용해 거리를 계산하기 때문에  
전파 지연을 오차가 아닌 정확한 거리 계산의 도구로 활용하고 있다.  
  
**에코와 리버브**  
오디오 기술에서 지연을 이용해 소리를 반복적으로 혹은 울리게 하는 등  
다양한 효과를 만들때 사용할 수 있다.  
  
**양자 암호 통신**  
빛의 이동 속도를 이용해 도청 여부를 판단하기도 하는데  
예상한 시간보다 신호가 빨리 도착하면 도청 시도가 있었음을 의미한다.  
