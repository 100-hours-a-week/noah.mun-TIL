# kakao bootcamp class

---

## VLANS 심화

https://min-310.tistory.com/175

위 블로그를 참조해야 함.

다른 방식에는 vlan database를 이용하여 VLAN을 생성하라고 나와있지만, 새로운 Cisco IOS에서는 conf t를 사용해 생성해야 함

# 코테 문풀 (그리디)

---

### [BOJ 11000번 강의실 배정](https://www.acmicpc.net/problem/11000)

```python
import heapq
import sys
input = sys.stdin.readline

n = int(input())
cnt = 0
que = []

for _ in range(n):
    start, end = map(int, input().split())
    heapq.heappush(que, (start, end))

que.sort(key = lambda x : (x[0], x[1]))

heap = [que[0][1]] # 3
for i in range(1, n):
    if heap[0] <= que[i][0]:
        heapq.heappop(heap)
    heapq.heappush(heap, que[i][1])
print(len(heap))
```

### 회고

<aside>
💡

내일 작성… 너무 졸림

</aside>

# 리눅스마스터 1급 1차 - 2020 10월 필기 기출 분석

---

### GRUB 환경 설정 파일

<aside>
📖

Rocky Linux 8 버전에 사용되는 GRUB 환경 설정 파일 → /boot/grub2/grub.cfg
심볼릭 링크 파일인 /etc/grub2/grub2.cfg 사용 가능

</aside>

- GRUB2 버전부터는 환경 설정 파일 수정 후에 `grub2-mkconfig` 명령을 실행해야 함
- GRUB 운영과 관련된 주요 설정들은 /etc/default/grub 파일에 있으므로 이 파일을 분석
    1. GRUB_TIMEOUT=5
        - GRUB 부트 화면에서 대기하는 시간을 초 단위로 지정하는 부분
    2. GRUB_DISTRIBUTOR=”$(sed ‘s, release .*$,,g’ /etc/system-release)”
        - GRUB 부트 화면에서 각 엔트리 앞에 보여질 리눅스 배포판 이름을 추출할 때 사용
    3. GRUB_DEFAULT=saved
        - 부트 화면에 제시된 목록 중에 기본적으로 부팅할 모드를 선택하는 항목으로 일반적으로 0부터 N에 해당하는 정수값 입력
    4. GRUB_DISABLE_SUBMENU=true
        - gurb-mkconfig에서 버전 번호가 가장 높은 커널에 대해 최상위 메뉴 항목으로 생성하고 다른 모드 커널 또는 복구 모드에 대한 대체 항목을 하위 메뉴로 생성 (true → 하위 메뉴 생성 ❌)
    5. GRUB_TERMINAL_OUTPUT=”console”
        - GRUB이 출력되는 터미널 장치를 설정하는 항목
    6. GRUB_CMDLINE_LINUX=”—-생략—- rhgb quiet”
        - 커널 인자값을 지정하는 항목
    7. GRUB_DISABLE_RECOVERY=”true”
        - 부트 메뉴 엔트리에 복구 모드를 표시할 것인지를 지정하는 항목 (true → 복구 모드 나오지 않음)
    8. GRUB_ENABLE_BLSCFG=”true “
        - 다양한 부트 로더 구현과 관리를 지원

---

### bash 기능

| ; | 단순히 한 줄에 여러 명령을 나열하기 위해 사용, 입력한 순서대로 순차처리 |
| --- | --- |
| || | 논리적 OR, 앞 명령이 성공이면 결과 출력, 그렇지 않으면 뒤의 명령을 실행하여 결과 출력 |
| && | 논리적 AND, 앞의 명령이 성공적으로 수행되어야먄 다음 명령을 수행 |

사용 예시

- `pwd ; ( cd / ; pwd ) ; pwd`
    - () 기호를 사용하여 그룹 명령을 실행한 후에는 원래의 상태로 전환
- `grep zzang /etc/passwd || echo “No zzang”`
    - zzang이라는 계정이 없으므로 No zzang 실행
- `grep posein /etc/passwd || echo “No posein”`
    - posein이 존재하므로 앞의 명령의 결과만 출력 (grep : 입력으로 전달된 파일의 내용에서 특정 문자열을 찾고자할 때 사용하는 명령어)

```bash
ls
// lin.txt
mv lin.txt joon.txt && ls
// joon.txt
mv lin.txt joon.txt && ls
// cannot stat 'lin.txt' : No such file or directory
```

- mv : 이름 변경 (1번 인자 파일이 존재할 경우 2번 인자로 이름 변경, 그렇지 않을 경우 에러 미시지 출력)

---

### 주요 환경 변수

| 변수 | 설명 |
| --- | --- |
| HOME | 사용자의 홈 디렉토리 |
| PATH | 실행 파일을 찾는 디렉토리 경로 |
| LANG | 셸 사용 시 기본으로 지원되는 언어 |
| TERM | 로그인한 터미널 종류 |
| PWD | 사용자의 현재 작업 디렉토리 |
| SHELL | 사용자의 로그인 셸 |
| USER | 사용자의 이름 |
| DISPLAY | X 윈도에서 프로그램 실행 시 출력되는 창 |
| PS1 | 프롬프트 변수 |
| PS2 | 2차 프롬프트 변수 |
| HISTFILE | 히스토리 파일의 절대 경로 |
| HISTSIZE | 히스토리 파일에 저장되는 명령어의 개수(줄 기준) |
| HISTFILESIZE | 히스토리 파일의 파일 크기 |
| HOSTNAME | 시스템의 호스트명 |
| MAIL | 도착한 메일이 저장되는 경로 |
| TMOUT | 사용자가 로그인한 후 일정 시간 동안 작업을 하지 않을 경우에 로그아웃시키는 시간 (단위 : 초) |
| UID | 로그인할 때 사용한 계정의 UID (Real UID) |
| EUID | 명령을 수행하는 주체의 UID (Effective UID) |

---

### RAID

> 여러 개의 하드 디스크가 있을 때 동일한 데이터를 다른 위치에 중복해서 저장하는 방법
데이터를 여러 개의 디스크에 저장하여 입출력 작업이 균형을 이루게 되어 전체적인 성능을 향상
> 
- RAID-5
    - 최소 디스크의 구성이 3개이므로 3개로 구성 시 33.3%, 4개로 구성시 25%, 5개로 구성시 20%가 패리티 공간으로 사용

---

### 시그널

| 번호 | 이름 | 설명 |
| --- | --- | --- |
| 1 | SIGHUP(HUP) | 터미널에서 접속이 끊겼을 때 보내지는 시그널 (데몬 관련 환경 설정 파일을 변경시키고 변화된 내용을 적용하기 위해 재시작할 때 이 시그널 사용) |
| 2 | SIGINT(INT) | 키보드로부터 오는 인터럽트 시그널로 실행을 중지 |
| 3 | SIGQUIT(QUIT) | 키보드로부터 오는 실행 중지 시그널 |
| 9 | SIGKILL(KILL) | 무조건 종료, 프로세스를 강제로 종료 |
| 15 | SIGTERM(TERM) | 가능한 정상 종료시키는 시그널로 kill 명령의 기본 시그널 |
| 18 | SIGCONT(CONT) | STOP 시그널 등에 의해 정지된 프로세스를 다시 실행시킬 때 사용 |
| 19 | SIGSTOP(STOP) | 터미널에서 입력된 정지 시그널 |
| 20 | SIGTSTP(TSTP) | 프로세스를 계속 실행해야 하는 상황에서 해당 프로세스를 일시 중지시키기 위해 사용되는 시그널 |
