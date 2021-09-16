# 0️⃣ 들어가기 전에

## lldb?

  - Low Level Debugger
  - LLVM 프론트엔드에 대응하는 디버거

  ![image](https://user-images.githubusercontent.com/20410193/133586643-5b73bfef-3acb-4ac0-9795-02fdb8829fad.png)

- lldb의 명령어를 이용해 다양한 동작을 할 수 있음

  - `breakpoint` , `thread` , `process` , `expression` , `frame` , `image` 등 다양한 명령어 존재 (알고 있는 명령어가 많을 수록 사용할 수 있는 기능이 많아지겠죠?)

  - 더 많은 명령어를 보고 싶다면 `(lldb) help` 쳐보기. 해당 문법으로 사용가능한 Subcommand, Option 리스트나 사용법을 보여줌

    ```sh
    # LLDB에서 제공하는 Command가 궁금하다면,
    (lldb) help
    
    # 특정 Command의 Subcommand나, Option이 궁금하다면,
    (lldb) help breakpoint
    (lldb) help breakpoint set
    ```

    ![image](https://user-images.githubusercontent.com/20410193/133586695-ce1b4431-1fab-41b7-b0be-6fe25638d5fa.png)


### 다음 👉🏻
- [1️⃣ expression 명령어를 이용해 값 출력하기 ( feat. p, po, v 명령어)](https://github.com/sujinnaljin/Improving_Productivity/blob/main/contents/printUsingExpression.md)
