---
layout: post
title: "리눅스 file descriptor 확인 및 설정 "
tags: ['linux']
date: 2017-03-31T11:30:54+09:00
---

<!--more-->
![출처: http://q-linux.com/how-linux-certification-can-help-you/](/thumbnails/python1.png)

최근 개발 중인 서버에 load test를 하다가 발생한 에러 때문에 알게 된 리눅스의 소켓 문제에 직면하게 되어 그에 관해서 얘기해보려 합니다.

우선 시작 전에 제가 개발하고 있는 서버에 대해서 말씀드리면 nginx <--> uWSGI <--> Django로 구성되어 있습니다.

```bash
ConnectionError(MaxRetryError("HTTPConnectionPool(host='www.example.com',
port=80): Max retries exceeded with url: /v1/test (Caused by
NewConnectionError('<requests.packages.urllib3.connection.HTTPConnection
object at 0x....>: Failed to establish a new connection: [Errno 60] Operation 
timed out',))",),)
```

위 로그에서 본 에러의 내용인즉슨 너가 할 수 있는 최대 연결치를 초과했다는 내용입니다.

저도 처음에는 이해가 되질 않아 검색하고 선임분들에게 여쭤보고 알게 된 내용 중 리눅스에는 `file opens`라는 옵션이 있는데 이것이 생성할 수 있는 소켓의 개수와도 연관된다는 것이었습니다.
속성명이 `file opens(default=1024)`라고 보면 파일을 열 수 있는 수량이라고 생각할 수 있습니다. 저또한 그렇게 생각했지만, 리눅스에서는 socket을 여는 작업도 파일을 여는 것과 같은 의미라고 하기에 그 값을 조정해야 했습니다.

```bash
vi /etc/security/limits.conf
# 파일 오픈 후 아래의 내용 추가
<user_id> hard nofile 65535
<user_id> soft nofile 65535
<user_id> hard nproc 65535
<user_id> soft nproc 65535
```
(*파일 수정 후 리눅스 재시작*)

파일을 오픈하면 각 속성에 대해서 주석으로 설명해 놓은 부분이 있으니 설정에 대해서는 따로 언급하지 않겠습니다. 값을 수정 후 다시 테스트한 결과는 역시 동일한 에러를 내뿜어 저를 멘붕에 빠지게 하였습니다.. 다시 심기일전하여 검색하던 중 nginx service에 `LimitNOFILE`라는 속성값을 알게 되어 아래와 같이 수정 하였습니다.

```bash
mkdir -p /lib/systemd/system/nginx.service.d
vi /lib/systemd/system/nginx.service.d/worker_files_limit.conf

# 아래의 값 입력 후 저장
[Service]
LimitNOFILE=16384

# 서비스 재시작
systemctl daemon-reload
systemctl restart nginx.service
```

이렇게 수정 후 테스트 해보니 커넥션 에러가 더 이상 발생하지 않았습니다.

리눅스에 대해서 많이 부족함을 저 스스로 느낀 하루였습니다. 매일 매일 부족함을 깨닫고 하나하나 채워가보려 합니다. 공부하면서 정리한 포스트이기에 부족한 부분이나 틀린 부분이 있을 수 있습니다. 그 점들에 대해서 피드백을 남겨 주신다면 저 뿐만이 아니라 혹시나 저와 같은 문제로 고생하시는 분들에게 정말 도움이 될 거 같습니다.  
  
감사합니다.