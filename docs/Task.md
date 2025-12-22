# HRD-Analyzer 개발 태스크

이 문서는 [PRD.md](PRD.md)를 기반으로 **HRD-Analyzer** 프로젝트를 완성하기 위한 개발 항목을 체크리스트로 정리한 것입니다.

## 1. 환경 설정 (Environment Setup)
- [ ] Python 설치 및 버전 확인
- [ ] 필수 라이브러리 설치 (`streamlit`, `pandas`, `plotly`, `openpyxl`)
- [ ] 프로젝트 디렉토리 구조 생성 (`app.py`, `data/`)

## 2. 핵심 기능 개발 (Core Features)
- [ ] **Streamlit 기본 앱 생성**
    - [ ] `app.py` 생성 및 페이지 타이틀/레이아웃 설정
    - [ ] 사이드바 및 메인 영역 구성
- [ ] **데이터 업로드 기능**
    - [ ] 엑셀 파일 업로더 위젯 구현 (`st.file_uploader`)
    - [ ] 업로드된 파일 읽기 및 데이터프레임 변환 (`pd.read_excel`)
    - [ ] 예외 처리 (파일 없을 때 빈 화면 처리 등)
- [ ] **데이터 전처리**
    - [ ] 필수 컬럼 확인 ('만족도', '강사평가' 등) 로직 구현

## 3. 대시보드 및 KPI 구현 (Dashboard & KPIs)
- [ ] **KPI 계산 로직**
    - [ ] 평균 만족도 계산
    - [ ] 평균 강사 평가 점수 계산
    - [ ] NPS (순추천지수) 계산 (추천의향 9점 이상 비율 등)
- [ ] **KPI UI 표시**
    - [ ] `st.metric` 활용하여 주요 지표 3개 표시
- [ ] **데이터 미리보기**
    - [ ] `st.expander`를 활용한 원본 데이터 테이블 표시

## 4. 데이터 시각화 (Visualization)
- [ ] **직급별 만족도 분석**
    - [ ] Plotly Box Plot 구현 (X: 직급, Y: 만족도)
    - [ ] 차트 레이아웃 조정 및 표시
- [ ] **상관관계 분석**
    - [ ] Plotly Scatter Plot 구현 (X: 이해도, Y: 실무적용)
    - [ ] 컬러 코딩 (직급별 구분 등)
- [ ] **워드 클라우드 (Optional)**
    - [ ] 텍스트 데이터 존재 시 워드 클라우드 생성 로직

## 5. 배포 및 마무리 (Deployment & Finalize)
- [ ] 로컬 실행 테스트 (`streamlit run app.py`)
- [ ] README.md 작성 (실행 가이드 및 사용법)
- [ ] 코드 정리 및 주석 추가
