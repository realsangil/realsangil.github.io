---
layout: post
title: 'golang 공용 모듈의 버전 간섭 문제 해결기'
date: 2019-12-22T13:11:05+09:00
tags: ['golang', 'modules']
---

공용 모듈 버전관리를 잘못하여 겪은 버전간섭 문제를 해결한 경험을 토대로 작성한 글입니다.
<!--more-->

![thumbnail](https://github.com/egonelbre/gophers/raw/master/.thumb/animation/gopher-dance-long-3x.gif)

go 1.13 버전으로 업데이트 되면서 go modules가 정식으로 릴리즈 되어 `dep`, `glide`를 별도로 설치하지 않고도 `go mod` 커맨드로 모듈들을 관리할 수 있게 되었습니다. 

## Problem
현재 제가 소속된 라라웍스는 Go로 개발할 때 사용하는 `gopkg`라는 공통 모듈이 있습니다.  
`gopkg`가 처음 만들어 졌을 때는 `gopkg` 자체에 버전 관리를 하고 있었습니다.  
문제는 모듈에 기능이 추가되고 리팩토링 되면서 코드가 변하는 과정에서 발생되었습니다.  

예를 들어 `gopkg v0.0.1`의 `log` 모듈에 정의된 `PrintInt` 함수가 `gopkg v0.0.2`에서 삭제되었다고 가정합니다.
이때 `v0.0.1`로 구현된 프로젝트에서 `PrintInt` 함수를 사용하고 `gopkg v0.0.2`의 `str` 모듈에 새롭게 정의된 함수가 필요할 때 `gopkg` 버전을 올리게 되면 삭제된 `PrintInt` 함수 때문에 에러가 발생하는 상황이 발생하였습니다.

## Solution
사실 이 문제는 공용 모듈 내에 존재하는 서브모듈의 버전을 따로 관리한다면 해결되는 간단한 문제였습니다.
이 문제를 해결하기 위해 각각의 모듈에 `go.mod` 파일을 정의하고 `submodule_name/v0.0.1`과 같은 포멧의 git tag를 생성하여 각 모듈의 버전을 따로 생성해 주었습니다.

```bash
# example
gopkg
├── log
│   ├── go.mod
│   └── log.go
└── str
    ├── go.mod
    └── string.go
```

전체적인 레포지토리 구조가 궁금하시다면 [realsangil/gopkg](https://github.com/realsangil/gopkg)를 참조하시면 됩니다.
  
혹시 저와 비슷한 문제로 불편함을 겪으신다면 각각의 모듈별로 버전을 관리하시길 추천드립니다.
  
궁금하신 점이나 잘못된 부분이 있다면 댓글이나 tkddlf59@gmail.com로 메일 남겨주시면 감사하겠습니다.

## Reference
- https://github.com/go-modules-by-example/index/tree/master/009_submodules