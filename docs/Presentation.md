---
marp: true
theme: gaia
paginate: true
backgroundColor: #fff
style: |
  section {
    font-family: 'Arial', sans-serif;
  }
  h1 {
    color: #0066cc;
  }
  h2 {
    color: #333;
  }
---

<!-- _class: lead -->

# HRD-Analyzer
## 로컬 기반 HRD 만족도 원클릭 분석기

**보안 걱정 없는 나만의 데이터 분석 비서**

---

# 1. 프로젝트 개요 (Overview)

### 배경 (Background)
- **보안 이슈**: 사내 교육 만족도 데이터는 민감 정보로, 외부 클라우드 업로드가 제한됨.
- **반복 업무**: 매번 엑셀을 열어 차트를 그리고 보고서를 만드는 비효율 발생.

### 목표 (Objectives)
- **Localhost**: 외부 전송 없이, 100% 내 PC에서 실행.
- **Save Time**: 엑셀 파일만 넣으면 분석 끝.
- **Data Driven**: 감이 아닌 데이터로 교육 효과 입증.

---

# 2. 핵심 기능 (Key Features)

1. **데이터 업로드 (Excel Upload)**
   - 엑셀 파일(`xlsx`) 드래그 앤 드롭 지원.
   - 별도 데이터 가공 없이 원본 그대로 사용 가능.

2. **자동 분석 대시보드 (Dashboard)**
   - 평균 만족도, 강사 평가 점수 자동 계산.
   - NPS (순수 추천 지수) 등 핵심 지표 카드 표시.

3. **인사이트 시각화 (Visualization)**
   - 직급별 만족도 차이 (Box Plot)
   - 이해도 vs 실무 적용도 상관관계 (Scatter Plot)

---

# 3. 기술 스택 (Tech Stack)

비용 0원, 유지보수 용이한 **파이썬 오픈소스** 생태계 활용

- **Language**: Python 3.x
- **Web Framework**: Streamlit (빠르고 직관적인 UI)
- **Data Processing**: Pandas (엑셀 데이터 처리)
- **Visualization**: Plotly Express (인터랙티브 차트)

> "복잡한 웹 개발 지식 없이도, 파이썬 코드만으로 강력한 대시보드를 구축"

---

# 4. 사용자 흐름 (User Flow)

1. **실행**: 터미널에서 `streamlit run app.py` 입력
2. **접속**: 브라우저 창이 열림 (localhost)
3. **업로드**: 분석할 엑셀 파일 투척
4. **확인**: 자동으로 그려진 차트와 지표 확인
5. **보고**: 결과 캡쳐하여 보고서 작성

---

# 5. 개발 로드맵 (Roadmap)

- **Step 1: 환경 구축** (Python, Libs 설치) ✅
- **Step 2: MVP 기능 구현** (파일 업로드, 기본 차트) 🚧
- **Step 3: 고도화** (워드 클라우드, PDF 내보내기) 📅
- **Step 4: 배포** (사내 공유용 exe 파일 변환 등) 🚀

---

<!-- _class: lead -->

# Q & A
## 감사합니다
