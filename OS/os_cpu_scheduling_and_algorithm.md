# CPU 스케줄링

CPU 이용률을 극대화하기 위해서는 멀티프로그래밍(multiprogramming)이 필요하다. 하지만 만약 CPU core가 하나라면 한 번에 하나의 프로세스만 실행 가능할 것이다. 이때 필요한 것이 CPU 스케줄링이다. 

즉, CPU 스케줄링은 언제 어떤 프로세스에 CPU를 할당할지 결정하는 작업이라고 할 수 있다. 

# 배경지식

## 입출력 집중 프로세스와 CPU 집중 프로세스

프로세스의 종류마다 입출력 장치를 이용하는 시간과 CPU를 이용하는 시간의 양에는 차이가 있다. 비디오 재생이나 디스크 백업 작업을 담당하는 프로세스와 같이 입출력 작업이 많은 프로세스가 있는 반면, 복잡한 수학 연산, 컴파일, 그래픽 처리 작업을 담당하는 프로세스와 같이 CPU 작업이 많은 프로세스도 있다. 전자를 입출력 집중 프로세스(I/O bound Process)라고 하고, 후자를 CPU 집중 프로세스(CPU bound process)라고 한다.

입출력 집중 프로세스는 CPU 집중 프로세스에 비해 CPU를 덜 사용하며, 입출력을 위한 대기 상태에 더 많이 머무른다. 따라서 입출력 집중 프로세스와 CPU 집중 프로세스가 동시에 CPU 자원을 요구했을 경우, 입출력 집중 프로세스를 먼저 실행시켜 입출력 장치를 작동시키면, 입출력 작업이 완료되기 전까지 해당 프로세스는 대기 상태에 머무르기 때문에 다른 프로세스가 CPU를 사용할 수 있다.

## 스케줄링 큐

프로세스의 처리 순서는 스케줄링 큐로 관리된다. 대표적인 큐로 준비 큐와 대기 큐가 있다. 준비 큐는 CPU를 이용하고 싶은 프로세스들이 줄을 서는 큐이고, 대기 큐는 입출력장치를 이용하기 위해 대기 상태로 들어간 프로세스들이 서는 줄을 의미한다.

<p align="center"><img src="./img/os_cpu_scheduling_and_algorithm_1.PNG" alt="os_cpu_scheduling_and_algorithm_1" width=700></p>

## 선점형 스케줄링과 비선점형 스케줄링

갑자기 어떤 프로세스가 CPU를 당장 사용하길 원한다면 어떻게 대처할지가 선점형과 비선점형을 나누는 기준이다. 선점형 스케줄링은 프로세스가 CPU를 비롯한 자원을 사용하고 있더라도 운영체제가 프로세스로부터 자원을 강제로 빼앗아 다른 프로세스에 할당하는 스케줄링 방식을 말한다. 반면 비선점형 스케줄링은 하나의 프로세스가 자원을 사용하고 있다면 그 프로세스가 종료되거나 스스로 대기상태에 접어들기 전까진 다른 프로세스가 끼어들 수 없는 스케줄링 방식을 말한다.

현재 대부분의 운영체제는 선점형 스케줄링 방식을 차용하고 있지만, 선점형 스케줄링과 비선점형 스케줄링은 각기 장단점을 가지고 있다. 선점형 스케줄링은 더 급한 프로세스가 언제든 끼어들어 사용할 수 있는 스케줄링 방식이므로 어느 한 프로세스의 자원 독점을 막고 프로세스들에 골고루 자원을 배분할 수 있다는 장점이 있지만, 그만큼 문맥 교환 과정에서 오버헤드가 발생할 수 있다.

```
오버헤드 (Overhead)
어떤 일을 처리할 때 추가적으로 소모된 자원
예를 들어, 프로그램의 실행흐름 도중에 동떨어진 위치의 코드를 실행시켜야 할 때, 추가적으로 시간, 메모리, 자원이 사용되는 현상
```

```
문맥 교환 (Context Switching)
CPU가 어떤 하나의 프로세스를 실행하고 있는 상태에서 인터럽트 요청에 의해 다음 우선 순위의 프로세스가 실행되어야 할 때
기존의 프로세스의 상태 또는 레지스터 값(Context)을 저장하고 CPU가 다음 프로세스를 수행하도록
새로운 프로세스의 상태 또는 레지스터 값(Context)를 교체하는 작업
```

반면 비선점형 스케줄링은 문맥 교환의 횟수가 선점형 스케줄링보다 적기 때문에 문맥 ㄹ교환에서 발생하는 오버헤드도 적지만, 하나의 프로세스가 자원을 사용 중이라면 당장 자원을 사용해야 하는 상황에서도 무작정 기다리는 수밖에 없다. 즉, 모든 프로세스가 골고루 자원을 사용할 수 없다는 단점이 있다.

# CPU 스케줄링 알고리즘

## 1. 선입 선처리 스케줄링

선입 선처리 스케줄링은 FCFS 스케줄링(First Come First Served Scheduling)이라고도 부른다. 이는 단순히 준비 큐에 삽입된 순서대로 프로세스들을 처리하는 비선점형 스케줄링 방식이다. 이러한 방식은 CPU를 오래 사용하는 프로세스가 먼저 도착하면 다른 프로세스들은 그 프로세스가 CPU를 사용하는 동안 무작정 기다리는 수 밖에 없다. 이를 호위 효과라고 한다.

```
호위 효과 (Convoy Effect) (=콘보이 효과)
CPU 사용시간이 긴 프로세스에 의해 사용시간이 짧은 프로세스들이 오래 기다리는 현상
호위 효과가 발생할 경우 CPU와 장치 이용률이 낮아진다.
```

<p align="center"><img src="./img/os_cpu_scheduling_and_algorithm_2.PNG" alt="os_cpu_scheduling_and_algorithm_2" width=700></p>

## 2. 최단 작업 우선 스케줄링

호위 효과를 해결하기 위한 가장 단순한 방법은 CPU 사용 시간이 짧은 순서대로 실행하는 것이다. 이를 최단 작업 우선 스케줄링 혹은 SJF(Shortest Job First Scheduling) 스케줄링이라고 한다. 이는 비선점형 스케줄링이며 비슷한 알고리즘 중 선점형으로 구현된 것이 4번에서 소개할 최소 잔여 시간 우선 스케줄링이다.

<p align="center"><img src="./img/os_cpu_scheduling_and_algorithm_3.PNG" alt="os_cpu_scheduling_and_algorithm_3" width=700></p>

## 3. 라운드 로빈 스케줄링

라운드 로빈 스케줄링(Round robin Scheduling)은 선입 선처리 스케줄링에 타임 슬라이스라는 개념이 추가된 것이다. 타임 슬라이스란 각 프로세스가 CPU를 사용할 수 있는 정해진 시간을 의미하고, 그 타임 슬라이스만큼 돌아가면서 CPU를 사용하는 알고리즘이다. 예를 들어 CPU 사용 시간이 11ms, 3ms, 7ms인 프로세스 A, B, C가 있고 타임 슬라이스는 4ms인 라운드 로빈 스케줄링을 한다면 아래 그림과 같이 수행된다.

<p align="center"><img src="./img/os_cpu_scheduling_and_algorithm_4.PNG" alt="os_cpu_scheduling_and_algorithm_4" width=700></p>

라운드 로빈 스케줄링에서는 타임 슬라이스의 크기가 매우 중요하다. 타임 슬라이스가 지나치게 크면 선입 선처리 스케줄링과 다를 바가 없어 호위 효과가 발생할 수 있으며, 반대로 타임 슬라이스가 지나치게 작으면 문맥 교환에 드는 비용이 커져 프로세스 처리보다 프로세스 전환에 더욱 많은 자원을 사용할 수 있다.

## 4. 최소 잔여 시간 우선 스케줄링

최소 잔여 시간 우선 스케줄링은 SRT 스케줄링(Shortest Remaining Time Scheduling)이라고도 하며, 최단 작업 우선 스케줄링 알고리즘과 라운드 로빈 알고리즘을 합친 방식이다. 각 프로세스들은 정해진 타임 슬라이스만큼 CPU를 사용하되, CPU를 사용할 다음 프로세스로는 남아있는 작업 시간이 가장 적은 프로세스가 선택된다.

## 5. 우선순위 스케줄링

우선순위 스케줄링이란 프로세스들에 우선순위를 부여하고 우선순위가 높은 순서대로 실행하는 알고리즘을 말한다. 따라서 앞서 설명했던 최단 작업 우선 스케줄링이나 최소 잔여 시간 우선 스케줄링 알고리즘은 우선순위 스케줄링의 일종이라고 볼 수 있다.

이러한 방식의 단점으로는 기아(Starvation) 현상이 있다. 우선순위가 높은 프로세스만 계속 먼저 실행되기 때문에 후순위의 프로세스는 계속해서 뒤로 밀리는 것이다. 이를 방지하기 위한 대표적인 방법이 바로 에이징(Aging)이다. 마치 나이를 먹는 것처럼 시간이 흐름에 따라 대기 중인 프로세스의 우선순위를 높이는 방법을 말한다.

## 6. 다단계 큐 스케줄링

다단계 큐 스케줄링은 우선순위 스케줄링의 업그레이드 버전이다. 각 우선순위별로 준비 큐를 여러 개 사용하는 방식으로, 우선순위가 가장 높은 큐에 있는 프로세스들을 먼저 처리하고 해당 큐가 비어있으면 다음 우선순위 큐로 넘어가는 방식이다.

<p align="center"><img src="./img/os_cpu_scheduling_and_algorithm_5.PNG" alt="os_cpu_scheduling_and_algorithm_5" width=700></p>

이 방식의 장점으로는 프로세스 유형별로 우선순위를 구분하여 실행하기 편리하다는 것이다. 예를 들어 어떤 큐에는 우선순위가 비교적 높아야 하는 입출력 집중 프로세스를 삽입하고, 또 다른 큐에는 비교적 우선순위가 낮아도 되는 CPU 집중 프로세스를 삽입하는 것이 가능하다. 또한 큐별로 타임 슬라이스나 스케줄링 알고리즘을 다르게 사용할 수 있다.

반면 단점으로는 프로세스들이 큐 사이를 이동할 수는 없기 때문에 우선순위가 낮은 큐는 계속해서 실행이 지연되는 기아 현상이 발생한다. 이러한 단점을 보완한 스케줄링 알고리즘이 바로 마지막으로 소개할 다단계 피드백 큐 스케줄링이다.

## 7. 다단계 피드백 큐 스케줄링

다단계 피드백 큐 스케줄링은 각 프로세스들이 큐 사이를 이동할 수 있다는 점이 다르다. 다단계 피드백 큐는 이러한 사실을 두 가지 방식으로 활용하고 있다.

우선 새로 들어오는 프로세스를 가장 높은 우선순위 큐에 삽입한 다음, 타임 슬라이스 안에 완료되지 않으면 다음 우선순위 큐로 삽입하는 것이다. 이렇게 함으로써 CPU를 비교적 오래 사용하는 CPU 집중 프로세스들은 자연스럽게 우선순위가 낮아지고, 입출력 집중 프로세스는 자연스럽게 높은 우선순위에서 실행이 완료된다.

<p align="center"><img src="./img/os_cpu_scheduling_and_algorithm_6.PNG" alt="os_cpu_scheduling_and_algorithm_6" width=700></p>

다만 이렇게만 스케줄링한다면 낮은 우선순위로 내려간 프로세스들은 너무 오래 기다려야 한다는 단점이 있기 때문에 에이징 기법을 활용하여 낮은 우선순위 큐에서 높은 우선순위 큐로 이동시키는 방식 역시 적용한다. 이로써 기아 현상을 예방할 수 있다.

<p align="center"><img src="./img/os_cpu_scheduling_and_algorithm_7.PNG" alt="os_cpu_scheduling_and_algorithm_7" width=700></p>

즉, 다단계 피드백 큐 스케줄링 알고리즘은 CPU 이용 시간이 길면 낮은 우선순위 큐로 이동시키고, 낮은 우선순위 큐에서 너무 오래 기다리면 높은 우선순위 큐로 이동시키는 알고리즘이다. 이 방식은 구현이 복잡하지만 가장 일반적인 CPU 스케줄링 알고리즘이라고 한다.

<p align="center"><img src="./img/os_cpu_scheduling_and_algorithm_8.PNG" alt="os_cpu_scheduling_and_algorithm_8" width=700></p>


### 출처

혼자 공부하는 컴퓨터구조 + 운영체제 (강민철 지음, 한빛미디어)<br>
https://imbf.github.io/computer-science(cs)/2020/10/18/CPU-Scheduling.html<br>
https://code-lab1.tistory.com/45<br>

