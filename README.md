# pwnable.kr — 13문제 풀이 기록

2026년 2월 9일부터 24일까지 작성한 pwnable.kr 풀이를 공개용으로 다시 정리한 인덱스입니다. 원본 13개 글은 로컬에 백업했으며, 플래그 문자열은 공개하지 않습니다.

| 날짜 | 문제 | 분석한 핵심 | 해결 방법 |
|---|---|---|---|
| 02.09 | [fd](notes/fd.md) | 인자로 파일 디스크립터 선택 | 표준 입력이 선택되도록 fd를 계산하고 데이터 전달 |
| 02.10 | [collision](notes/collision.md) | 5개 정수의 합으로 만든 단순 해시 | 조건을 Z3로 구성해 동일한 합을 갖는 입력 생성 |
| 02.11 | [bof](notes/bof.md) | `gets` 기반 스택 버퍼 오버플로우 | 버퍼와 key 사이 오프셋을 계산해 key 덮어쓰기 |
| 02.11 | [passcode](notes/passcode.md) | 잘못된 `scanf` 인자와 스택 재사용 | passcode1을 쓰기 주소로 만들고 `fflush` GOT overwrite |
| 02.11 | [random](notes/random.md) | `srand`가 호출되지 않은 libc PRNG | 고정된 첫 `rand()` 값과 XOR 가역성 이용 |
| 02.12 | [input2](notes/input2.md) | argv·fd·환경변수·파일·소켓 검사 | Python으로 다섯 입력 경로를 단계별 구성 |
| 02.12 | [leg](notes/leg.md) | ARM/Thumb 모드의 PC·LR 동작 | key1–3의 반환값을 어셈블리에서 추적 |
| 02.17 | [mistake](notes/mistake.md) | `<`와 `=`의 연산자 우선순위 | fd가 stdin이 되는 흐름을 이용해 XOR 입력 구성 |
| 02.19 | [cmd1](notes/cmd1.md) | PATH 초기화와 문자열 필터 | 절대 경로와 와일드카드로 명령 구성 |
| 02.19 | [coin1](notes/coin1.md) | 제한 횟수 안에서 위조 동전 탐색 | 이진 탐색을 pwntools로 자동화 |
| 02.19 | [horcruxes](notes/horcruxes.md) | `gets` 오버플로우와 7개 함수 | A–G 호출 ROP 후 signed 32비트 합계 전달 |
| 02.19 | [lotto](notes/lotto.md) | 중복을 고려하지 않은 비교 로직 | 같은 바이트를 반복해 일치 횟수 조건 공략 |
| 02.24 | [cmd2](notes/cmd2.md) | 환경 초기화와 `/`·PATH·flag 필터 | `printf` 8진수 치환과 심볼릭 링크로 경로 구성 |

## 대표적으로 설명할 수 있는 사례

- **passcode:** 104바이트가 필요한 초기 접근이 100바이트 제한에 막혀, 한 변수만 제어하는 GOT overwrite 방식으로 바꿨습니다.
- **input2:** 프로세스 인자, fd 0·2, 환경변수, 파일, TCP 소켓을 한 실행 안에서 모두 제어했습니다.
- **horcruxes:** 일곱 함수를 순서대로 호출하는 ROP 체인을 만들고 Python 합계를 C의 signed 32비트 값으로 변환했습니다.
- **leg:** ARM/Thumb 모드에서 PC가 가리키는 위치와 LR의 반환 주소를 구분해 세 함수의 결과를 계산했습니다.

더 자세한 설명은 [포트폴리오의 13문제 기록](https://abr4xis.github.io/cases/input-rop/)에 정리했습니다.
