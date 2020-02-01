---
layout: post
title: "delve error: 'could not launch process': EOF 에러 해결법"
tags: ['golang', 'delve']
date: 2018-04-04T08:30:00+09:00
---

macOS 버전 업데이트에 따른 delve 에러 해결하기
<!--more-->

## Problem

시작하기에 앞서 delve에 대해서 간략하게 소개해 드리겠습니다.  
Go는 아직까지 정식 debugger가 없어 디버깅할 때 delve라는 툴을 사용해서 디버깅을 합니다.
이 툴은 `vscode-go`에서도 사용중인 debugger이며 유료지만 진짜 갓갓한 `Goland` 내부에서도 delve를 사용할 정도로 유명한 debugger 입니다. 

참고로 저는 `Visual Studio Code + vscode-go`를 사용하여 Go application을 작성하고 있습니다.
이틀전 OSX를 `10.13.4` 버전으로 업데이트 한 후 정확히 말하면 `CommandLineTools`를 업데이트한 후 vscode에서 프로젝트를 빌드하고 `delve`로 디버깅할 때 에러가 발생하여 이 문제를 어떻게 해결할 수 있는지에 대해서 간략하게 써보도록 하겠습니다.

```bash
could not launch process: EOF
Process exiting with code: 1
```

처음 위와 같은 에러가 발생했을 땐 약간 어리둥절 했습니다. **아니 코드를 고친게 없는데 왜 안돼!!!!!!!!!!**
그래서 [delve repository](https://github.com/derekparker/delve)에서 [issue](https://github.com/derekparker/delve/issues/1015)를 검색해보니 저와 똑같은 에러가 보고 되었고 친절히도 해결법도 함께 제시해 주었습니다.

## Solution

### 1. CommandLineTools 삭제
```bash
sudo rm -rf /Library/Developer/CommandLineTools
```

### 2. Command Line Tools (macOS 10.13) for Xcode 9.2 - Dec 4, 2017 버전으로 설치

![photho1](/images//2018-04-04-delve-error01/photo1.png)

https://developer.apple.com/download/more/
위 링크에서 Command Line Tools (macOS 10.13) for Xcode 9.2 - Dec 4, 2017 버전으로 설치

## Conclusion
OSX 버전이 `10.13.4`로 업데이트 되면서 `CommandLineTools`도 함께 업데이트 되었는데 delve에서 아직 대응을 못해서 나는 에러이다.
위의 방법은 임시적으로 할 수 있는 방법이고 delve 측에서 대응이 끝나면 `CommandLineTools`도 함께 업데이트 하는걸 추천한다.

## Reference
 - [dlv exec throws "could not launch process: EOF"](https://github.com/derekparker/delve/issues/1015)