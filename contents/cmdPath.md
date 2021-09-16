# :nine: 폴더 경로 cmd 에서 쉽게 잡기

## shortcut으로 폴더에서 새로운 cmd 열기

- 환경설정 > 키보드 > 단축키 > 서비스 > 폴더에서 새로운 터미널 열기 shortcut 지정 (item 쓰고 있다면 'New Iterm2 Tab Here' 설정 가능)

![image](https://user-images.githubusercontent.com/20410193/133589360-4b19142f-b68b-42f5-b8fe-f8096bd4682b.png)

![image](https://user-images.githubusercontent.com/20410193/133589399-654f05a5-c1f2-46f2-ac1d-d474623da0e8.png)

## custom 명령어를 통해 열려 있는 Finder 경로로 이동하기  

- 아래 명령어를 현재 사용중인 **bash_profile** 이나 **zshrc** 등에 복붙 (ex. zsh 을 쓰고 있다면 `~/.zshrc` 에 복붙)

```
# cd into whatever is the forefront Finder window.
cdf() {  # short for cdfinder
  cd "`osascript -e 'tell app "Finder" to POSIX path of (insertion location as alias)'`"
}
```

- finder 여러개 있으면 마지막에 열려있는 finder 기준으로 동작

### 이전 👈🏻
- [8️⃣ Xcode 단축키](https://github.com/sujinnaljin/Improving_Productivity/blob/main/contents/xcodeShortcuts.md)
