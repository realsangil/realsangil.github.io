---
layout: post
title: "Concurrency와 Parallelism의 차이"
tags: ["golang", "concurrency", "parallelism"]
canonical_url: ''
---

이 글은 Concurrency와 Parallelism의 차이점과 관계에 대해 개인적으로 공부한 내용을 간단한 예시와 함께 정리한 글입니다.
<!-- more -->

우선 제가 보고 가장 쉽게 차이점을 느꼈던 사례를 예로 들어보겠습니다. 
 
'철수가 신발끈이 풀린 채 조깅을 하는 중이라고 가정해봅시다. 철수는 끊이 풀린 걸 확인하고는 제자리에 멈춰 신발끈을 묶고 다시 달리기 시작했습니다.' 이 예시에서 철수는 '끊을 묶다'와 '달린다' 이 두가지 일을 동시에 하진 못하지만 나눠서 한번에 두 가지 일을 처리했습니다. 이를 **동시성(Concurrency)**이라고 합니다.  
  
이번엔 '철수가 조깅을 하면서 음악을 듣습니다.' 이번 예시에서는 철수가 조깅을 멈추지 않고 '달리다'와 '음악을 듣다'라는 행위를 동시에 하였습니다. 이것을 **병렬성(Parallelism)**이라고 합니다.

> "Concurrency is about dealing with lots of things at once. Parallelism is about doing lots of things at once."  
> \- Rob Pike, Concurrency is not Parallelism

Go언어 주요 개발자 중 한명인 '롭 파이크'는 말을 인용하면 '동시성은 한번에 여러가지 일을 다루는 것이다. 병렬성은 한번에 여러가지 일을 하는 것이다.' 

## 동시성(Concurrency)

| ![concurrency](/images/2020-02-03-concurrency-parallelism/concurrency.jpeg){: .center-image} |
| :--: |
| Concurrency |

동시성은 앞에서 언급했듯이 많은 일을 한번에 다루는 걸 의미합니다.

지금은 스마트폰 AP마저도 멀티코어로 구성되었지만 데스크탑이 싱글코어였던 시절에도 멀티태스킹을 할 수 있었습니다. 이것은 컴퓨터가 굉장히 빠른 속도로 여러가지 일을 나눠서 처리하여 보여주기 때문에 우리는 동시에 처리한다고 착각하게 되는 것입니다. 현실세계에서 병렬적으로 일어나는 일들을 컴퓨터 세계에서 구현하기 위한 방법 중 하나가 '동시성'입니다.

## 병렬성(Parallelism)

| ![parallelism](/images/2020-02-03-concurrency-parallelism/parallelism.jpeg){: .center-image} |
| :--: |
| Parallelism |

병렬성은 많은 일을 동시에 실행하는 것을 의미합니다. 프론트엔드에서 자주 사용되는 AJAX가 하나의 예가 될 수 있습니다. 예를 들어 글을 작성하는 중에 이미지에 대한 처리들은 서버에서 처리하도록 요청을 보내고 사용자가 글을 계속 작성할 수 있도록 하는 비동기적 처리들이 병렬성이라고 할 수 있습니다.

## Conclusion

이제는 롭 파이크가 동시성과 병렬성에 차이에 대해 말한 'Concurrency is about dealing with lots of things at once. Parallelism is about doing lots of things at once.'가 이해가 됩니다.

둘의 차이에 대해 공부하면서 저는 Go언어에서 goroutine들을 많이 사용했지만 병렬성과 동시성에 대해서 너무 막연한 생각을 가지고 있었던 것 같습니다.  

궁금하신 점이나 잘못된 정보들은 메일이나 댓글을 남겨주세요. 
감사합니다.🙏

## Reference
 - http://tutorials.jenkov.com/java-concurrency/concurrency-vs-parallelism.html
 - https://medium.com/@tilaklodha/concurrency-and-parallelism-in-golang-5333e9a4ba64
 - https://wiki.haskell.org/Parallelism_vs._Concurrency
 - https://stackoverflow.com/a/1050257
 - https://light-tree.tistory.com/25