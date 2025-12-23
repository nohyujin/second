# 🚀 HRD-Analyzer (로컬 기반 HRD 교육 만족도 분석기)

**HRD-Analyzer**는 외부 서버(클라우드) 전송 없이 **내 컴퓨터(Localhost)**에서 안전하게 실행되는 교육 만족도 분석 대시보드입니다. 엑셀 파일을 업로드하는 즉시 만족도 지표와 시각화 차트를 자동으로 생성합니다.

## 📋 주요 기능
- **데이터 보안 100%**: 인터넷 연결 없이 로컬 환경에서만 데이터가 처리됩니다.
- **원클릭 분석**: 엑셀 파일(`xlsx`)을 드래그 앤 드롭하면 즉시 분석 결과가 나옵니다.
- **자동 시각화**:
    - 평균 만족도, 강사 평가, NPS(순추천지수) KPI 카드
    - 직급별 만족도 분포 (Box Plot)
    - 이해도 vs 실무 적용도 상관관계 분석 (Scatter Plot)

## 🛠 기술 스택
- **언어**: Python 3.x
- **UI 프레임워크**: Streamlit
- **데이터 분석**: Pandas, OpenPyXL
- **시각화**: Plotly Express

## 📂 문서 (Documentation)
이 프로젝트의 상세 문서는 `docs/` 폴더에서 확인할 수 있습니다.
- [**기획안 (Ideation)**](ideation.md): 프로젝트 초기 구상 및 기획 의도
- [**요구사항 정의서 (PRD)**](docs/PRD.md): 상세 기능 명세 및 개발 로드맵
- [**개발 태스크 (Task)**](docs/Task.md): 개발 진행 상황 체크리스트

## 🚀 실행 방법 (Getting Started)
1. 저장소를 클론(Clone)합니다.
   ```bash
   git clone https://github.com/nohyujin/second.git
   ```
2. 필수 라이브러리를 설치합니다.
   ```bash
   pip install streamlit pandas plotly openpyxl
   ```
3. 앱을 실행합니다.
   ```bash
   streamlit run app.py
   ```
