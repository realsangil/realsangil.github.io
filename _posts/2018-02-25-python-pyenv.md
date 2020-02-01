---
layout: post
title: "pyenv virtualenv, autoenv 환경 꾸리기 (feat. bash, zsh, fish)"
tags: ['pyenv', 'virtualenv', 'autoenv']
date: 2018-02-25T02:44:20+09:00
---

<!--more-->
## pyenv란?
pyenv는 다양한 파이썬 버전들을 쉽게 스위칭하고 관리할 수 있도록 도와주는 툴이다.
이미지를 보면 좀 더 이해하기 쉽다.
![출처: https://qiita.com/hedgehoCrow/items/0733c63c690450b14dcf](https://camo.qiitausercontent.com/2c9d3d97afe99e49d4b3a924ebb2544f374246ca/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e616d617a6f6e6177732e636f6d2f302f3131363039322f36353436656665642d303932302d393861342d633737342d6162303563623065343830342e706e67)
## virtualenv란?
virtualenv는 프로젝트별로 독립된 파이썬 환경을 만들 수 있도록 도와주는 툴이다. 보통 파이썬 라이브러리는 `/usr/lib/python2.7/site-packages`에 설치되고 관리한다. 이때 서로 다른 프로젝트가 `foo`라는 라이브러리를 사용한다고 가정해보자. 프로젝트 1은 foo1.0, 프로젝트2는 foo2.0을 사용한다 이런 경우 같은 환경에서 프로젝트를 관리하면 버저닝 문제가 발생할 확률이 매우 높다. 이런 경우를 해결하기 위해 virtualenv를 사용한다.
이미지를 보면 좀 더 이해하기 쉽다.
![출처: https://qiita.com/hedgehoCrow/items/0733c63c690450b14dcf](https://camo.qiitausercontent.com/02f421a83afe95f4d10569c62397e2ef4ff4c59f/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e616d617a6f6e6177732e636f6d2f302f3131363039322f39663766313332632d346132662d383731622d616163372d6233343464643334313637632e706e67)

## autoenv란?
프로젝트 디렉토리에 `.env` 파일이 존재할 때 `.env`파일 안에 있는 내용을 자동으로 실행해주는 툴이다 예를 들면 `pyenv activate [VIRTUALENV_NAME]`을 넣어주면 해당 디렉토리로 이동했을 때 자동으로 virtualenv를 활성화 해준다. 편의성으로는 아주 굳굳한 툴이다.

## Install
설치는 macOS를 기준으로 설명하겠다.
```bash
# pyenv 설치
brew install pyenv
# virtualenv 설치
brew install pyenv-virtualenv
# autoenv 설치
brew install autoenv
```

**환경변수 설정**
구글에 검색해보면 이미 많은 블로그에서 pyenv와 virtualenv, autoenv를 설치하는 글들이 많다 하지만 나는 거기에 다양한 터미널(bash, zsh, fish)에서 설치할 수 있는 팁을 덧붙여 설명해보려 한다.

_아래의 명령어들은 터미널을 실행 할 때마다 새로운 세션을 만들고 적용하기 위한 설정이다. 쉽게 말해서 터미널을 오픈할 때 자동으로 실행되지 않게 하려면 아래의 명령어를 터미널 환경 파일에 기입하지 않고 실행 사용하지 않고 직접 타이핑 하면 된다._

**bash 환경**
```bash
# pyenv
echo eval "$(pyenv init -)" >> ~/.bash_profile
#virtualenv
echo eval "$(pyenv virtualenv-init -)" >> ~/.bash_profile
#autoenv
echo 'source /usr/local/opt/autoenv/activate.sh' >> ~/.bash_profile
```

**zsh 환경**
```bash
# pyenv
echo eval "$(pyenv init -)" >> ~/.zshenv
#virtualenv
echo eval "$(pyenv virtualenv-init -)" >> ~/.zshenv
#autoenv
echo 'source /usr/local/opt/autoenv/activate.sh' >> ~/.zshenv
```

**fish 환경**
`~/.config/fish/config.fish`에 아래의 라인들을 추가하면 된다.
```bash
# pyenv
set -x PATH "$HOME/.pyenv/bin" $PATH
status --is-interactive; and . (pyenv init - | psub)
#virtualenv
status --is-interactive; and source (pyenv virtualenv-init -|psub)
pyenv rehash
# autoenv
brew tap loopbit/tap
brew install autoenv_fish
echo 'source /usr/local/opt/autoenv_fish/activate.fish' >> ~/.config/fish/config.fish
```

## Usage
```bash
# python 설치
pyenv install [PYTHON_VERSION]

# python 버전 변경
# 버전 변경에 대한 다양한 커맨드가 있지만
# 그 부분은 한번 찾아보길 권장한다.
pyenv global [PYTHON_VERSION]

# .env 파일 생성
echo pyenv activate [VIRTUALENV_NAME] >> .env
```

## 마치며
간략하게 글을 썼지만 충분히 설명 됐으리라 믿습니다.
_이 글에서 잘못된 점이나 보완할 점이 있다면 이메일(tkddlf59@gmail.com)이나 댓글로 알려주시면 수정하도록 하겠습니다._

## Reference 
 - [pyenv + virtualenv + autoenv 를 통한 Python 개발 환경 구축하기](https://ansuchan.com/how-to-set-python-dev-env/)
