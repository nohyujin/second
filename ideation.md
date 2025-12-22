클라우드나 외부 SaaS 툴을 쓰지 않고, **내 컴퓨터(Local)에서만 안전하게 실행되는 MVP**를 원하시는군요.

한화비전 채용공고의 핵심인 **'교육 효과성 분석'**과 **'데이터 기반 의사결정'** 역량을 개발자스럽게 보여줄 수 있는 **Python 기반의 데이터 분석 대시보드**를 제안합니다.

이 프로젝트는 **"민감한 임직원 교육 만족도 데이터를 외부 서버(클라우드)로 보내지 않고, 내 PC에서 안전하고 빠르게 시각화하여 보고서를 쓴다"**는 강력한 명분이 있습니다.

---

### 🚀 프로젝트명: 로컬 기반 HRD 만족도 원클릭 분석기 (HRD-Analyzer)

엑셀(Excel)로 된 교육 설문 결과를 넣으면, 자동으로 **만족도 차트, 상관관계 분석, 워드 클라우드**를 보여주는 로컬 웹 대시보드입니다.

### 🛠️ 기술 스택 (Local Tech Stack)

비용은 0원이며, Python만 설치되어 있다면 유지보수가 필요 없습니다.

1. **언어: Python**
* 데이터 분석의 표준 언어입니다. "HRD 업무 자동화를 위해 파이썬을 독학했다"는 스토리는 매우 매력적입니다.


2. **UI 프레임워크: Streamlit**
* **선정 이유:** HTML, CSS를 몰라도 Python 코드 몇 줄이면 근사한 웹 대시보드를 만들어줍니다. 로컬(`localhost`)에서 웹사이트처럼 실행됩니다.


3. **데이터 처리: Pandas**
* 엑셀 데이터를 읽고 가공하는 라이브러리입니다.


4. **시각화: Plotly Express**
* 마우스로 상호작용(줌인/줌아웃)이 가능한 예쁜 차트를 그려줍니다.



---

### 💻 구현 코드 (복사해서 바로 사용 가능)

이 코드는 `app.py`라는 파일로 저장하고 실행하면 됩니다.

**전제 조건:** Python 설치 후 터미널에서 아래 명령어 입력
`pip install streamlit pandas plotly openpyxl`

```python
import streamlit as st
import pandas as pd
import plotly.express as px

# 1. 페이지 설정
st.set_page_config(page_title="HRD 교육 결과 분석기", layout="wide")

st.title("📊 HRD 교육 만족도 원클릭 분석 대시보드")
st.markdown("---")
st.markdown("> **기획 의도:** 보안이 중요한 임직원 평가 데이터를 외부 서버 전송 없이 **로컬**에서 안전하게 분석합니다.")

# 2. 파일 업로드 기능
uploaded_file = st.file_uploader("교육 만족도 설문 결과(Excel)를 업로드하세요", type=['xlsx', 'xls'])

if uploaded_file is not None:
    # 엑셀 파일 읽기
    df = pd.read_excel(uploaded_file)
    
    st.success("✅ 파일 업로드 성공! 데이터 분석을 시작합니다.")
    
    # 데이터 미리보기
    with st.expander("📂 원본 데이터 미리보기 (클릭하여 열기)"):
        st.dataframe(df)

    # 3. 주요 지표(KPI) 보여주기
    st.markdown("### 1. 핵심 성과 지표 (KPI)")
    col1, col2, col3 = st.columns(3)
    
    # 예: 컬럼명이 '만족도(5점)', '강사평가(5점)', '추천의향(10점)' 이라고 가정
    # 실제 사용 시에는 본인의 엑셀 컬럼명으로 수정 필요
    try:
        avg_satisfaction = df['만족도'].mean()
        avg_instructor = df['강사평가'].mean()
        nps_score = df[df['추천의향'] >= 9].shape[0] / df.shape[0] * 100 # 약식 NPS
        
        col1.metric("평균 만족도", f"{avg_satisfaction:.2f} / 5.0")
        col2.metric("강사 평가", f"{avg_instructor:.2f} / 5.0")
        col3.metric("NPS (추천 지수)", f"{nps_score:.0f} 점")
    except KeyError:
        st.error("⚠️ 엑셀 파일에 '만족도', '강사평가', '추천의향' 컬럼이 있는지 확인해주세요.")

    st.markdown("---")

    # 4. 시각화 (차트)
    st.markdown("### 2. 상세 분석 차트")
    
    c1, c2 = st.columns(2)
    
    with c1:
        st.subheader("직급별 만족도 분포")
        # '직급' 컬럼이 있다고 가정
        if '직급' in df.columns and '만족도' in df.columns:
            fig1 = px.box(df, x="직급", y="만족도", color="직급", title="직급별 만족도 차이")
            st.plotly_chart(fig1, use_container_width=True)
            
    with c2:
        st.subheader("교육 내용 이해도 vs 실무 적용도")
        # '이해도', '실무적용' 컬럼이 있다고 가정
        if '이해도' in df.columns and '실무적용' in df.columns:
            fig2 = px.scatter(df, x="이해도", y="실무적용", color="직급" if '직급' in df.columns else None, 
                              title="이해도와 실무 적용도의 상관관계")
            st.plotly_chart(fig2, use_container_width=True)

else:
    st.info("👆 위 버튼을 눌러 엑셀 파일을 업로드해주세요.")
    st.markdown("""
    #### 💡 테스트용 엑셀 파일 만드는 법
    A열: 직급 (사원, 대리, 과장...)  
    B열: 만족도 (1~5 숫자)  
    C열: 강사평가 (1~5 숫자)  
    D열: 이해도 (1~5 숫자)  
    E열: 실무적용 (1~5 숫자)  
    F열: 추천의향 (0~10 숫자)
    """)

```

---

### 📜 실행 방법 (How to Run)

1. 위 코드를 메모장에 붙여넣고 `app.py`로 저장합니다.
2. 터미널(CMD)을 열고 파일이 있는 폴더로 이동합니다.
3. 명령어 입력: `streamlit run app.py`
4. 자동으로 브라우저가 열리며 대시보드가 실행됩니다. (인터넷 연결 없어도 됨)

---

### 💼 채용 담당자에게 어필하는 법 (자소서/면접)

이 프로젝트는 단순한 코딩이 아니라 **"HRD 실무 효율화"**에 초점을 맞춰야 합니다.

* **제목:** "반복되는 엑셀 작업을 0초로 단축: Python 기반의 로컬 교육 분석 툴 개발"
* **어필 포인트:**
1. **보안 의식:** "HR 데이터의 민감성을 고려하여, 데이터를 외부로 보내지 않는 로컬 환경(Localhost) 분석 도구를 직접 구축했습니다."
2. **업무 자동화:** "매번 엑셀 차트를 새로 그리는 비효율을 없애고, 파일만 넣으면 보고서용 차트가 나오도록 자동화하여 교육 기획이라는 본질적 업무에 집중할 시간을 확보했습니다."
3. **데이터 리터러시:** "Python과 Pandas를 활용해 단순 평균뿐만 아니라 상관관계 분석 등을 시도하며 데이터 기반의 교육 설계를 지향합니다."



이 방식은 개발자 수준의 깊은 코딩 실력은 필요 없으면서도, **"디지털 툴을 활용해 문제를 해결하는 스마트한 인재"**라는 인상을 강력하게 심어줄 수 있습니다.

혹시 파이썬 설치나 실행 과정이 어렵다면 말씀해 주세요. 더 쉬운 방법을 찾아드리겠습니다.