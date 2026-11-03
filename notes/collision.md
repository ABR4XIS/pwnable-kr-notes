# collision
> 아빠가 오늘 MD5 해시 충돌을 알려주셨어요. 나도 그런걸 해보고 싶어요!

# 문제 개요
* pwnable.kr - Toddler's Bottle
* 3 point
* files: `col`, `col.c`, `flag`
# 프로그램 분석 
문자열을 정수 배열로써 읽어 그 값을 모두 더한 값을 해쉬와 비교하여 비밀번호를 검증합니다.
# 설계 및 시나리오
해쉬 함수가 단순하므로 해쉬 충돌을 일으킬 수있습니다.
# 익스플로잇
문제 소스에 주어진 조건을 모두 만족하는 값을 찾기 위해 z3을 이용합니다

```
from z3 import *

s = Solver()
ip = [BitVec(f'ip_{i}', 32) for i in range(5)]
hashcode = 0x21DD09EC

s.add(Sum(ip) == hashcode)

for i in range(5):
    for j in range(4):
        byte = Extract(j * 8 + 7, j * 8, ip[i])
        s.add(Or(
            And(byte >= ord('0'), byte <= ord('9')),
            And(byte >= ord('A'), byte <= ord('Z')),
            And(byte >= ord('a'), byte <= ord('z'))
        ))

if s.check() == sat:
    m = s.model()
    res_bytes = bytearray()
    
    for i in range(5):
        val = m[ip[i]].as_long()
        res_bytes.append((val >> 0) & 0xFF)
        res_bytes.append((val >> 8) & 0xFF)
        res_bytes.append((val >> 16) & 0xFF)
        res_bytes.append((val >> 24) & 0xFF)

    print(password)
else:
    print("why")
```
# 결과

실행 결과와 플래그 문자열은 공개하지 않습니다.
