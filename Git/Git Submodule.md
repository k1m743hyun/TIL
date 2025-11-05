# Git Submodule 완전 가이드

Git Submodule을 사용하여 household-budget-common을 각 서비스에 통합하는 방법을 단계별로 설명합니다.

## 📋 목차

1. [Submodule이란?](#1-submodule이란)
2. [초기 설정](#2-초기-설정)
3. [Submodule 추가하기](#3-submodule-추가하기)
4. [Submodule 포함된 프로젝트 클론](#4-submodule-포함된-프로젝트-클론)
5. [Submodule 업데이트](#5-submodule-업데이트)
6. [Submodule 제거](#6-submodule-제거)
7. [실전 워크플로우](#7-실전-워크플로우)
8. [문제 해결](#8-문제-해결)
9. [Best Practices](#9-best-practices)

---

## 1. Submodule이란?

Git Submodule은 **한 Git 저장소를 다른 Git 저장소의 서브디렉토리로 포함**시키는 기능입니다.

### 왜 사용하나요?

```
✅ 공통 코드를 여러 프로젝트에서 공유
✅ 각 프로젝트는 독립적으로 버전 관리
✅ 공통 코드 업데이트를 선택적으로 반영
✅ 의존성 버전을 명확하게 관리
```

### 우리 프로젝트 구조

```
household-budget-msa/
├── auth-service/
│   ├── common/              ← submodule
│   └── src/
├── ledger-service/
│   ├── common/              ← submodule
│   └── src/
└── transaction-service/
    ├── common/              ← submodule
    └── src/
```

---

## 2. 초기 설정

### 2.1. Common Repository 생성

먼저 common을 별도 저장소로 push합니다.

```bash
# 1. GitHub에서 새 repository 생성
# Repository name: household-budget-common

# 2. Local에서 remote 추가 및 push
cd household-budget-common
git init
git add .
git commit -m "Initial commit: boilerplate v1.0.0"
git remote add origin https://github.com/your-org/household-budget-common.git
git push -u origin main

# 3. 버전 태그 생성 (중요!)
git tag -a v1.0.0 -m "Release v1.0.0"
git push origin v1.0.0
```

---

## 3. Submodule 추가하기

### 3.1. 기본 추가 방법

```bash
# Auth Service에 common 추가
cd auth-service
git submodule add https://github.com/your-org/household-budget-common.git common

# 결과
# - .gitmodules 파일 생성
# - common/ 디렉토리 생성
# - 변경사항 staging
```

### 3.2. 생성된 .gitmodules 파일

```ini
[submodule "common"]
    path = common
    url = https://github.com/your-org/household-budget-common.git
```

### 3.3. 특정 브랜치 추가

```bash
# develop 브랜치를 추적하도록 설정
git submodule add -b develop https://github.com/your-org/household-budget-common.git common
```

### 3.4. 커밋 및 푸시

```bash
git add .gitmodules common
git commit -m "Add household-budget-common as submodule"
git push
```

### 3.5. 모든 서비스에 추가

```bash
# Ledger Service
cd ../ledger-service
git submodule add https://github.com/your-org/household-budget-common.git common
git commit -m "Add common submodule"
git push

# Transaction Service
cd ../transaction-service
git submodule add https://github.com/your-org/household-budget-common.git common
git commit -m "Add common submodule"
git push

# Analytics Service
cd ../analytics-service
git submodule add https://github.com/your-org/household-budget-common.git common
git commit -m "Add common submodule"
git push

# Notification Service
cd ../notification-service
git submodule add https://github.com/your-org/household-budget-common.git common
git commit -m "Add common submodule"
git push
```

---

## 4. Submodule 포함된 프로젝트 클론

### 4.1. 방법 1: 한 번에 클론 (권장)

```bash
# Submodule까지 함께 클론
git clone --recurse-submodules https://github.com/your-org/auth-service.git

# 또는
git clone --recursive https://github.com/your-org/auth-service.git
```

### 4.2. 방법 2: 나중에 Submodule 초기화

```bash
# 1. 일반 클론
git clone https://github.com/your-org/auth-service.git
cd auth-service

# 이 시점에서 common/ 디렉토리는 비어있음

# 2. Submodule 초기화
git submodule init

# 3. Submodule 내용 가져오기
git submodule update

# 또는 한 번에
git submodule update --init
```

### 4.3. 여러 단계 Submodule이 있는 경우

```bash
# Submodule이 또 다른 submodule을 포함한 경우
git clone --recurse-submodules https://github.com/your-org/auth-service.git

# 또는
git submodule update --init --recursive
```

---

## 5. Submodule 업데이트

### 5.1. 최신 버전으로 업데이트

```bash
cd auth-service

# 방법 1: Submodule 디렉토리에서 직접
cd common
git pull origin main
cd ..

# 방법 2: 루트에서 명령어 사용 (권장)
git submodule update --remote common

# 방법 3: 모든 submodule 업데이트
git submodule update --remote
```

### 5.2. 특정 버전(태그)으로 고정

```bash
cd auth-service/common

# 특정 태그로 체크아웃
git checkout v1.2.0

cd ..
git add common
git commit -m "Update common to v1.2.0"
git push
```

### 5.3. 업데이트 후 확인

```bash
# 현재 submodule 상태 확인
git submodule status

# 출력 예:
# 1a2b3c4d5e6f7g8h9i0j common (v1.2.0)
```

### 5.4. 모든 팀원이 업데이트 반영

```bash
# 다른 개발자가 pull 받은 후
git pull

# Submodule 업데이트 반영
git submodule update --init --recursive
```

---

## 6. Submodule 제거

### 6.1. 완전히 제거하는 방법

```bash
cd auth-service

# 1. Submodule 등록 해제
git submodule deinit -f common

# 2. .git/modules에서 제거
rm -rf .git/modules/common

# 3. 디렉토리 제거
git rm -f common

# 4. 커밋
git commit -m "Remove common submodule"
git push
```

### 6.2. 임시로 비활성화

```bash
# Submodule을 초기화하지 않고 유지
git submodule deinit common

# 다시 활성화
git submodule update --init common
```

---

## 7. 실전 워크플로우

### 7.1. 시나리오 1: 신규 개발자가 프로젝트 시작

```bash
# 1. 프로젝트 클론
git clone --recurse-submodules https://github.com/your-org/auth-service.git
cd auth-service

# 2. Gradle 빌드 확인
./gradlew clean build

# 3. 개발 시작!
```

### 7.2. 시나리오 2: Common 업데이트 후 각 서비스에 반영

```bash
# Common Repository에서 변경 후 릴리즈
cd household-budget-common
# ... 코드 수정 ...
git commit -m "feat(core): add new utility"
git push
git tag -a v1.1.0 -m "Release v1.1.0"
git push origin v1.1.0

# Auth Service에 반영
cd ../auth-service
git submodule update --remote common
cd common
git checkout v1.1.0
cd ..
git add common
git commit -m "Update common to v1.1.0"
git push

# 다른 개발자들은
git pull
git submodule update --init --recursive
```

### 7.3. 시나리오 3: 브랜치 작업

```bash
# Feature 브랜치에서 작업
cd auth-service
git checkout -b feature/new-endpoint

# Common의 특정 버전 사용
cd common
git checkout v1.2.0
cd ..

# 개발 및 커밋
git add common
git commit -m "Use common v1.2.0 for new feature"

# Feature 완료 후 merge
git checkout main
git merge feature/new-endpoint
```

### 7.4. 시나리오 4: Common 개발 중인 기능 테스트

```bash
cd auth-service

# Common의 develop 브랜치로 전환
cd common
git checkout develop
git pull origin develop
cd ..

# 테스트
./gradlew test

# 문제 없으면 릴리즈 요청
# Common에서 develop → main → tag

# 이후 stable 버전으로 되돌리기
cd common
git checkout v1.0.0
cd ..
git add common
git commit -m "Revert to stable common v1.0.0"
```

---

## 8. 문제 해결

### 8.1. "common 디렉토리가 비어있어요"

```bash
# 초기화 안 된 경우
git submodule init
git submodule update

# 또는 한 번에
git submodule update --init
```

### 8.2. "Submodule 업데이트가 안 돼요"

```bash
# 캐시 초기화
git submodule deinit -f common
git submodule update --init common

# 강제 업데이트
cd common
git fetch
git reset --hard origin/main
cd ..
```

### 8.3. "Submodule에 수정사항이 있어요"

```bash
# 상태 확인
cd common
git status

# 변경사항 버리기
git reset --hard HEAD

# 또는 변경사항 저장
git stash
```

### 8.4. "Detached HEAD state에요"

```bash
cd common

# 정상입니다! Submodule은 특정 커밋을 가리킵니다.
# 브랜치로 전환하려면
git checkout main
```

### 8.5. "다른 개발자가 업데이트한 submodule을 못 받아요"

```bash
# Pull 후 반드시 submodule 업데이트
git pull
git submodule update --init --recursive

# 또는 pull과 동시에
git pull --recurse-submodules
```

### 8.6. ".gitmodules 충돌"

```bash
# 충돌 해결 후
git add .gitmodules
git submodule sync
git submodule update --init
```

---

## 9. Best Practices

### 9.1. 버전 관리

```bash
# ✅ 좋은 예: 프로덕션에서는 항상 태그 버전 사용
cd common
git checkout v1.2.0

# ❌ 나쁜 예: 브랜치 사용 (예상치 못한 변경 가능)
cd common
git checkout main
```

### 9.2. 자동화 스크립트

**update-common.sh 생성:**

```bash
#!/bin/bash
set -e

echo "🔄 Updating common submodule..."

# Submodule 디렉토리로 이동
cd common

# 현재 버전 출력
CURRENT_VERSION=$(git describe --tags --abbrev=0 2>/dev/null || echo "No version")
echo "Current version: $CURRENT_VERSION"

# 최신 태그 가져오기
git fetch --tags
LATEST_VERSION=$(git describe --tags `git rev-list --tags --max-count=1`)
echo "Latest version: $LATEST_VERSION"

# 업데이트 여부 확인
if [ "$CURRENT_VERSION" = "$LATEST_VERSION" ]; then
    echo "✅ Already up to date!"
    exit 0
fi

# 사용자 확인
read -p "Update to $LATEST_VERSION? (y/n) " -n 1 -r
echo
if [[ $REPLY =~ ^[Yy]$ ]]; then
    git checkout $LATEST_VERSION
    cd ..
    git add common
    git commit -m "chore: update common to $LATEST_VERSION"
    echo "✅ Updated successfully!"
    echo "📝 Don't forget to push: git push"
else
    echo "❌ Update cancelled"
fi
```

사용법:

```bash
chmod +x update-common.sh
./update-common.sh
```

### 9.3. CI/CD 설정

**.github/workflows/build.yml:**

```yaml
name: Build

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
        with:
          submodules: recursive  # ⭐ 중요!
      
      - name: Set up JDK 17
        uses: actions/setup-java@v3
        with:
          java-version: '17'
          distribution: 'temurin'
      
      - name: Build with Gradle
        run: ./gradlew build
```

### 9.4. .gitignore 설정

```bash
# Submodule 디렉토리는 gitignore하지 않습니다!
# .gitmodules가 관리하기 때문입니다.

# ❌ 이렇게 하지 마세요
# common/

# ✅ 이것만 하세요
.DS_Store
.idea/
build/
```

### 9.5. 팀 컨벤션

```bash
# 1. 항상 태그 버전 사용
git checkout v1.2.0

# 2. 업데이트 전 변경사항 확인
git log --oneline HEAD~5..HEAD

# 3. Breaking Changes 확인
git log --grep="BREAKING"

# 4. 업데이트 후 즉시 테스트
./gradlew test

# 5. 문제 있으면 즉시 롤백
cd common
git checkout v1.1.0
```

---

## 10. 유용한 명령어 모음

### 10.1. 상태 확인

```bash
# Submodule 상태 확인
git submodule status

# 상세 정보
git submodule foreach git status

# 현재 버전 확인
cd common && git describe --tags
```

### 10.2. 일괄 업데이트

```bash
# 모든 submodule 최신화
git submodule update --remote --merge

# 모든 submodule에 명령어 실행
git submodule foreach 'git fetch && git checkout main'
```

### 10.3. 디버깅

```bash
# Submodule 설정 확인
cat .gitmodules

# Git 설정 확인
cat .git/config

# Submodule 상세 로그
git submodule foreach --recursive git log --oneline -5
```

---

## 11. 체크리스트

### 신규 프로젝트 시작 시

- [ ] `git clone --recurse-submodules` 사용
- [ ] Submodule 버전 확인 (`git describe --tags`)
- [ ] 빌드 테스트 (`./gradlew build`)

### Common 업데이트 시

- [ ] CHANGELOG 확인
- [ ] Breaking Changes 확인
- [ ] 로컬에서 테스트
- [ ] 특정 버전 태그로 체크아웃
- [ ] 커밋 및 푸시
- [ ] 팀원들에게 공지

### 문제 발생 시

- [ ] `git submodule status` 확인
- [ ] `git submodule update --init` 실행
- [ ] 캐시 초기화
- [ ] 이전 버전으로 롤백

---

## 12. 자주 묻는 질문

**Q: Submodule vs npm package, 어떤 게 나아요?**

```
Submodule:
✅ Git으로 직접 관리
✅ 소스코드 직접 접근 가능
✅ 버전 고정 쉬움
❌ 설정이 조금 복잡

npm package:
✅ 설정 간단
✅ 의존성 관리 자동화
❌ Private package는 비용 발생
❌ 빌드 필요
```

**Q: Submodule을 수정하고 싶어요**

```bash
# 1. Common repository를 직접 클론
git clone https://github.com/your-org/household-budget-common.git

# 2. 수정 후 push
# ... 수정 ...
git push

# 3. 서비스에서 업데이트
cd auth-service
git submodule update --remote common
```

**Q: 로컬에서만 임시로 수정하고 싶어요**

```bash
cd auth-service/common
# ... 임시 수정 ...

# 테스트 후 되돌리기
git reset --hard HEAD
```

---

## 요약

```bash
# 추가
git submodule add <url> <path>

# 클론
git clone --recurse-submodules <url>

# 업데이트
git submodule update --remote

# 초기화
git submodule update --init --recursive

# 제거
git submodule deinit <path>
git rm <path>
```