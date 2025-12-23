# GitHub Actions 워크플로우 완벽 가이드 🚀

> 이 문서는 `.github/workflows/static.yml` 파일의 모든 내용을 대학생도 쉽게 이해할 수 있도록 설명합니다.

## 📚 목차
1. [GitHub Actions란?](#github-actions란)
2. [워크플로우 파일 구조](#워크플로우-파일-구조)
3. [코드 한 줄씩 이해하기](#코드-한-줄씩-이해하기)
4. [실제 동작 과정](#실제-동작-과정)
5. [자주 묻는 질문](#자주-묻는-질문)

---

## GitHub Actions란?

**GitHub Actions**는 GitHub에서 제공하는 **자동화 도구**입니다. 

### 실생활 비유 🏠
- **일반적인 방법**: 집에 도착할 때마다 직접 손으로 전등 스위치를 켜야 함
- **GitHub Actions**: 현관문을 열면 자동으로 전등이 켜지는 스마트홈 시스템

### 프로그래밍에서는?
- **일반적인 방법**: 코드를 수정할 때마다 수동으로 웹사이트에 업로드
- **GitHub Actions**: 코드를 GitHub에 푸시하면 자동으로 웹사이트가 배포됨 ✨

---

## 워크플로우 파일 구조

우리의 워크플로우 파일은 다음과 같은 구조로 되어 있습니다:

```
워크플로우 파일
├── 이름 (name)
├── 언제 실행할지 (on)
├── 권한 설정 (permissions)
├── 동시 실행 제어 (concurrency)
└── 실제 작업 (jobs)
    └── 배포 작업 (deploy)
        ├── 실행 환경 (environment)
        ├── 운영체제 (runs-on)
        └── 단계별 작업 (steps)
```

---

## 코드 한 줄씩 이해하기

### 1️⃣ 워크플로우 이름

```yaml
name: Deploy static content to Pages
```

**의미**: 이 자동화 작업의 이름은 "정적 콘텐츠를 Pages에 배포하기"입니다.

**왜 필요한가요?**
- GitHub Actions 탭에서 이 이름으로 표시됩니다
- 여러 워크플로우가 있을 때 구분하기 쉽습니다

**비유**: 책의 제목과 같습니다 📖

---

### 2️⃣ 트리거 설정 (언제 실행할까?)

```yaml
on:
  # Runs on pushes targeting the default branch
  push:
    branches: ["main"]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:
```

**의미**: 이 워크플로우는 두 가지 상황에서 실행됩니다.

#### 📌 `push` 트리거
```yaml
push:
  branches: ["main"]
```
- **언제**: `main` 브랜치에 코드를 푸시할 때
- **예시**: `git push origin main` 명령어를 실행하면 자동으로 실행됨

#### 📌 `workflow_dispatch` 트리거
```yaml
workflow_dispatch:
```
- **언제**: GitHub 웹사이트에서 수동으로 버튼을 눌렀을 때
- **사용 예시**: 코드 변경 없이 다시 배포하고 싶을 때

**비유**: 
- `push`: 자동문 센서 (사람이 다가가면 자동으로 열림)
- `workflow_dispatch`: 수동 버튼 (직접 눌러서 문 열기)

---

### 3️⃣ 권한 설정

```yaml
# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write
```

**의미**: 이 워크플로우가 할 수 있는 일의 범위를 정합니다.

#### 각 권한 설명

| 권한 | 레벨 | 의미 |
|------|------|------|
| `contents` | `read` | 저장소의 파일을 **읽을 수만** 있음 |
| `pages` | `write` | GitHub Pages에 **배포할 수** 있음 |
| `id-token` | `write` | 인증 토큰을 **생성할 수** 있음 |

**왜 필요한가요?**
- 보안을 위해 필요한 권한만 부여합니다
- 악의적인 코드가 실행되더라도 피해를 최소화합니다

**비유**: 
- 호텔 객실 카드키와 같습니다 🏨
- 자기 방은 열 수 있지만, 다른 방이나 금고는 열 수 없음

---

### 4️⃣ 동시 실행 제어

```yaml
# Allow only one concurrent deployment, skipping runs queued between the run in-progress and latest queued.
# However, do NOT cancel in-progress runs as we want to allow these production deployments to complete.
concurrency:
  group: "pages"
  cancel-in-progress: false
```

**의미**: 배포 작업이 동시에 여러 개 실행되지 않도록 제어합니다.

#### 📌 `group: "pages"`
- 같은 그룹(`pages`)에 속한 작업은 한 번에 하나만 실행됩니다

#### 📌 `cancel-in-progress: false`
- 진행 중인 작업을 취소하지 **않습니다**
- 새로운 작업은 대기열에서 기다립니다

**시나리오 예시**:
1. 첫 번째 푸시: 배포 시작 (진행 중...)
2. 두 번째 푸시: 대기 중...
3. 첫 번째 배포 완료 → 두 번째 배포 시작

**비유**: 
- 은행 창구와 같습니다 🏦
- 한 명씩만 처리하고, 앞 사람이 끝날 때까지 기다림
- 앞 사람 업무를 중단시키지 않음

---

### 5️⃣ 작업 정의 (Jobs)

```yaml
jobs:
  # Single deploy job since we're just deploying
  deploy:
```

**의미**: `deploy`라는 이름의 작업을 정의합니다.

**참고**: 
- 여러 개의 작업(jobs)을 정의할 수 있습니다
- 예: `build`, `test`, `deploy` 등

---

### 6️⃣ 실행 환경 설정

```yaml
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
```

**의미**: 배포 환경을 설정합니다.

#### 📌 `name: github-pages`
- 환경 이름은 `github-pages`입니다
- GitHub에서 이 환경에 대한 배포 기록을 추적합니다

#### 📌 `url: ${{ steps.deployment.outputs.page_url }}`
- 배포된 웹사이트의 URL을 저장합니다
- `${{ }}`: 변수를 참조하는 문법
- `steps.deployment.outputs.page_url`: 배포 단계에서 생성된 URL

**비유**: 
- 배송 추적 시스템과 같습니다 📦
- 어디로 배송되는지(환경), 배송 완료 후 주소(URL)를 기록

---

### 7️⃣ 운영체제 선택

```yaml
    runs-on: ubuntu-latest
```

**의미**: 이 작업은 최신 버전의 Ubuntu Linux에서 실행됩니다.

**선택 가능한 운영체제**:
- `ubuntu-latest`: Ubuntu Linux (가장 일반적)
- `windows-latest`: Windows
- `macos-latest`: macOS

**왜 Ubuntu를 사용하나요?**
- 무료입니다
- 빠르고 안정적입니다
- 대부분의 웹 서버가 Linux를 사용합니다

**비유**: 
- 작업을 수행할 컴퓨터를 빌리는 것과 같습니다 💻
- GitHub이 Ubuntu 컴퓨터를 제공해줍니다

---

### 8️⃣ 단계별 작업 (Steps)

이제 실제로 수행되는 작업들을 하나씩 살펴보겠습니다.

#### Step 1: 코드 가져오기 (Checkout)

```yaml
    steps:
      - name: Checkout
        uses: actions/checkout@v4
```

**의미**: GitHub 저장소의 코드를 작업 환경으로 복사합니다.

**상세 설명**:
- `name: Checkout`: 이 단계의 이름
- `uses: actions/checkout@v4`: GitHub에서 제공하는 공식 액션 사용
  - `actions/checkout`: 액션 이름
  - `@v4`: 버전 4

**비유**: 
- 도서관에서 책을 빌려오는 것과 같습니다 📚
- 작업하려면 먼저 코드를 가져와야 합니다

---

#### Step 2: GitHub Pages 설정 (Setup Pages)

```yaml
      - name: Setup Pages
        uses: actions/configure-pages@v5
```

**의미**: GitHub Pages 배포를 위한 환경을 설정합니다.

**이 단계에서 하는 일**:
- Pages 배포에 필요한 설정 확인
- 메타데이터 생성
- 배포 URL 준비

**비유**: 
- 요리하기 전에 재료와 도구를 준비하는 것과 같습니다 👨‍🍳

---

#### Step 3: 파일 압축 및 업로드 (Upload artifact)

```yaml
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          # Upload entire repository
          path: '.'
```

**의미**: 배포할 파일들을 압축하여 업로드합니다.

**상세 설명**:
- `uses: actions/upload-pages-artifact@v3`: 파일 업로드 액션
- `with`: 이 액션에 전달할 설정
- `path: '.'`: 현재 디렉토리(`.`)의 모든 파일
  - `.`은 "현재 위치"를 의미합니다

**업로드되는 파일**:
- `index.html` (프레젠테이션 페이지)
- `README.md`
- `docs/` 폴더
- 기타 모든 파일

**비유**: 
- 택배 상자에 물건을 담아 포장하는 것과 같습니다 📦
- 모든 파일을 하나의 패키지로 만듭니다

---

#### Step 4: GitHub Pages에 배포 (Deploy)

```yaml
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

**의미**: 압축된 파일을 GitHub Pages 서버에 배포합니다.

**상세 설명**:
- `id: deployment`: 이 단계에 `deployment`라는 ID 부여
  - 다른 곳에서 `steps.deployment`로 참조 가능
  - 앞에서 본 `${{ steps.deployment.outputs.page_url }}`가 여기서 나옴
- `uses: actions/deploy-pages@v4`: 배포 액션 실행

**이 단계에서 하는 일**:
1. 업로드된 파일을 받습니다
2. GitHub Pages 서버에 복사합니다
3. 웹사이트 URL을 생성합니다
4. 배포 완료!

**비유**: 
- 택배 기사가 물건을 배송하는 것과 같습니다 🚚
- 포장된 파일을 웹 서버에 전달합니다

---

## 실제 동작 과정

전체 워크플로우가 실행되는 과정을 순서대로 정리하면:

### 🎬 시작: 코드 푸시

```bash
git add .
git commit -m "Update presentation"
git push origin main
```

### ⚙️ 실행 과정

```
1. 트리거 감지
   └─> main 브랜치에 푸시 감지!

2. 권한 확인
   └─> 필요한 권한이 있는지 확인

3. 동시 실행 체크
   └─> 다른 배포가 진행 중인가?
       ├─> 진행 중이면: 대기
       └─> 없으면: 시작!

4. Ubuntu 컴퓨터 준비
   └─> GitHub이 가상 컴퓨터 제공

5. Step 1: Checkout ✅
   └─> 코드 다운로드 완료

6. Step 2: Setup Pages ✅
   └─> Pages 환경 설정 완료

7. Step 3: Upload artifact ✅
   └─> 파일 압축 및 업로드 완료

8. Step 4: Deploy to GitHub Pages ✅
   └─> 배포 완료!

9. 결과
   └─> https://nohyujin.github.io/second/ 업데이트됨! 🎉
```

### ⏱️ 소요 시간

일반적으로 **10~30초** 정도 걸립니다.

---

## 자주 묻는 질문

### Q1: 왜 `.github/workflows/` 폴더에 파일을 넣나요?

**A**: GitHub이 이 위치를 자동으로 확인하기 때문입니다.
- `.github/workflows/`는 GitHub Actions의 약속된 경로입니다
- 다른 곳에 넣으면 인식하지 못합니다

### Q2: 파일 이름을 `static.yml` 대신 다른 이름으로 해도 되나요?

**A**: 네, 가능합니다!
- 예: `deploy.yml`, `pages.yml`, `my-workflow.yml` 등
- 확장자는 반드시 `.yml` 또는 `.yaml`이어야 합니다

### Q3: `@v4`, `@v5` 같은 버전은 무엇인가요?

**A**: 사용하는 액션의 버전입니다.
- `@v4`: 버전 4
- `@v5`: 버전 5
- 최신 버전을 사용하면 더 많은 기능과 버그 수정이 포함됩니다

### Q4: 배포가 실패하면 어떻게 하나요?

**A**: GitHub Actions 탭에서 로그를 확인할 수 있습니다.

```bash
# 터미널에서 확인
gh run list              # 최근 실행 목록
gh run view <run-id>     # 특정 실행 상세 정보
```

### Q5: 비용이 드나요?

**A**: 공개 저장소는 무료입니다!
- 공개 저장소(Public): 무료 무제한
- 비공개 저장소(Private): 월 2,000분 무료, 이후 유료

### Q6: 다른 브랜치에도 배포하고 싶어요.

**A**: `branches` 부분을 수정하면 됩니다.

```yaml
on:
  push:
    branches: ["main", "develop", "staging"]  # 여러 브랜치 추가
```

### Q7: 특정 파일만 변경되었을 때만 실행하고 싶어요.

**A**: `paths` 필터를 추가하면 됩니다.

```yaml
on:
  push:
    branches: ["main"]
    paths:
      - 'index.html'      # index.html이 변경될 때만
      - 'docs/**'         # docs 폴더 내 파일이 변경될 때만
```

---

## 💡 핵심 정리

### 이 워크플로우가 하는 일
1. ✅ `main` 브랜치에 푸시하면 자동 실행
2. ✅ 코드를 가져와서
3. ✅ GitHub Pages 환경을 설정하고
4. ✅ 파일을 압축해서
5. ✅ 웹사이트로 배포합니다

### 장점
- 🚀 **자동화**: 수동 배포 불필요
- ⚡ **빠름**: 10~30초 만에 배포
- 🔒 **안전**: 권한 관리로 보안 유지
- 📊 **추적 가능**: 모든 배포 기록 확인 가능

### 배운 개념
- **워크플로우(Workflow)**: 자동화 작업의 정의
- **트리거(Trigger)**: 워크플로우를 실행시키는 이벤트
- **작업(Job)**: 워크플로우 내의 큰 단위 작업
- **단계(Step)**: 작업 내의 세부 작업
- **액션(Action)**: 재사용 가능한 작업 단위

---

## 📚 더 공부하고 싶다면

- [GitHub Actions 공식 문서](https://docs.github.com/en/actions)
- [GitHub Pages 문서](https://docs.github.com/en/pages)
- [YAML 문법 배우기](https://yaml.org/)

---

**작성일**: 2025-12-23  
**대상**: 대학생 및 GitHub Actions 초보자  
**난이도**: ⭐⭐ (초급~중급)
