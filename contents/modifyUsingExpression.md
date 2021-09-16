# :two:  expression 명령어를 이용해 값 변경 및 변수 선언해보기

- expression 명령어를 이용해 Runtime에 여러 정보를 출력할 수 있을 뿐아니라 **값을 변경** 할 수도 있음

```sh
(lldb) e self.isVisible = true
```

- `expression` Command를 이용해서 **변수를 직접 선언**해서 사용할 수도 있음. 단, 사용하고자 하는 변수명 앞에 `$` 문자를 붙여야 함

```
(lldb) e let $someNumber = 10
(lldb) e var $someString = "some string"
```

- 활용 예시

```sh
# 임의의 View 생성해서 얹기
(lldb) e var $view = UIView(frame: CGRect(x: 100, y: 100, width: 80, height: 80))
(lldb) e $view.backgroundColor = UIColor.blue
(lldb) e self.view.addSubview($view)
(lldb) c

# 임의의 UIViewController 생성하여 NavigationViewController에 Push
(lldb) e # `(lldb) expression` 명령어를 입력한 후 return키를 입력하면, Multi-line Command를 입력할 수 있음
Enter expressions, then terminate with an empty line to evaluate:
1 var $vc = UIViewController()
2 $vc.view.backgroundColor = UIColor.red
3 self.navigationController?.pushViewController($vc, animated: true)
4 
```

- 변수 말고도 method 및 class 도 만들어서 사용 가능

```sh
(lldb) e class $A { var $b = 0 }
(lldb) po $A().$b
0
```

- extension에 함수를 만들어 사용할 수도 있음

```sh
(lldb) e extension ViewController { func $changeBgColor() { self.view.backgroundColor = .red } }
(lldb) e self.$changeBgColor()
```
### 이전 👈🏻
- [1️⃣ expression 명령어를 이용해 값 출력하기 ( feat. p, po, v 명령어)](https://github.com/sujinnaljin/Improving_Productivity/blob/main/contents/printUsingExpression.md)
### 다음 👉🏻
- [3️⃣ expression 명령어와 unsafeBitCast를 이용해 시뮬레이터에서 속성 바뀌는것 바로 파악하기](https://github.com/sujinnaljin/Improving_Productivity/blob/main/contents/castUsingExpression.md)
