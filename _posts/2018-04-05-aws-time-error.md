---
layout: post
title: 'AWSError::An error occurred (InvalidSignatureException) when calling the GetAuthorizationToken operation'
tags: ['error']
date: 2018-04-05T21:53:00+09:00
---

<!--more-->

안녕하세요 상일입니다. 이번 포스트에서는 제가 Docker 이미지를 배포할 때 AWS ECS에서 겪은(?) 에러에 대해서 포스팅 해보겠습니다.

얼마 전에 도커 이미지를 배포하다가 다음과 같은 에러가 발생 했는데요..

## Error log

```bash
An error occurred (InvalidSignatureException) when calling the GetAuthorizationToken operation: 
Signature not yet current: 20180404T155030Z is still later than 20180404T070416Z (20180404T064916Z + 15 min.)
```

위의 에러로그를 잘 보시면 어떤 에러인지 감이 올 것입니다.
이 에러가 발생하는 이유는 간단히 말해서 요청에 서명된 시간과 요청시간이 일치하지 않아서 입니다. 따라서, 서버시간만 동기화 해주면 해결되는 에러입니다.

아래 인용한 문구는 AWS가 왜 이러한 에러를 발생시키는지에 대해서 설명한 것입니다.

>  많은 서버 작업과 프로세스에서 일관되고 정확한 시간 참조가 중요합니다. 대부분의 시스템 로그에는 문제가 발생한 시간과 이벤트가 발생한 순서를 파악하는 데 사용할 수 있는 타임스탬프가 포함되어 있습니다. AWS CLI 또는 AWS SDK를 사용하여 인스턴스에서 요청하는 경우 이러한 도구가 사용자를 대신하여 요청에 서명합니다. 인스턴스의 날짜와 시간이 잘못 설정되어 있으면 서명 날짜가 요청 날짜와 일치하지 않아 AWS가 해당 요청을 거부할 수 있습니다.
>> [Linux 인스턴스의 시간 설정 - Amazon Elastic Compute Cloud](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/set-time.html)

## Solution

1. REHEL 계열

```bash
yum erase ntp*
yum -y install chrony

# /etc/chrony.conf 명령어가 포함 되어있는지 확인 후 추가
# server 169.254.169.123 prefer iburst
service chronyd start
```

2. Debian 계열

```bash
apt install -y chrony

# /etc/chrony/chrony.conf 명령어가 포함 되어있는지 확인 후 추가
# server 169.254.169.123 prefer iburst
/etc/init.d/chrony restart
chronyc sources -v
```

## Reference
* https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/set-time.html (영어)
* https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/set-time.html (한국어)