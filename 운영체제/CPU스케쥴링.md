# CPU 스케쥴링

CPU는 한 번에 한 가지 프로세스만 처리할 수 있다. 메모리에 올라온 프로세스들 중 어떤 프로세스를 먼저 처리할지 일들의 순서를 정하는 것을 CPU 스케쥴링이라고 한다. 다르게 말하면, Ready queue에 있는 프로세스들의 실행 순서를 정하는 것이다.

-> 스케쥴링을 사용함으로써 다중 프로그래밍이 가능해지고 CPU 이용률을 극대화시킬 수 있다.

`다중 프로그래밍?` : CPU가 쉬지 않고 항상 실행되게 함으로써 CPU 이용률을 높이는 것

</br>

### 프로세스 상태

![img](https://img1.daumcdn.net/thumb/R800x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F99E85E3A5C460F1906)

**new : 프로세스 생성중**

- 프로세스를 생성하고 있는 단계로 커널 공간에 PCB가 만들어진 상태

**ready : 프로세스가 CPU를 기다리는 상태**

- 프로세스가 메모리에 적재된 상태로 실행하는데 필요한 자원을 모두 얻은 상태

- 아직 CPU를 받지는 않았지만 CPU를 할당 받으면 바로 실행 가능한 상태

- ready상태를 가지는 여러개의 프로세스들이 존재할 수 있음 (ready queue)

**running : 프로세스가 CPU를 할당받아 명령어를 수행중인 상태**

- 일반적으로 CPU가 하나이기 때문에, 여러 프로세스가 동시에 실행되도 실제로 실행중인 프로세스는 매 시점 하나 뿐임 

**waiting : 프로세스가 어떤 사건(event)이 일어나기를 기다림**

- 프로세스가 실행(running)되다가, 할당받은 CPU를 반납하고 입/출력 작업이 완료를 기다리는 등 특별한 사건을 기다림

**terminated : 프로세스의 실행 종료**

- 프로세스의 실행이 완료되고 할당된 CPU를 반납, 커널공간내의 PCB는 남아 있음

</br>

#### 상태 전이

**admit(new -> ready)** : OS에 의해 프로세스를 승인

**dispatch(ready -> running)** : 처리기가 프로세스를 수행하기 위해 CPU 할당(시간도 할당됨)

**interrupt(running -> ready)** : 할당된 시간이 지나면 time out interrupt가 발생

**event wait(running -> wait)** : time out 전에 I/O 요청이 발생(sleep,block)

**event completion(waiting -> ready)** : I/O 요청이 완료되면 다시 ready 생태로 전이

**exit(running -> terminated)** : 프로세스 종료

</br>

### Scheduling Criteria

스케쥴링 알고리즘의 성능을 평가하는 척도는 다음과 같다.

1. **프로세서 이용률(CPU utilization)** : 시간당 CPU를 사용한 시간의 비율 
2. **처리율(Throughput)** : 시간당 처리한 작업의 비율
3. **반환 시간 또는 소요 시간(Turnaround time)** : 프로세스가 시작해서 끝날 때까지 걸린 시간
4. **대기 시간(Waiting time)** : Ready Queue에 들어가서 CPU를 할당 받기까지 기다린 시간
5. **반응 시간 또는 응답 시간(Response time)** : Interactive system에서 의뢰한 시간에서부터 반응이 시작되는 시간

**시스템 입장**에서의 CPU 스케줄링 성능에서 중요한 것 - `CPU Utilization` + `Throughput` (클수록 좋음)

**사용자 입장**에서의 CPU 스케줄링 성능에서 중요한 것 -  `Turnaround Time` + `Waiting Time` + `Response Time` (작을수록 좋음)

</br>

### 스케쥴링 알고리즘의 종류

스케쥴링 알고리즘에는 선점과 비선점이라는 개념이 존재한다.

- **선점(Preemptive)** : 한 프로세스가 CPU를 할당받아 실행중이라도 다른 프로세스가 현재 프로세스를 중지 시키고 CPU를 강제적으로 뺏을 수 있는 스케줄링 방식
- **비선점(Non-preemptive)** : 한 프로세스가 CPU를 할당받아 실행중이라면 다른 프로세스들이  CPU를 강제적으로 뺏을 수 없는 스케줄링 방식

비유를 하자면 선점 방식은 위급한 환자 먼저 진찰하는 응급실과 같고, 비선점 방식은 앞 사람이 끝나야만 업무를 볼 수 있는 은행 창구와 같다.

</br>

#### 1. FCFS(First-Come First-Served)

- <u>먼저 요청한 프로세스 순으로 스케쥴링(==선입선출)</u>
- 비선점 방식
- **단점**
  : 프로세스를 장시간 독점하는 경우 오랜 시간 기다려야 함(평균 응답 시간이 길어짐)



#### 2. SJF(Shortest-Job-First)

- <u>CPU 작업 시간(CPU burst time)이 짧은 프로세스 순으로 할당</u>
- 비선점 방식
- 작업 시간이 동일한 경우 FCFS 정책을 따름
- 평균 대기 시간 최소화



#### 3. SRTF(Shortest-Remaining-Time-First)

- SJF의 선점형 스케쥴링 방식
- <u>남은 프로세스의 burst time보다 더 짧은 프로세스가 도착하면 CPU를 빼앗음</u>
- 프로세스가 새로 들어올 때마다 갱신

- **단점**
  : Starvation(기아 현상)이 발생할 수 있음. 
    잦은 선점으로 문맥 교환 오버헤드 증대
    새로운 프로세스가 들어올 때마다 스케쥴링이 변경되어 burst time을 정확히 예측하기 어려움 



#### 4. RR(Round-Robin)

- <u>각 프로세스는 동일한 크기의 할당 시간(time quantum 혹은 time slice)을 가지고, 할당 시간이 지나면 프로세스는 선점 당하고 Ready Queue의 맨 뒤로 가게 됨.</u>
- 시분할 시스템을 위해 설계
- 선점형 방식
- 평균 대기 시간은 조금 길어질 수 있지만, 응답 시간은 짧아진다.
- **단점** 
  : 할당 시간이 커지면 FCFS와 같아져 비효율적일 수 있다.
    할당 시간이 지나치게 작으면, 문맥교환이 대량으로 발생해 오버헤드가 증가한다.



#### 5. 우선순위 스케쥴링(Priority Scheduling)

- <u>우선순위가 높은 프로세스에게 CPU를 할당</u>
- 선점형 방식 : 새로 도착한 프로세스의 우선순위가 현재 실행되는 프로세스보다 높으면 CPU 선점
- 비선점형 방식 : 더 높은 우선순위의 프로세스가 들어오면 Ready queue의 head 부분에 넣는다.
- **단점**
  : Starvation 발생
    무한 정지(infinite blocking) - 프로세스가 CPU를 사용할 준비가 되었지만 우선순위가 낮을 경우 무한 대기
  => **[해결책] Aaging 기법** - 큐에 남아있던 시간에 비례해 가중치 부여(우선 순위를 1씩 증가)



#### 6. 다단계 큐(Multilevel-Queue)

- <u>Ready Queue를 여러 개의 큐로 분류하여 각 큐가 각각 다른 스케쥴링 알고리즘을 가지는 방식</u>
- 각 큐는 독자적 스케줄링 사용 가능
- 대화형, 배치(Background)등의 프로세스 성격에 따라 우선순위 부여
- 큐들 간의 프로세스 이동이 불가하기 때문에 스케줄링 부담이 적지만 유연성이 떨어짐
- 우선순위가 낮은 프로세스가 오랬동안 CPU 할당을 기다리는 기아 현상이 발생할 수도 있음

![img](https://blog.kakaocdn.net/dn/q0zDa/btqD54RB9AY/WwFDf4ByO9vxDgwWPEP1Q0/img.png)



#### 7. 다단계 피드백 큐(Multilevel-Feedback-Queue)

- 기존 다단계 큐 방식은 특정 프로세스가 큐에 고정되는 방식. 반면, 다단계 피드백 큐에서는 <u>큐와 큐 사이에 프로세스가 이동하는 것을 허용하는 방식</u>
- 낮은 우선 순위에 너무 오래 대기하는 프로세스는 높은 우선순위의 큐로 이동 할 수 있다.

</br>

#### [참고]

> - https://m.blog.naver.com/4717010/60207137085
> - https://dduddublog.tistory.com/23
> - https://jcsoohwancho.github.io/2019-10-25-%EC%84%A0%EC%A0%90%ED%98%95-%EC%8A%A4%EC%BC%80%EC%A5%B4%EB%A7%81&%EB%B9%84%EC%84%A0%EC%A0%90%ED%98%95-%EC%8A%A4%EC%BC%80%EC%A5%B4%EB%A7%81/
> - https://m.blog.naver.com/PostView.nhn?blogId=xowns4817&logNo=221075021877&proxyReferer=https:%2F%2Fwww.google.com%2F
> - https://velog.io/@ss-won/OS-CPU-Scheduling-Algorithm
> - https://velog.io/@yerin4847/CPU-%EC%8A%A4%EC%BC%80%EC%A4%84%EB%A7%81CPU-Scheduling
> - https://jhnyang.tistory.com/8?category=815411
> - https://zzsza.github.io/development/2018/07/29/cpu-scheduling-and-process/
> - https://wonit.tistory.com/108
> - https://cocoon1787.tistory.com/124
> - https://jhnyang.tistory.com/28