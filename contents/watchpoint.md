  # :five: 변수에 watchpoint 걸어보기

- breakpoint와 유사하지만 다음에 해당 **값이 변경**될 때 디버거를 일시 중지

- 하단 variable views에서 관찰하고 싶은 속성 우클릭해서 watch point 걸수 있음
  ![image](https://user-images.githubusercontent.com/20410193/133588634-8546439d-9ac5-407c-90e2-b154d2177363.png)


- breakpoint navigator에서도 watchpoint 확인 가능

  ![image](https://user-images.githubusercontent.com/20410193/133588419-4ea296b1-3870-4cdc-b7d3-e38e0b1ff9bc.png)

- watchpoint 걸리면 아래와 같은 화면이 나타나는데, 좌측의 debug navigator에 뜨는 콜스택 정보를 통해 어떻게 해당 값이 바뀌고 있는건지 파악 가능

  ![image](https://user-images.githubusercontent.com/20410193/133588409-7f1a70a0-87df-4b53-8ba1-d427c11bfd8d.png)



- 워치포인트 사용시 주의할 점은 재빌드시 브포와 달리 유지되지 않는다는 것. 다시 걸어줘야함; 리빌드해도 breakpoint navigator 에는 여전히 워치포인트가 남아있긴 한디 안걸림;; 약간 어이없는 포인트

  **NOTE**: Watchpoints are not saved between executions of your program.
- 그리고 String 같은거에 Watchpoint 걸면  `error: watch size of 16 is not supported` 에러 뱉으면서 워치포인트 설정 불가할때도 있음. 
  mac x86_64 architecture 를 사용하는 경우 1개의 워치포인트가 지원하는 최대 사이즈는 8바이트이기 때문. 그리고 워치 포인트 설정 가능 개수도 최대 4개임 (https://stackoverflow.com/questions/52072176/setting-watchpoints-for-large-data-structures-in-lldb)
  
- 참고로 좌측 stack frame의 backtrace 는 icon 으로 구분되는데, 각각의 의미는 아래와 같음
  ![image](https://user-images.githubusercontent.com/20410193/135053614-ab1613c3-2e24-4094-a0e8-56cd2fc09bfb.png)

### 이전 👈🏻
- [4️⃣ 브레이크 포인트에 action 으로 추가 command 입력해서 디버깅 하기](https://github.com/sujinnaljin/Improving_Productivity/blob/main/contents/breakpointAction.md)
### 다음 👉🏻
- [6️⃣ lldb 오픈소스 Chisel 으로 할 수 있는 것들](https://github.com/sujinnaljin/Improving_Productivity/blob/main/contents/Chisel.md)
