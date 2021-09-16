# :four: 브레이크 포인트에 action 으로 추가 command 입력해서 디버깅 하기

- 브레이크 포인트 더블 클릭하면 수정할 수 있는 창이 뜸

  ![image](https://user-images.githubusercontent.com/20410193/133587846-24ae5063-070f-45f5-bb7f-7ebba8ba7f19.png)

- Condition - bool 값 리턴하는 조건 입력해서 true 일때만 breakpoint에서 일시 중지

  ![Xcode showing an example of a breakpoint with a condition set to “indexPath.row > 8”.](https://docs-assets.developer.apple.com/published/f18819b4ae5ea07a1818a26608630723/10900/setting-breakpoints-to-pause-your-running-app-4@2x.png)

- Ignore -  일부 횟수에 대해 breakpoint를 무시하도록 디버거 설정. (근데 condition이랑 중복 안되는듯. ex. A condition 에서 5번 무시.. 같은?)

  ![Xcode showing an example of a breakpoint with a setting of “Ignore 8 times before stopping”.](https://docs-assets.developer.apple.com/published/dad9eae6778dcfc42e7429814d48d7a8/10900/setting-breakpoints-to-pause-your-running-app-3@2x.png)

- Action 버튼을 눌러서 해당 breakpoint가 트리거 될 때 이어서 수행될 작업을 지정할 수 있음.

  기본적으로 Debugger Command 타입으로 lldb 명령어를 입력할 수 있음. (AppleScript 또는 셸 스크립트를 실행하여 스크린샷을 찍거나 분석을 위해 일부 앱 데이터를 저장하는 것과 같은 유용한 디버깅 작업을 수행할 수도 있다고 하는디,, 그건 각자 알아보기로!)

  ![디버거 명령 작업으로 중단점을 표시하는 Xcode.  디버거 명령은 "po indexPath"입니다.](https://docs-assets.developer.apple.com/published/f6ac0431871e13b8fe9ede7c698b989b/10900/setting-breakpoints-to-pause-your-running-app-12@2x.png)

- 맨 마지막에 체크된 auto continue 옵션은 체크시 command 실행 후 break에 걸린채로 있지 않고 프로그램을 자동 진행하게 해줌

- Action 응용 예시
  - 만약 내가 `tableView.delegate = self` 코드를 추가 안해서 에러난거 같다? 
  - 원래 코드 추가한 후에 다시 확인을 위해 run 해야 했음
  - 하지만 이제 추가해야하는 부분에 브포 걸고 Action 으로 추가 -> 해당 브포 trigger 해서 확인
  - 만약 정상 작동 된다면 굳이 재빌드 하지 않고서 빠르게 디버깅 가능 (언젠가 코드 추가는 해야겠지만!)

  [![image](https://user-images.githubusercontent.com/20410193/132126800-61acef63-309f-4060-82df-36e1cd27cfc9.png)](https://user-images.githubusercontent.com/20410193/132126800-61acef63-309f-4060-82df-36e1cd27cfc9.png)

- breakpoint나 다음 섹션에 나올 watchpoint를 lldb 명령어로 거는 방법은 가장 하단에 추가 해놓음. (인터페이스로 조작하는 상황이 더 많을 것 같아서 인터페이스 위주로 우선 소개)

## 💡 참고 - Xcode 13의 Column Breakpoints

- line 단위로 breakpoint를 걸기 때문에 아래 코드에서 두번째 함수(`oddNumber()`)만 step into 같은걸로 디버깅해서보고 싶어도 첫번째 함수(`evenNumber()`) 를 거쳐야함

  ![image](https://user-images.githubusercontent.com/20410193/133587930-38610581-0344-43d7-b5e0-8e91a5ffee55.png)


- 하지만 엑코 13 부터 column breakpoint 등장 (원하는 곳에서 cmd + click 하면 나옴)

  ![image](https://user-images.githubusercontent.com/20410193/133587954-859e5960-b485-42dc-9f85-194a89809f8f.png)


- 그러면 oddNumber 쪽에 바로 브포 걸린다

  ![image](https://user-images.githubusercontent.com/20410193/133587967-d8a520ce-b4ba-4631-88d4-4f4d979d31a9.png)

- 브포 수정하는것도 일반 line 브포와 동일하게 더블클릭해서 조작 가능

  ![image](https://user-images.githubusercontent.com/20410193/133587990-97460873-20f6-4eac-aa2c-ab94e2e8b65e.png)
  
### 이전 👈🏻
- [3️⃣ expression 명령어와 unsafeBitCast를 이용해 시뮬레이터에서 속성 바뀌는것 바로 파악하기](https://github.com/sujinnaljin/Improving_Productivity/blob/main/contents/castUsingExpression.md)
### 다음 👉🏻
- [5️⃣ 변수에 watchpoint 걸어보기](https://github.com/sujinnaljin/Improving_Productivity/blob/main/contents/watchpoint.md)
