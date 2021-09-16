# :one: expression 명령어를 이용해 값 출력하기 ( feat. p, po, v 명령어)

## `expression` 명령어

- lldb 의 가장 유용하고 강력한 명령어 중 하나

- 브레이크 포인트가 멈춰있는 동안 **새로운 동작을 실행**시킬 수 있음

- 객체에 대한 다양한 정보를 **프린트** 할 수 있을 뿐만 아니라 **코드를 실행**하거나 컴파일하지 않고 기존 **변수를 수정**할 수도 있음 

- lldb 포맷에서 특정 variable 의 값을 print 하기 위해서는 expression 명령어를 사용

  ```sh
  (lldb) expresssion <variable>
  
  # ex) 
  # expression self.view
  ```

- expression 명령어를 입력하고나면 결과 값으로 \$R0 과 같은 이름이 지정됨. 이 값을 이용할 수도 있음

  ```sh
  (lldb) expresssion myValue
  # 출력
  (Int) $R2 = 3
  
  (lldb) e $R2
  # 출력
  (Int) $R3 = 3
  ```

- expression을 evaluate 할 때도 동일한 expression 명령어 사용

  ```sh
  (lldb) expression <expression> 
  
  # ex) 
  # expression myFunction() 
  # expression 1+2
  ```

- `--ignore-breakpoints <boolean> ` (`-i <boolean> `) option 을 통해 expression 실행 중 만나는 breakpoint를 ignore 여부 선택 가능. (default는 –ignore-breakpoint true)

  ```sh
  # 그냥 expression 함수명을 입력하면 해당 함수를 실행함. 이때 함수 안에 걸려있는 break point는 무시
  (lldb) expression myFunction()
  
  #  실행 도중 breakpoint를 만나도 그냥 진행 (default)
  (lldb) expression -ignore-breakpoints true -- myFunction()
  (lldb) e -i 1 -- helloWorld() # true, false 여부를 1 또는 0 으로 바꿀 수도 있음.
  
  #  실행 도중 breakpoint를 만나면 멈춤
  (lldb) expression -ignore-breakpoints false -- myFunction()
  (lldb) e -i 0 -- helloWorld()
  ```

- `expression` 은 짧게 `expr`, `ex`, `e` 등으로 사용할 수 있음

  ```sh
  (lldb) expresssion <variable>
  (lldb) expr <variable>
  (lldb) ex <variable>
  (lldb) e <variable>
  ```



### 옵션

- 옵션은 앞에 `--` 를 붙여서 사용. 옵션을 줄여서 표현할때는 `-` 처럼 대쉬를 하나로 표현. 또한 반드시 `--` 를 option과 raw input 사이에 넣어야함.

  ```
  (lldb) expression --depth 1 -- <variable>
  (lldb) expression -D 1 -- <variable>
  ```

- 사실 옵션을 풀네임으로 표현하고 대쉬를 한개만 해도 실행은 되는듯?

  ```
  (lldb) expression -depth 1 -- <variable>
  ```

#### `–-depth <count>` (`-D <count>`) 

- aggregate type 을 dump 할 때 최대 recurse depth 설정 가능 (default 는 infinity)

```sh
(lldb) e -D 1 -- self

# 출력
(sample.ViewController) $R0 = 0x00007fb5f2a052f0 {
  UIKit.UIViewController ={...}
  myValue = 1{...}
}
```

## `p` 명령어

- `print` 의 약자
- 사실 `expression -- ` 명령어의 축약형
- 엥 그럼 `expression` 명령어랑 동일한거 아니냐? 👉🏻  `print` 명령어는 flag나 추가적인 argument 를 받지 않음

```sh
  # e 명령어
  (lldb) e -D 1 -- self
  # 출력
  (sample.ViewController) $R1 = 0x00007fb5f2a052f0 {
    UIKit.UIViewController ={...}
    myValue = 1{...}
  }

  # p 명령어
  (lldb) p -D 1 -- self
  # 출력
  error: <EXPR>:8:3: error: consecutive statements on a line must be separated by ';'
  -D 1 -- self
    ^
    ;
```

- 데이터를 표현하는 형식은 lldb 의 `formatters` 를 이용

## `po` 명령어

- 가장 빈번하게 사용되는 lldb 명령어인듯 (ex. 🤔 lldb 아세요?  🙄 po 는 알아요)

- `expression --object-description --` 의 축약형
- `--object-description` ( `-O`) 옵션은 object의 설명 (debugDescription) 을 출력하겠다는 의미

```sh
  (lldb) expression -O -- self
  (lldb) po self
  
  # 출력
  <sample.ViewController: 0x7f8e3a405c70>
```

- `po`가 출력하는 description은 `NSObject`의 `debugDescription` . 따라서 해당 프로퍼티를 override 하면 po 결과 customize 가능

```swift
override var debugDescription: String {
   return "sujinnaljin의 description \(super.debugDescription)"
}
```

  ```sh
(lldb) po self

# 출력
sujinnaljin의 description <sample.ViewController: 0x7fc66ed08850>
  ```

- `p` 에서 제공하는 표현과 **형식은 약간 다르지만 동일한 정보**를 담고 있음 (`p` 나 `v` (`frame variable`) 는 lldb의 `formatters` 를 이용하고, (`po` 는 `debugDescription` 정보를 이용)

```swift
struct Trip {
    var name: String
    var desinations: [String]
}
let cruise = Trip(name: "naljin cruise", desinations: ["Sorrento, Capri, Taormina"])
```

```sh
# po 명령어
(lldb) po cruise
# 출력
▿ Trip
  - name : "naljin cruise"
  ▿ desinations : 1 element
    - 0 : "Sorrento, Capri, Taormina"

# p 명령어
(lldb) p cruise
# 출력
(sample.Trip) $R4 = {
  name = "naljin cruise"
  desinations = 1 value {
    [0] = "Sorrento, Capri, Taormina"
  }
}
```



## `v` 명령어

- `frame variable` 의 약자 (not `expression`)
- output으로 출력되는 형식이 p 명령어와 같음 👉🏻 p와 동일하게 lldb 의 `formatter` 를 이용하기 때문
- 다만 p나 po 명령어와 달리 코드를 compile & evaluate 하지 않음. 그냥 메모리에 있는거 바로 읽어옴. 따라서 속도가 매우 빠름. 
- 코드를 compile & evaluate 하지 않기 때문에 현재 스택 프레임에 있는 변수에만 액세스 가능하고, computed property는 계산 불가 (exectued 되어야하기 때문)

```swift
struct Trip {
    var name: String
    var subName: String {
        return "sub" + name
    }
    var desinations: [String]
}
let cruise = Trip(name: "naljin cruise", desinations: ["Sorrento, Capri, Taormina"])
```

```sh
# v 명령어 - computed property 계산 불가
(lldb) v cruise.subName
error: "subName" is not a member of "(sample.Trip) cruise"
# v 명령어 - expression evaluate 불가
(lldb) v cruise.name.isEmpty
error: "isEmpty" is not a member of "(Swift.String) cruise.name"

# p 명령어 - computed property 계산 가능
(lldb) p cruise.subName
(String) $R1 = "subnaljin cruise"
# p 명령어 - expression evaluate 가능
(lldb) p cruise.name.isEmpty
(Bool) $R4 = false
```


### 이전 👈🏻
- [0️⃣ 들어가기 전에](https://github.com/sujinnaljin/Improving_Productivity/blob/main/contents/beforeStart.md)
### 다음 👉🏻
- [2️⃣ expression 명령어를 이용해 값 변경 및 변수 선언해보기](https://github.com/sujinnaljin/Improving_Productivity/blob/main/contents/modifyUsingExpression.md)
