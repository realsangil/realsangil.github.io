---
layout: post
title: "Go 언어를 소개합니다."
tags: ['golang']
date: 2018-03-02T14:37:57+09:00
---

<!--more-->

![thumbnail](/thumbnails/golang1.jpg)

## Go 언어?

Go 언어를 들어보신 분도 계실 것이고 처음 들어보신 분도 계실 것입니다. 그래서 오늘 Go에 대해서 간단하게 소개해드릴려고 합니다.  

[Go 웹사이트](golang.org)에서는 Go를 다음과 같이 설명하고 있습니다.  
> Go는 간결하고 신뢰성 있으며 효율적인 소프트웨어를 손쉽게 만들기 위한 오픈소스 프로그래밍 언어다.

Go는 프로그래밍 언어로 간결한 문법으로 생산성 높은 프로그래밍을 할 수 있습니다. GC(garbage collection)을 지원합니다. 또한 타입 시스템은 정적 자료형으로 동적 타입으로 개발할 때 생기는 부주의한 실수를 방지함과 동시에 `:=` 연산자로 동적 자료형을 코딩하는 듯한 느낌을 주기도 합니다. 또한 일반 프로그래밍 언어에서 제공하는 스레드보다 가벼운 `goroutine`을 이용해서 쉽게 동시성을 지원하도록 코딩할 수 있습니다. 또한 "배터리 포함"이란 개념이 적용된 Go의 표준 라이브러리는 I/O, 텍스트 처리, 그래픽, 암호화, 네트워킹 등을 위한 요소들을 제공합니다. 

## 생산성이 높다?

위의 소개에서 생산성이 높다라고 설명했는데 어떻게 생산성을 높여주는지 한번 간단하게 설명해 보겠습니다.

  - `:=` 연산자를 통해 자료형을 추론해서 선언이 가능합니다.
  - `gofmt`라는 도구를 통해 소스 코드 형식을 자동으로 맞춰줍니다. 어떻게 보면 소스코드 포멧을 강제하기 때문에 python에서 pep8를 사용하여 얻을 수 있는 장점을 똑같이 얻을 수 있습니다. 또한 이게 IDE와 합쳐지면 소스 코드를 저장할 때 코드를 자동으로 포맷해주기 때문에 써보면 진짜 편합니다.
  - 컴파일 속도가 아주 빨라서 컴파일과 테스트를 반복적으로 수행하면서 작성하기 용이합니다.
  - GC 지원.. 무슨 말이 필요하겠습니까...
  - 명시적으로 interface를 지정하지 않아도 interface 구현이 가능하여 기존에 있던 코드를 고치지 않고도 interface 구현이 가능합니다.

## 설치

저는 개발하는 머신의 OS가 OSX이기 때문에 `Homebrew`를 통해 쉽게 Go를 설치하였습니다. 사담이지만 진짜 OSX를 사용하는 입장에서 `Homebrew`는 갓갓합니다.

```bash
# 한 라인이면 go를 설치 할 수 있습니다.
brew install go
```

_`GOPATH`에 대해서는 [Go workspace 구조](#go-workspace-%EA%B5%AC%EC%A1%B0)에서 설명 드리겠습니다._  
다양한 플랫폼 및 자세한 설치 방법은 [링크](https://golang.org/doc/install)로 이동하셔서 참고하시면 됩니다.

## Go workspace 구조
Go의 workspcae 구조에 대해 설명하기 앞서 `GOROOT`와 `GOPATH`에 대해서 간단히 설명 드리겠습니다.
 - `GOROOT`는 쉽게 말해 Go가 설치된 디렉터리를 가리키는 환경변수라고 설명할 수 있습니다.
 - `GOPATH`는 Go workspcae 디렉터리를 가리키는 환경변수입니다.

GOPATH로 지정된 디렉터리(workspace)는 `pkg`, `bin`, `src`라는 3개의 디렉터리를 갖습니다.

**각각의 디렉터리에 대한 설명**
 - `src` 디렉터리는 내가 작성한 코드와 `go get`이나 웹에서 가져온 코드를 저장하는 디렉터리입니다. 
 - `bin` 디렉터리는 컴파일된 바이너리 파일을 저장하는 디렉터리입니다. 
 - `pkg` 디렉터리는 컴파일한 패키지의 라이브러리 파일이 생성되는 디렉터리입니다. `pkg` 디렉터리 아래에는 `<운영체제>_<아키텍쳐>` 형식으로 디렉터리가 생성됩니다. 64비트 리눅스라면 `linux_amd64` 디렉터리 아래에 라이브러리 파일이 생성됩니다.

```bash
GOPATH=/home/user/go
    /home/user/go/
        src/
            foo/
                bar/  (go code in package bar)
                    x.go
                quux/ (go code in package main)
                    y.go
        bin/
            quux (installed command)
        pkg/
            linux_amd64/
                foo/
                    bar.a (installed package object)
```

**Go의 환경변수를 확인하는 커맨드는 `go env`입니다.**  
![go env](/images//2018-03-01-go-instruction/go_env.png)


## 마무리하며..
아무래도 최근 Go를 사용하면서 파이썬으로 개발했던 입장이기에 조금씩 비교하면서 공부했습니다. 물론 러닝커브가 적은건 파이썬입니다만 Go도 러닝커브가 그리 크지 않다고 느꼈고 단순함을 강조하는 Go언어가 아주 맘에 들었습니다. 타언어를 공부해보셨다면 서로 비교하면서 공부하면 금방 배울 수 있는 언어라 생각됩니다.

_이 글에서 잘못된 점이나 보완할 점이 있다면 이메일(tkddlf59@gmail.com)이나 댓글로 알려주시면 수정하도록 하겠습니다._

## 공부할 때 유용한 사이트
 - [Go by Example 원문](https://gobyexample.com/)
 - [Go by Example 번역판](https://mingrammer.com/gobyexample/)
 - [페이스북 페이지 Golang Korea](https://www.facebook.com/groups/golangko)
 - [예제로 배우는 GO 프로그래밍](http://golang.site/)

