# :seven: Execution Command (continue, step over, step in, step out)

- Execution 명령들은 LLDB Console 상단에 위치한 네개 버튼과 같은 역할. 차례대로 Continue, Step-Over, Step-In, Step-Out 을 의미

[![image](https://user-images.githubusercontent.com/20410193/132126765-a268b144-8ab4-4540-9780-0fe43b7c618b.png)](https://user-images.githubusercontent.com/20410193/132126765-a268b144-8ab4-4540-9780-0fe43b7c618b.png)

## Continue

### `continue` (`c`) 명령어

- `process continue` 의 약어
- 정지된 프로그램 실행을 재개. 다음 Breakpoint가 나타날때까지 프로그램을 진행

```
  (lldb) process continue
  (lldb) continue
  (lldb) c
```

- process 명령어는 현재 플랫폼의 프로세스와 상호작용 할 수 있게 도와주는 명령어로 `process status` 같은 경우는 현재 디버거가 어디 있는지 찾을 수 있게 해줌

```
(lldb) process status
```

## Stepping Over

### `next` (`n`) 명령어

- `thread step-over` 의 약어
- 현재 Break 걸려있는 지점에서 바로 **다음 Statement**로 이동. (현재 선택된 Frame에서 소스 수준의 한 단계를 진행)

```
  (lldb) thread step-over
  (lldb) next
  (lldb) n
```

## Stepping In

### `step` (`s`) 명령어

- `thread step-in` 의 약어
- 현재 선택된 Frame에서 소스 수준의 한 단계 안으로 들어감
- 다음 Statement가 Function Call 인경우 Debugger를 해당 **함수 내부에 위치한 시작 지점**으로 이동

```
  (lldb) thread step-in
  (lldb) step
  (lldb) s
```

## Stepping Out

### `finish` 명령어

- `thread step-out` 의 약어
- 현재 진행중인 function이 return 될때까지 프로그램을 진행한 후 프로그램 Break 걸어주는 Stepping Action (현재 선택된 Frame에서 벗어남)
- Stack Memory 관점에서 Stepping Out은 Stack Frame을 Pop하는 것과 동일 (왼쪽 Debug Navigator를 잘 살펴보면 Stepping-In과 Stepping-Out 시 StackFrame이 어떻게 달라지는 지 확인 가능)

```
  (lldb) thread step-out
  (lldb) finish
```

- 단축어로 `f` 로 되지 않을까 싶었는데 그건 `frame` 명령어의 약자였다
