---
title: Jekyll 블로그 만들기 간단 메모
author: Rito15
date: 2021-06-15 14:55:00 +09:00
categories: [Memo]
tags: [memo]
math: true
mermaid: true
---

## 1. Ruby 설치

- <https://rubyinstaller.org/downloads/>
- 2.7.3

<br>

## 2. Jekyll Bundler 설치

- cmd 켰을 때 나오는 사용자 기본 경로에 설치

```
gem install jekyll bundler
```

<br>

## 3. 블로그용 깃헙 원격 리포 준비

- 리포 이름은 `닉네임.github.io`

<br>

## 4. 로컬 리포 준비

- 원격 리포랑 연결

```
git init
git add .
git commit -m "init"
git branch -M main
git remote add origin [깃헙 원격 리포 주소]
git push -u origin main
```

<br>

## 5. Jekyll 테마 다운로드

- <http://jekyllthemes.org/>

- 로컬 리포에 그대로 넣어주기

<br>

## 6. 번들 설치

- cmd로 해당 로컬 리포 경로에 이동 후

```
bundle install
```

<br>

간혹 에러를 띄우는 경우가 있는데, 에러 메시지에

```
To update to the latest version installed on your system, run `bundle update --bundler`
```

라던가

```
To install the missing version, run `gem install bundler:1.16.3`
```

같은게 써있다면,

그대로 잘 실행해주고 다시 `bundle install` 해주면 된다.

<br>

## 7. 로컬호스트로 실행

```
bundle exec jekyll serve
```

- <http://localhost:4000>

<br>

## 8. docs 폴더에 내용물 넣어주고 깃헙 업로드

```
echo Y| rmdir docs /s
mkdir docs
xcopy "_site\*.*" "docs\" /e /y

git pull
git add .
git commit -m "Upload"
git push
```

<br>

## 9. [최초 한번만] 깃헙 페이지 설정

- 원격 리포 - `Settings` - `Pages`
- `Source` - `Branch:main` - `/docs` - `Save`
