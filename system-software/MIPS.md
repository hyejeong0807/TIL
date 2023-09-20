###  MIPS Instruction Set Architecture

컴퓨터의 종류마다 기계어의 집합이 다르다. 

MIPS Register & Memory model

32개의 레지스터 (정수 연산을 위한) CPU에 피연산자로 올 수 있는 레지스터가 총 32개라는 의미임. 32개의 레지스터 중 연산의 인풋과 아웃풋으로 저장할 레지스터가 몇 번 레지스터인지에 대한 구분이 필요하다. 32개의 레지스터를 구분하기 위해서는 몇 비트가 필요한지용? 5비트가 필요하다 2^5 = 32

MIPS에서 메모리 모델은? 총 접근 가능한 메모리의 크기가 2^32 bytes이다. 메모리는 바이트 단위로 주소가 매겨진다. 메모리의 구분을 위해 32 bits를 지원한다. word로 표현하면? 4 바이트가 1 워드이므로 2^30 words로 표현할 수 있다. 

메모리는 바이트 단위로 2^32 바이트로 이뤄져 있다 (32 비트가 필요). 레지스터는 번호가 매겨져 레지스터 각각은 4 바이트로 이뤄진다. 

PC (Program Counter) 레지스터는 cpu가 바로 다음에 수행할 기계어의 메모리 위치를 나타낸다. cpu는 매 클락마다 기계어를 읽어서 해석하여 연산을 수행한다. 하나의 기계어를 끝내고 나면? 메모리 몇 번지의 기계어를 읽어올지를 의미한다. 4 바이트를 의미한다. (32비트, 메모리의 위치) 

메모리를 표현하기 위해서는 비트가 더 필요하다. 기계어가 더 길어지는가? MIPS에서는 모든 기계어의 길이가 동일하다. 기계어의 길이는 4바이트로 구성된다. 

LO, HI 곱셈 나눗셈을 저장하기 위한 레지스터

$0 ~ $31까지의 레지스터는 자유롭게 사용되는 것이 아니라. 어느정도 기능이 미리 정해져 있는 부분이 있다. 

#### MIPS Instructions의 종류

1. 산술/논리 연산
 - 덧셈, 뺄셈, AND, OR ... 연산을 의미한다. 

2. 데이터 트랜스퍼 (Load/Store)
 - load? 메모리에 있는 데이터를 레지스터로 불러오는 것 반대로 cpu에서 연산된 결과를 메모리에 저장하는 것을 store라고 한다. 
 - MIPS instruction에서는 이 부분을 제외하고 메모리에 접근하지 않는다. 
 - 명령 중 메모리에 접근하면 느려진다. 100배 정도 느리기 때문에 빠르고 느린 기계어를 뒤섞지 않고 구분한다. 
 - 1번 레지스터에 있는 데이터와 메모리에 있는 데이터를 더해서 저장해라 이런 명령이 없다. 연산을 두개로 쪼개어서 사용한다. 메모리에 있는 데이터를 레지스터에 로드하고 레지스터만 접근하는 산술 연산을 수행하도록 하는 것이다. 

3. 조건 분기 명령
 - 프로그램 카운터를 바꾸어 주는 명령어 
 - MIPS에서는 다음 명령이 끝나면 바로 다음 메모리의 명령을 수행한다. 
 - 프로그래밍이 실행되는 로직이 순차적으로 동작되도록 하지 않는다. (function 호출 ...)

4. 비조건 점프 명령
 - 프로그램 카운터를 바꾸어 주는 명령어 

#### MIPS Instruction Format

32 bits로 구성되어 있다. 

메모리 위치를 지정하기 위해서 32 비트가 필요하지만 mips에서는 32 비트의 포맷을 지원한다. 어떻게 표현? 직접 메모리 비트를 나타내기 위해 26비트를 사용한다. 메모리 주소를 직접 적지 않고 레지스터에 넣어서 그 메모리 주소를 담고 있는 레지스터 사용하여 접근한다. 하지만, 레지스터에 접근하고 메모리에 접근하기 때문에 더 느려짐. 

6비트는 연산의 종류, 총 64개인데 부족하기 때문에 피연산자를 레지스터로 사용 시 남는 비트를 활용하여 추가적인 연산을 표현한다. 

ISA에서는 피 연산자로 레지스터나 메모리를 올 수 있다고 했지만 MIPS에서는 외에서는 immediate가 올 수 있다. 레지스터나 메모리 같은 경우는 실제 값이 피 연산자에 들어 있는게 아닌 위치 정보가 있는 것. immediate는 기계어안에 연산하려는 숫자가 들어 있다. 피 연산자 중 immediate, 레지스터, 메모리에 접근 하는 것 중 어디가 제일 빠를까? 제일 느린 건 메모리 접근 레지스터 접근은 이 보다 빠르다. immediate는 읽어올 필요가 없어 가장 빠르다. 

곱셈인 경우 32bit * 32bit 를 곱해버리면 64bit까지 나올 수 있다. 그래서 인풋/아웃풋 레지스터 2가지가 필요하다. 

정수 나눗셈의 경우 결과가 몫과 나머지로 정의된다. 

#### 산술/논리 연산

C언어와 같은 고급 언어는 0이 아닌 숫자를 모두 참으로 간주한다. 

|Instruction|Example|Meaning|Comments|
|-----------|-------|-------|--------| 
|and|and $s1, $s2, $s3|$s1 = $s2 & $s3|Three reg. operands; logical AND|
|or|or $s1, $s2, $s3| $s1 = $s2 \| $s3 |Three reg.operands; logical OR|

레지스터의 비트 수는 32비트를 사용할 수 있고 immediate는 16비트를 사용할 수 있다. 이 두 가지를 연산하려고 하지만 두 가지의 비트 사이즈가 다를 때 어떻게 연산을 수행하는가? 

이때는 작은 비트 사이즈의 비트를 확장 시켜서 연산을 수행한다. 

음수가 들어있을 때는? 2의 보수 형태로 사용하게 되는데 맨 상단의 sign 비트가 0이면 양수 1이면 음수를 나타낸다. 16비트도 마찬가지로 그렇게 나타냄. 

#### 메모리 접근 연산

|Instruction|Example|Meaning|Comments|
|-----------|-------|-------|--------| 
| load word | lw $s1, 100($s2) | $s1=Memory($s2 + 100) | word from memory to register |
| store word | sw $s1, 100($s2) | Memory[$s2 + 100]=$s1 |  |

load half word : 2bytes를 저장하는 연산도 있음. 

#### MIPS Addressing

- 모든 메모리 접근은 load/store를 통해서만 가능

- 메모리로부터 halfword 또는 byte 단위 load 시

  - signed operation인 경우 sign-extended
  - unsigned operation인 경우 zero-extended

- Alignment restriction (경계를 지켜준다)
  
  - word address는 4의 배수여야 함.

    - 만약 4의 배수의 메모리 주소가 연결되어 있을 때 4의 배수가 아닌 지점을 읽고 싶을 때는? halfword로 두 번 읽던지 등등 하나의 명령으로 처리할 수 없다. 

  - halfword addree는 2의 배수여야 함.

#### 분기/점프 명령

분기는 어떤 조건을 만족할 때 Program Counter를 변경

점프는 무조건적으로 수행할 위치를 변경

|Instruction|Example|Meaning|Comments|
|---------|---------|-------|--------| 
| branch on equal | beq $s1, $s2, 25 | if($s1==$s2)go to PC+4+100 | Equal test; PC-relative branch |
| branch on not equal | bne $s1, $s2, 25 | if($s1!=$s2)go to PC+4+100 | Not equal test; |
| set on less than | slt $s1, $s2, $s3 | if($s2<$s3) $s1=1; else $s1=0 | Compare less than; two's complement |
| set less than imediate | slti $s1, $s2, 100 | if($s2<100) $s1=1; else $s1=0 | Compare<constant; two's complement |

25는 16bit immediate이다. 
25번째 떨어진 위치로 branch하라는 말은 기계어 하나가 4바이트 이므로 100번지 떨어졌다고 할 수 있다. 정보를 최대한 아끼기 위해 4바이트를 곱하지 않는다고... 어차피 기계어는 4바이트이므로 굳이 곱하지 않아도 상관없음. 

만약 브랜치 조건과 같지 않다면? PC에서 4바이트만 이동하여 다음 명령을 수행한다. 

set? A가 B보다 작으면 특정 레지스터를 참으로 설정하고 그렇지 않으면 거짓으로 설정해서 그것에 따라 분기를 수행하도록 하는 명령어. 

|Instruction|Example|Meaning|Comments|
|---------|---------|-------|--------| 
| jump | j 2500 | go to 10000 | Jump to target address |
| jump register | jr $ra | go to $ra | For switch, procedure return |

jump register : 점프할 메모리를 레지스터에 저장하고 사용하는 방식. 레지스터를 읽고 메모리 위치로 가야 하기 때문에 시간이 소요됨. 

#### C언어와 MIPS 어셈블리언어

C언어 프로그래밍을하면 컴파일러가 컴파일을 하고 타겟 명령 아키텍처에 맞는 기계어로 변환한다. 

#### Case 1
```
f = (g + h) - (i + j);

f is mapped to s0.
g is mapped to s1.
h is mapped to s2.
i is mapped to s3.
j is mapped to s4.
```

```
add $t0, $s1, $s2
add $t1, $s3, $s4
sub $s0, $t0, $t1
```
#### Case 2
```
g = h + A[i];

g is mapped to s1.
h is mapped to s2.
s3 contains the base address of array A[].
i is mapped to s4. -> i+i = 2i
```
배열의 시작 위치를 레지스터에 올린 상황 

```
add $t1, $s4, $s4
add $t1, $t1, $t1
add $t1, $t1, $s3
lw $t0, 0($t1)
add $s1, $s2, $t0
```
정수 배열의 사이즈는 4바이트이므로 곱셈으로 s4를 다음 명령으로 이동하면 되지만, $t1 = $s4 + $s4를 통해 4i를 계산하여 레지스터를 아꼈다. 그 다음으로 계산된 4i와 배열의 시작 위치를 더하여 실제 배열의 위치로 이동하였다. 

다음으로 t0에 읽은 t1를 로드한다. 마지막으로 `h`(`s2`)와 `A[i]`(`t0`)을 더하여 `g`(`$s1`)를 계산한다. 

#### Case 3
```
if (i==j)
    f - g + h;
else
    f = g - h;

f is mapped to s0.
g is mapped to s1.
h is mapped to s2.
i is mapped to s3.
j is mapped to s4.
```
```
      bne $s3, $s4, Else
      add $s0, $s1, $s2
      j Exit
Else: sub $s0, $s1, $s2
Exit:
```
#### Case 4
```
while (save[i] == k)
    i = i + j;
i is mapped to s3.
j is mapped to s4.
k is mapped to s5.
s6 contains the base address of array save[].
```
```
Loop: add $t1, $s3, $s3
      add $t1, $t1, $t1
      add $t1, $t1, $s6
      lw  $t0, 0($t1)
      bne $t0, $s5, Exit
      add $s3, $s3, $s4
      j Loop
Exit:
```

lw는 t1의 메모리 위치에 있는 데이터를 t0라는 레지스터로 읽어옴을 의미한다. 