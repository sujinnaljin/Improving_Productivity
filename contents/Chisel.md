# :six: lldb 오픈소스 Chisel 으로 할 수 있는 것들

- `Chisel` 은 iOS 앱 환경에서 디버깅 시 다양하고 유용한 LLDB 명령어들을 지원하기 위한 오픈소스 라이브러리
- 해당 라이브러리를 import 하여 이곳에 정의되어 있는 많은 기능들을 사용할 수 있음

## Chisel 설치

1. 홈 브류 버전 업데이트

```
brew update
```

2. 홈브류를 통한 chisel 설치

```
brew install chisel
```

3. ~/ 경로에 .lldbinit 파일 생성

```
touch ~/.lldbinit
```

4. .lldbinit 파일 오픈

````
open ~/.lldbinit 
````

5. .lldbinit 파일에 chisel 내용 추가

```
command script import /usr/local/opt/chisel/libexec/fbchisellldb.py
```

## Chisel 사용

- 엑코 breakpoint 트리거 시킨 후 `(lldb) command source ~/.lldbinit` 수행하면 chisel의 커맨드를 쓸 수 있게 됨

### Print Commands

**pvc** - rootViewController로부터 시작하고 UIWindow에 표시하는 모든 UIViewController를 출력하는 명령어

```
(lldb) pvc
```

**pclass** - 해당 인스턴스의 상속 계층 구조를 보여주는 명령어

```sh
(lldb) pclass 0x7fd915f0a070 # pclass 명령어는 swift 를 지원하지 않기 때문에 pclass self 와 같은 명령어 실행하면 에러 
```

**presponder** - 특정 reponder 을 시작으로하는 responder chain을 보여주는 명령어

```
(lldb) presponder self.view
```

**pcells** - 현재 화면에 나타난 최상위의 UITableView에 visible cell을 출력하는 명령어 (collectionView는 없는듯 ㅠ)

```
(lldb) pcells
```

**paltrace** - SubView들의 Hierarchy를 출력하는 명령어, 기본은 Key Window로부터 시작

```
(lldb) paltrace 
(lldb) paltrace 0x7fada085d600
```

### Find Commands

**fvc** - 정규식을 사용하여 특정 UIViewController 클래스를 찾아 출력하는 명령어 

```
(lldb) fvc BaseViewController
```

**fv** - 현재 표시되는 화면에서 정규식을 사용하여 특정 UIView 클래스를 찾아 출력하는 명령어

```
(lldb) fv UIButton
```

### Visualize Command

 **visualize** - UIImage, CGImageRef, UIView, CALayer 등을 이미지로 만들어 Preview로 열어 보여주는 명령어

```
(lldb) visualize self.view
```

### **Display Commands**

**caflush** - 즉각적으로 화면을 다시 그리도록 하는 명령어

```
(lldb) fv UILabel
0x7fd17b60d720 UILabel
(lldb) e [((UILabel*) 0x7fd17b60d720) setBackgroundColor:[UIColor blueColor]]
(lldb) caflush
```

**border/unborder** - 해당 View 또는 Layer에 border를 설정하거나 끄는 명령어

```sh
(lldb) border 0x7f80f2d11b40
(lldb) border 0x7f80f2d11b40 --color blue
(lldb) border 0x7f80f2d11b40 --color blue --width 10
(lldb) unborder 0x7f80f2d11b40 # 특이했던건 border self.view 은 적용되는데 unborder self.view 는 안됨. 주소값 넣어줘야.
```

**mask/unmask** - 투명한 사각형을 View 또는 Layer 위에 노출시키거나 끄는 명령어

```
(lldb) fv UILabel
0x7fdfeee04200 UILabel
(lldb) mask 0x7fdfeee04200 --color red
(lldb) unmask 0x7fdfeee04200
```

**show/hide** - 특정 UIView나 CALayer를 숨기거나 보여주는 명렁어

```
(lldb) hide 0x7f80f2c01f20
(lldb) show 0x7f80f2c01f20
```

- 더 많은 명령어가 궁금하다면 아래 링크들 참고
  - [\[Xcode\][LLDB]Debugging With Xcode, LLDB and Chisel](https://minsone.github.io/ios/mac/xcode-lldb-debugging-with-xcode-lldb-and-chisel) 
  - https://github.com/facebook/chisel/wiki
