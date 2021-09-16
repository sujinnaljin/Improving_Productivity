# :three:  expression 명령어와 unsafeBitCast를 이용해 시뮬레이터에서 속성 바뀌는것 바로 파악하기

- 객체의 주소값과 Type을 알고있는 경우 Swift에서는 `unsafeBitCast(to:)` 함수를 이용해 캐스팅을 할 수 있음

```sh
(lldb) e let $myObject = unsafeBitCast(0xabcdef, to: UILabel.self) # 0xabcdef에 할당된 UILabel 에 접근해 변수로 선언
(lldb) e $myObject # 변수를 이용해 해당 object의 정보 알 수 있음
(lldb) e $myObject.text = "hello world" # 변경도 가능

## 예시
(lldb) po self.view # po 를 통해 사용하고 싶은 객체의 주소값 알 수 있음
▿ Optional<UIView>
  - some : <UIView: 0x7f9cc8e05d70; frame = (0 0; 390 844); autoresize = W+H; layer = <CALayer: 0x600003c01860>>
  
(lldb) e let $myView = unsafeBitCast(0x7f9cc8e05d70, to: UIView.self) # UIView 로 캐스팅
(lldb) e $myView.backgroundColor = .red # 속성 변경
```

- unsafeBitCast 이용하는건 특히 debug View Hierarchy 에서 유용할 듯
- debug View Hierarchy 클릭해서 lldb 로 들어왔을 때 유의할 점은, 기본적으로 objc context 인듯. 따라서 Swift 표현을 사용하면 오류 발생
- expression 의  `-l` (`-language`) 옵션을 사용해서 Objective-C Context에서도 Swift Expression을 사용할 수 있음 (그 반대도 마찬가지)

```sh
# UIkit을 import하여 LLDB Console에서 UIKit 의 type 사용할 수 있게 함 (view hierarchy 에서 lldb 로 들어왔으니까)
(lldb) e -l swift -- import UIKit
# 0xabcdef에 할당된 UIView로 캐스팅 해서 변수로 선언
(lldb) e -l swift -- let $myObject = unsafeBitCast(0xabcdef, to: UIView.self)
# 변수에 할당 된 값 변경
(lldb) e -l swift -- $myObject.backgroundColor = .blue
# 디버거에서 일시 중지되어 있기 때문에 core animation에서 현재 화면의 프레임 버퍼에 뷰 모듈 변경 사항을 적용하고 있지 않음. 따라서 CATransaction 의 flush 함수를 통해 core animation이 screen의 frame buffer를 업데이트하도록 함
(lldb) e -l swift -- CATransaction.flush()
```

- 매 명령 앞에 e -l swift -- 붙여야하는게 귀찮으니까 settings 명령어를 통해 하위 context를 모두 swift 로 변경해줄 수 있음

```sh
(lldb) settings set target.language swift 
(lldb) e
Enter expressions, then terminate with an empty line to evaluate:
1 import UIKit
2 unsafeBitCast(0x7fa0f4406d10, to: UIView.self).backgroundColor = .red # 변수로 할당 안하고 바로 접근도 가능
3 CATransaction.flush()
4 
```

- 오토레이아웃도 컨스트레인트 잡고 비슷하게 변경 가능

```
(lldb) e -l swift -- unsafeBitCast(0x600003926710, NSLayoutConstraint.self).constant = 300
(lldb) e -l swift -- CATransaction.flush()
```

- 사실 viewHierarchy 에서 객체 잡고 cmd + c 하면 `((NSLayoutConstraint *)0x600003926710)` 이런 식으로 선택된 object 의 casted pointer 을 줌

```
(lldb) e [((NSLayoutConstraint *)0x600003926710) setConstant: 130]
(lldb) e -l swift -- CATransaction.flush()
```

## Alias

- `settings set target.language swift` 나 `expr -l swift --` , `e CATransaction.flush()` 같은거 매번 입력하기 귀찮다! -> alias 설정가능

```sh
(lldb) command alias 별명 "줄이고 싶은 Command"
```

- 예시

  ```sh
  (lldb) command alias els expression -l swift --  # expression language swift의 약자로 els 라고 해둠
  (lldb) command alias setSwift settings set target.language swift
  (lldb) command alias setObjc settings set target.language objc
  (lldb) command alias flush e CATransaction.flush()
  
  # 사용
  (lldb) setObjc
  (lldb) els unsafeBitCast(0x600002ab8640, NSLayoutConstraint.self).constant = 150
  (lldb) setSwift
  (lldb) flush
  ```

- 하지만 Alias로 등록해두어도 별칭은 **해당 빌드 시점이 지나면, LLDB Session이 끝남과 동시에 사라지**기 때문에 Xcode를 실행시 LLDB 초기화를 위해 사용되는 **~/.lldbinit** 파일에 원하는 Command Alias를 추가해두면, 별칭을 매번 정해주지 않고 계속 사용 가능
