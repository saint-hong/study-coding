# 인형뽑기
- 인형이 각 열마다 차례대로 쌓여있는 배열에서 하나씩 꺼내어 다른 바구니에 쌓는다.
- 집게발의 이동위치에 따라서 해당하는 열의 숫자를 꺼낸다.
- 집게발의 이동위치에 숫자 0이 있으면 인형이 없다는 의미이고, 다음 위치로 이동한다.
- 뽑은 인형이 새 바구니에 쌓일 때 이전에 쌓은 인형과 중복이면 사라진다.
- 새 바구니에 인형을 담을 때 사라진 인형의 갯수를 반환한다.

```
### board 변수에는 인형이 담긴 바구니의 배열이 2차원 리스트로 주어진다.

board = [[0, 0, 0, 0, 0],
        [0, 0, 1, 0, 3],
        [0, 2, 5, 0, 1],
        [4, 2, 4, 4, 2],
        [3, 5, 1, 3, 1]]

### moves 변수에는 집게발이 움직이는 위치가 순서대로 저장되어 있다. 이 숫자는 인형 뽑기 상자의 인형이 쌓여있는 열의 위치를 의미한다.

moves = [1, 5, 3, 5, 1, 2, 1, 4] 

```

### 코딩 계획
- 집게발의 위치에 해당하는 숫자를 선택한다.
- 이 선택된 인형을 새로운 바구니에 저장한다.
   - 이 새로운 바구니에서 중복된 인형을 어떻게 처리 할 것인가?
   - 중복된 인형을 지울 경우 횟수는 어떻게 셀 것인가?
- 집게발의 위치에 인형이 없으면 다음 위치로 이동한다.
   - 배열의 어떤 위치에 숫자가 있는이 0이 있는지 어떻게 확인할 것인가?
   - 다음 위치로 이동했을 경우에도 숫자가 없으면 그 다음 위치로 이동한다.
- 새로운 위치에서 숫자를 선택하면 이 숫자가 새 바구니의 앞의 숫자와 같은지 확인한다.
   - 앞의 숫자와 같으면 중복이므로 삭제한다. 
   - 삭제할 떄 삭제 횟수를 세어준다.
- 새 바구니에 쌓인 인형들 중에서 중복되어 삭제된 인형의 갯수를 파악하여 결과값을 반환한다.

### 코딩 분석

#### 문제의 예시로 주어진 board와 moves

- board 와 moves 각각 리스트에 숫자가 들어있다.
```python
board = [
    [0, 0, 0, 0, 0],
    [0, 0, 1, 0, 3],
    [0, 2, 5, 0, 1],
    [4, 2, 4, 4, 2],
    [3, 5, 1, 3, 1]
       ]

moves = [1, 5, 3, 5, 1, 2, 1, 4]
```

#### 인덱싱, 선택, 삭제등을 위해 넘파이의 2차원 배열로 변환
```python
array_board = np.array(board)
array_board

>>>

array([[0, 0, 0, 0, 0],
       [0, 0, 1, 0, 3],
       [0, 2, 5, 0, 1],
       [4, 2, 4, 4, 2],
       [3, 5, 1, 3, 1]])
```

#### 집게발의 위치에 해당하는 숫자를 배열에서 선택
- for문을 사용하여 집게발 위치 선택
- 집게발의 위치는 배열의 열의 인덱스이다.
   - 이 위치가 배열의 인덱스와 일치해야하므로 -1을 해준다.
   - now_col 변수에 저장한다.

```python
for i in range(len(moves)) : # 열
    now_col = moves[i] - 1
    print("moves : {}, re_moves : {}".format(moves[i], now_col))

>>>

moves : 1, now_col : 0
moves : 5, now_col : 4
moves : 3, now_col : 2
moves : 5, now_col : 4
moves : 1, now_col : 0
moves : 2, now_col : 1
moves : 1, now_col : 0
moves : 4, now_col : 3
```

#### 집게발의 위치의 인형이 0인지 아닌지 확인
- 이중 반복문 사용하여 인형의 위치로 이동
   - 첫번쨰 반복문은 moves 값, 즉 배열의 열의 위치에 해당한다.
   - 두번빼 반복문은 배열의 행의 위치에 해당한다.
- 조건문을 사용하여 현재 위치의 인형이 0이면 첫번쨰 반복문을 다시 진행한다.
   - continue 함수 사용
- 현재 위치의 인형이 0이 아닌 숫자이면 doll 변수에 저장한다.
   - break 를 사용하여 내부 반복문에서 빠져나와 다음 moves로 이동한다.

```python
for i in range(len(moves)) : # 열
    now_col = moves[i] - 1
    for j in range(5) : 
        if array_bord[:, now_col][j] == 0 :
            continue
        else :
            doll = array_bord[:, now_col][j]
            print("현재 moves : {}, 인형 번호 : {}".format(now_col, doll))
            break

>>>

현재 moves : 0, 인형 번호 : 4
현재 moves : 4, 인형 번호 : 3
현재 moves : 2, 인형 번호 : 1
현재 moves : 4, 인형 번호 : 3
현재 moves : 0, 인형 번호 : 4
현재 moves : 1, 인형 번호 : 2
현재 moves : 0, 인형 번호 : 4
현재 moves : 3, 인형 번호 : 4
```

#### 숫자 인형을 선택한 후 현재 위치의 배열을 0으로 바꿔준다.
- 현재 위치의 인형이 0이 아니라 숫자이면 새로운 바구니에 담는다.
- 또한 새로운 바구니로 인형을 옮겼기 때문에 인형뽑기 배열에서는 0으로 바꿔줘야한다.
   - 다음 moves가 같은 위치에 왔을 때 중복으로 선택하지 않기 위해서

```python
array_bord[:, now_col][j] = 0
```

```python
for i in range(len(moves)) : # 열
    now_col = moves[i] - 1
    for j in range(5) :
        if array_bord[:, now_col][j] == 0 :
            continue
        else :
            doll = array_bord[:, now_col][j]
	    array_bord[:, now_col][j] = 0   # 현재 위치의 값을 0으로 바꿔준다.
```


#### 현재 위치에서 선택한 인형을 새로운 바구니에 담는다.
- 숫자 인형을 선택한 후 새로운 바구니에 쌓는다.
   - append 명령어를 사용하여 리스트에 새로운 인형을 담는다.
   - 리스트의 앞부분부터 저장된다. 
- 새로운 바구니에 인형을 저장할 때 현재 새바구니에 들어있는 마지막 인형의 번호와 비교한다.
   - 비교하여 같은 경우 새바구니에서 중복 인형을 삭제한다.
   - pop 명령어 사용
   - counting_pop 변수를 만들고 이 작업을 할 때마다 1씩 더해준다.
- 새바구니에 중복된 인형이 없을 경우 그대로 저장한다.
   - 새바구니에 인형을 저장했으면 break 명령어를 통해 첫번쨰 반복문을 다시 실행한다.
   - 즉 moves 의 위치를 이동한다.

```python
for i in range(len(moves)) : # 열
        now_col = moves[i] - 1
        for j in range(5) :
            if array_bord[:, now_col][j] == 0 :
                continue
            else :
                doll = array_bord[:, now_col][j]
                array_bord[:, now_col][j] = 0
                if doll == new_list[-1] :   # 인형의 번호가 새바구니의 마지막 번호와 같은지 확인
                    counting_pop += 1       # 인형의 번호가 같으면 중복이므로 counting_pop에 1을 더해준다.
                    new_list.pop()          # 중복인 인형의 번호를 새바구니에서 삭제한다.
                else :
                    new_list.append(doll)
                break

```

#### 새바구니에는 0을 최초로 저장한다.
- 현재 선택된 인형을 새바구니에 담기전에 새바구니의 마지막 인형과 비교를 해야한다.
- 이때 첫 인형의 경우 새바구니가 비어 있으면 비교를 할 수 없으므로 0을 저장한다.
```python
mew_list = [0]
```

#### counting_pop의 2배는 삭제된 인형의 갯수와 같다.
- 새바구니에 인형을 담을 떄 앞에 담긴 인형과 숫자가 같으면 countin_pop에 숫자를 누적했다.
- 이 숫자는 중복된 인형인 경우의 숫자이므로 삭제된 인형은 앞의 인형과 현재 인형 2개이다.
- 따라서 counting_pop의 2배를 해주면 중복되어 삭제된 인형의 갯수와 같다.

```python
result = counting_pop * 2
```

### 코드 실행
```python
bord = [
    [0, 0, 0, 0, 0],
    [0, 0, 1, 0, 3],
    [0, 2, 5, 0, 1],
    [4, 2, 4, 4, 2],
    [3, 5, 1, 3, 1]
       ]
array_bord = np.array(bord)
moves = [1, 5, 3, 5, 1, 2, 1, 4]

new_list = [0]
counting_pop = 0

for i in range(len(moves)) : # 열
    now_col = moves[i] - 1
    print("== 현재 열의 위치 : {} ===".format(now_col))
    print("새바구니 : {}".format(new_list))
    print()
    for j in range(5) : 
        if array_bord[:, now_col][j] == 0 :
            continue
        else :
            doll = array_bord[:, now_col][j]
            print(">> 현재 인형 : {}".format(doll))
            array_bord[:, now_col][j] = 0
            if doll == new_list[-1] :
                counting_pop += 1
                new_list.pop()
                print(">> 중복 삭제된 인형 : {}, 삭제횟수 : {}".format(doll, counting_pop))
                print()
            else :
                new_list.append(doll)
                print(">>중복아닌 인형 : {}".format(doll))
                print()
            break

print("새바구니 최종 : {}, 삭제횟수 : {}, 삭제된 인형의 갯수 : {}".format(new_list, counting_pop, counting_pop*2))
```

- 각 단계별 코드 결과 확인
```python
>>>

== 현재 열의 위치 : 0 ===
새바구니 : [0]

>> 현재 인형 : 4
>>중복아닌 인형 : 4

== 현재 열의 위치 : 4 ===
새바구니 : [0, 4]

>> 현재 인형 : 3
>>중복아닌 인형 : 3

== 현재 열의 위치 : 2 ===
새바구니 : [0, 4, 3]

>> 현재 인형 : 1
>>중복아닌 인형 : 1

== 현재 열의 위치 : 4 ===
새바구니 : [0, 4, 3, 1]

>> 현재 인형 : 1
>> 중복 삭제된 인형 : 1, 삭제횟수 : 1

== 현재 열의 위치 : 0 ===
새바구니 : [0, 4, 3]

>> 현재 인형 : 3
>> 중복 삭제된 인형 : 3, 삭제횟수 : 2

== 현재 열의 위치 : 1 ===
새바구니 : [0, 4]

>> 현재 인형 : 2
>>중복아닌 인형 : 2

== 현재 열의 위치 : 0 ===
새바구니 : [0, 4, 2]

== 현재 열의 위치 : 3 ===
새바구니 : [0, 4, 2]

>> 현재 인형 : 4
>>중복아닌 인형 : 4

새바구니 최종 : [0, 4, 2, 4], 삭제횟수 : 2, 삭제된 인형의 갯수 : 4
```

# 최종코드

```python
def solution(board, moves) :
    array_bord = np.array(board)
    new_list = [0]
    counting_pop = 0

    for i in range(len(moves)) : # 열
        now_col = moves[i] - 1
        for j in range(len(board) :
            if array_bord[:, now_col][j] == 0 :
                continue
            else :
                doll = array_bord[:, now_col][j]
                array_bord[:, now_col][j] = 0
                if doll == new_list[-1] :
                    counting_pop += 1
                    new_list.pop()
                else :
                    new_list.append(doll)
                break
		
    result = counting_pop * 2
    
    return result
```

- 함수 호출

```python
board = [[0,0,0,0,0],[0,0,1,0,3],[0,2,5,0,1],[4,2,4,4,2],[3,5,1,3,1]]
moves = [1,5,3,5,1,2,1,4]
solution(board, moves)

>>>

4
```
