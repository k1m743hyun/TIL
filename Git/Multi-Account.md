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

### 회사용 Directory `~/company`와 개인용 Directory `~/personal` 생성
```
$ cd ~
$ mkdir company
$ mkdir personal
```

### `~/.gitconfig`에 특정 Directory에서 사용할 Git 설정 내용 추가
```
$ vi ~/.gitconfig
```
```
[includeIf "gitdir:~/company/"]
  path = ~/company/.gitconfig
  
[includeIf "gitdir:~/personal/"]
  path = ~/personal/.gitconfig
```

### 회사용 `.gitconfig`와 개인용 `.gitconfig` 사용자 정보 파일 생성
회사용 `.gitconfig`
```
$ vi ~/company/.gitconfig
```
```
[user]
  name = Taehyun Kim
  email = k1m743hyun@gmail.com
```
개인용 `.gitconfig`
```
$ vi ~/personal/.gitconfig
```
```
[user]
  name = Taehyun Kim
  email = k1m743hyun@icloud.com
```

## SSH Key 생성
회사용 SSH Key 생성
```
$ ssh-keygen -t rsa -b 4096 -C "k1m743hyun@gmail.com"
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
Host github.com-company
  User git
  Port 22
  HostName github.com
  IdentityFile ~/.ssh/id_rsa_company

Host github.com-personal
  User git
  Port 22
  HostName github.com
  IdentityFile ~/.ssh/id_rsa_personal
```

### GitHub Repository Clone 받기
회사용 Repository Clone
```
$ cd company
$ git clone git@github.com-company:{user}/{your-repo-name}.git
```
개인용 Repository Clone
```
$ cd personal
$ git clone git@github.com-personal:k1m743hyun/TIL.git
```


# 출처
- [GitHub 멀티 어카운트를 사용할 때 유용한 Git 설정](https://www.lainyzine.com/ko/article/useful-git-settings-when-using-github-multi-account/)
- [Handling Multiple Github Accounts on MacOS](https://gist.github.com/Jonalogy/54091c98946cfe4f8cdab2bea79430f9)