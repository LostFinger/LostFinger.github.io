---
title: 스터디 목표
tags: 커리큘럼
author: pilsoo lee
show_author_profile: true
---

<h2>1차시 <a href="http://www.kocw.net/home/search/kemView.do?kemId=1125218">링크</a></h2>
>이 과목에서는 컴퓨터 시스템의 구성, 동작원리와 설계를 다룬다. 특히 컴퓨터 성능, 명령집합, 제어와 마이크로프로그래밍, 파이프라인, 정수와 부동 소수점수 연산, 기억부 시스템, 입출력 부시스템, 고급 컴퓨터구조 등과 같은 내용에 중점을 둔다. 교과목을 통해 고속 Data Network 시스템에 들어가는 여러 Processors를 이해하고, 이들을 설계 또는 Programming 할 수 있는 능력을 배양한다.

#

<h3>챕터 학습목표</h3>
>퍼포먼스의 정의를 이해할 수 있다.

#

<h3>1-1. Introduction</h3>
<h4>1.무어의법칙</h4>
1965 Gordon moore 
<div>
    <img src="Moor's Law.png" width="400" height="200" />
</div>

> 매 2년마다 컴퓨터 성능이 2배씩 증가한다.

<h4>2. Classes of Computers</h4>

1. Personal Computer
    
    1. 일반적 용도에 사용
    2. 가성비에 민감
    
2. Server Computer
    
    1. 고가 , 대용량 , High reliability
    
3. 슈퍼컴퓨터
    
    1. high end scientific 문제, 복잡한 문제 해결과 같은 특수목적에 사용
    2. 매우 고가, 고전력 사용량, 높은 유지비
    
4. Embedded Computers
    
    1. 큰 시스템의 컴포넌트 형태로 주로 사용
    2. 저전력, 저성능, 낮은 비용이 필요로 한 곳에 사용  

<h4>3. The PostPC Era</h4>

1. Personal Mobile Device (PMD)
    
    1. 배터리로 동작
    2. 인터넷으로 연결
    3. Hundreds of dollars (저가)
    4. 스마트폰, 태블릿, 스마트 안경
2. Cloud computing
    
    1. WareHouse Scale Computers (WSC) : 특정한 데이터를 처리해주는 하나의 서버 팜
    2. Software as a Service (SaaS) : 필요한 소프트웨어를 서비스로 제공받을 수 있다.
    3. 필요한 소프트웨어의 일부분은 PMD로 제공받고, 중요한,복잡한 로직은 클라우드 컴퓨터로 부터 전달.
    4. 아마존과 구글이 있다.

<h4>4. 이 챕터에서 배울 목표</h4>

1. 프로그램이 어떻게 머신랭귀지로 바뀔 것인가.
    
    1. 어떻게 하드웨어가 인스트럭션을 실행시키나.
2. 하드웨어 소프트웨어 인터페이스에 대해서.
3. 퍼포먼스의 정의
    
    1. 퍼포먼스를 어떻게 정의할 것인가.
    2. 퍼포먼스를 어떻게 향상시킬 것인가.
4. 아키텍쳐들이 어떻게 성능을 향상시킬 것인가.
5. Parallel Processing이 뭘까?

<h4>5. Understanding Performance</h4>

> 성능이 뭘까.

1. 알고리즘
    오퍼레이션의 수로 결정
    > 계산이 적은 알고리즘일수록 성능이 Up
2. 프로그래밍 랭귀지, 컴파일러, 아키텍쳐
    
    1. Instruction의 수로 결정
        > Instruction: 실제 CPU 가 수행하는 명령어.
3. 프로세서, 메모리 시스템
    
    1. 얼마나 명령어를 빨리 실행할 것인가.
        > 단위시간당 얼마나 많은 명령어를 처리하는가.
4. I/O System.
    
    1. IO Operation의 수로 결정.
        > 단위 시간당 얼마나 많은 오퍼레이션을 처리하는지.
#

<h3>1-2.Eight Great Ideas In Computer Architecture</h3>
<h4>1. 컴퓨터 구조에서 일반적으로 사용되는 아이디어</h4>

1. Design for Moore's Law
2. Use abstraction to simplify design.
    >복잡한 문제를 단순화 하기위해 추상화(abstraction)를 사용
3. Make the common case fast.
    > 아주 많이 사용되는 케이스를 빠르게 하는것이 성능향상에 도움
4. Performance via parallelism.
    > 병렬화를 통해 성능을 향상.
5. Performance via pipelining
    > 4장에서 배울 파이프라인을 통해 성능을 향상.
6. Performance via prediction
    > 5장에서 배울 prediction 을 통해 성능을 향상.
7. Hierarchy of memories.
    > 5장에서 배울 Memory Hierarchy.
8. Dependability via redundancy.
    > Redundancy 를 통해 dependability를 향상.  
    Raid(6장)
    
> 여기서 사용되는 8가지 아이디어가 컴퓨터 성능을 향상.

#

<h3>1-3.Below your computer</h3>
<h4>1.Below your computer</h4>

1. Application Software
    
    1. 고수준 언어로 작성
    2. 엑셀, 파워포인트 등
2. System software.
    
    1. 운영체제, 컴파일러와 같은 기본적인 소프트웨어를 의미
    2. 컴파일러
        1. 하이레벨 랭귀지를 머신코드로 변경
    3. 어플리케이션이 사용되는 서비스를 제공.
        1. I/O 핸들링
        2. 메모리, 스토리지 관리.
        3. 태스크를 스케쥴링하고, 자원을 공유
3. Hardware.
    
    1. 실제 만질수 있는 물리적 장치
    2. Processor,Memory, I/O Controller
    
<h4>2.Levels of Program Code.</h4>
1. High-Level language
    1. 문제 해결
2. Assembly language
    1. 머신 인스트럭션의 텍스트 표현
3. Hardware representation
    1. 기계가 이해할 수 있는 코드.
    2. 0,1 의 조합.