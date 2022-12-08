# Github Multi-Account를 사용하기 위한 Git 설정
  

## Global로 설정된 사용자 이름과 이메일 정보 삭제 방법
```
$ git config --global --unset user.name
$ git config --global --unset user.email
```
  

## Git 사용자 이름과 이메일 정보가 없을 때 Commit을 막는 방법
```
$ git config --global user.useConfigOnly true
```
  

## 특정 Directory 아래 저장소들의 사용자 정보 설정하기
  

### 업무용 Directory `~/thkim0022`와 개인용 Directory `~/k1m743hyun` 생성
```
$ cd ~
$ mkdir thkim0022
$ mkdir k1m743hyun
```
  

### `~/.gitconfig`에 특정 Directory에서 사용할 Git 설정 내용 추가
```
$ vi ~/.gitconfig
```
```
[includeIf "gitdir:~/thkim0022/"]
  path = ~/thkim0022/.gitconfig
  
[includeIf "gitdir:~/k1m743hyun/"]
  path = ~/k1m743hyun/.gitconfig
```
  

### 업무용 `.gitconfig`와 개인용 `.gitconfig` 사용자 정보 파일 생성
업무용 `.gitconfig`
```
$ vi ~/thkim0022/.gitconfig
```
```
[user]
  name = Taehyun Kim
  email = thkim0022@icloud.com
```
  

개인용 `.gitconfig`
```
$ vi ~/k1m743hyun/.gitconfig
```
```
[user]
  name = Taehyun Kim
  email = k1m743hyun@icloud.com
```
  

## SSH Key 생성
업무용 SSH Key 생성
```
$ ssh-keygen -t rsa -b 4096 -C "thkim0022@icloud.com"
```
  

개인용 SSH Key 생성
```
$ ssh-keygen -t rsa -b 4096 -C "k1m743hyun@icloud.com"
```

## SSH Config 수정
```
$ vi ~/.ssh/config
```
```
Host github.com-thkim0022
  User git
  Port 22
  HostName github.com
  IdentityFile ~/.ssh/id_rsa_thkim0022

Host github.com-k1m743hyun
  User git
  Port 22
  HostName github.com
  IdentityFile ~/.ssh/id_rsa_k1m743hyun
```
  

### GitHub Repository Clone 받기
업무용 Repository Clone
```
$ cd thkim0022
$ git clone git@github.com-thkim0022:{user}/{your-repo-name}.git
```
  

개인용 Repository Clone
```
$ cd k1m743hyun
$ git clone git@github.com-k1m743hyun:k1m743hyun/TIL.git
```
  
  
# 출처
- [GitHub 멀티 어카운트를 사용할 때 유용한 Git 설정](https://www.lainyzine.com/ko/article/useful-git-settings-when-using-github-multi-account/)
- [Handling Multiple Github Accounts on MacOS](https://gist.github.com/Jonalogy/54091c98946cfe4f8cdab2bea79430f9)