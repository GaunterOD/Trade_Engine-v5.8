<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>Gaunter-O-Dimm Engine v5.8</title>
    <style>
        body { font-family: 'Malgun Gothic', 'Apple SD Gothic Neo', sans-serif; line-height: 1.8; color: #333; max-width: 900px; margin: 40px auto; padding: 20px; background-color: #f4f7f6; }
        .container { background: white; padding: 50px; border-radius: 12px; box-shadow: 0 10px 30px rgba(0,0,0,0.15); border-top: 12px solid #2c3e50; }
        h1 { color: #2c3e50; border-bottom: 3px solid #eee; padding-bottom: 15px; text-align: center; font-size: 2.2em; margin-bottom: 10px; }
        .subtitle { text-align: center; font-size: 1.2em; color: #7f8c8d; margin-bottom: 40px; font-weight: bold; }
        h2 { color: #d35400; border-left: 6px solid #d35400; padding-left: 15px; margin-top: 45px; font-size: 1.6em; }
        h3 { color: #2980b9; margin-top: 30px; border-bottom: 1px solid #ddd; display: inline-block; padding-bottom: 5px; }
        .formula-box { background: #f8f9fa; border: 1px solid #e9ecef; padding: 20px; border-radius: 8px; margin: 25px 0; text-align: center; font-size: 1.3em; box-shadow: inset 0 2px 5px rgba(0,0,0,0.02); font-family: 'Courier New', Courier, monospace; font-weight: bold; color: #34495e; }
        .protocol-step { margin-bottom: 15px; padding-left: 10px; }
        .security-notice { background-color: #fdf2f2; border: 2px dashed #e74c3c; color: #c0392b; padding: 25px; margin-top: 50px; border-radius: 8px; }
        .security-notice strong { font-size: 1.1em; }
        ul { list-style-type: square; }
    </style>
</head>
<body>
    <div class="container">
        <h1>🎩 Gaunter-O-Dimm Engine v5.8</h1>
        <div class="subtitle">퀀트 기반 유동성 추적 및 동적 리스크 관리 시스템 명세서</div>

        <h2>1. 설계 철학 (Philosophy)</h2>
        <p>본 시스템은 시장의 비대칭 정보와 유동성 공백을 활용하여, 단순 가격 변동이 아닌 <strong>진성 자본의 유입(Authentic Capital Inflow)</strong>을 추적한다. 전 세계 최대 유동성 공급원인 Binance의 실시간 피드를 나침반으로 삼아, BitMEX 거래소에서 정밀한 비대칭 수익을 창출하도록 설계되었다.</p>

        <h2>2. 고차원 수치 분석 모델 (Mathematical Framework)</h2>

        <h3>2.1. 중력 벡터 분석 (Gravity Vector, GV)</h3>
        <p>다중 시계열(1D, 4H, 1H) 데이터의 지수적 가중치를 결합하여 시장의 중심축을 도출한다.</p>
        <div class="formula-box">
            GV = &Sigma; sgn(P_t - EMA_i,t)
        </div>
        <p>GV가 임계값(&plusmn;2 이상)에 도달할 때만 강한 추세성이 확립된 것으로 간주한다.</p>

        <h3>2.2. 시장 흡수 지수 (Market Absorption Index, MAI)</h3>
        <p>단순 거래량이 아닌, 시장가 주문의 불균형(CVD)을 통계적 표준편차 범위 내에서 재해석한다.</p>
        <div class="formula-box">
            MAI = (&Delta;Vol_net - &mu;) / &sigma;
        </div>
        <p>|MAI| &gt; 2.0&sigma; 구간은 스마트 머니에 의한 매집 또는 투하 구간으로 정의하며, 이는 통계적 유의미성을 가진다.</p>

        <h3>2.3. 유동성 델타 모멘텀 (Liquidity Delta Momentum, LDM)</h3>
        <p>미결제약정(Open Interest)의 백분율 변화율을 추적하여, 파생상품 시장의 에너지 팽창 및 수축 강도를 측정한다.</p>
        <div class="formula-box">
            LDM = (OI_t - OI_prev) / OI_prev &times; 100 (%)
        </div>
        <p>가짜 반등(휩쏘)을 필터링하기 위해 LDM이 특정 임계치(0.1%)를 초과하는지 교차 검증한다.</p>

        <h3>2.4. 절대 자본 유속 (Absolute Capital Flux, ACF)</h3>
        <p>비율(%)의 한계를 보완하기 위해, 특정 윈도우(15분 주기) 내 시장에 유입/유출된 실제 자본의 총량을 절대 수치로 계량화한다.</p>
        <div class="formula-box">
            ACF = OI_t - OI_t-15m
        </div>
        <p>LDM(비율)과 ACF(절대량)의 방향성이 일치할 때만 <strong>'진성 추세(Authentic Trend)'</strong>로 승인한다.</p>

        <h2>3. 실전 운용 프로토콜 (Operation Protocol)</h2>
        <div class="protocol-step">
            <strong>Step 1: 크로마-싱크 시각화 (Chroma-Sync HUD)</strong>: 시스템 터미널 상단에 표시되는 데이터의 양음(초록/빨강) 스펙트럼을 통해 유동성의 유입과 유출을 직관적으로 파악한다.
        </div>
        <div class="protocol-step">
            <strong>Step 2: 신호 무결성 검증 (Signal Integrity)</strong>: 추세 점수(GV)가 정렬되고, 흡수 지수(MAI)가 2.0&sigma;를 돌파하며, LDM과 ACF가 동기화(양수 또는 음수 일치)될 때만 시장 진입을 승인한다.
        </div>
        <div class="protocol-step">
            <strong>Step 3: 적응형 방어막 가동 (Adaptive Boundary Shield)</strong>: 진입 즉시 동적 방어선(Dynamic Stop-Loss)을 활성화하며, 1H ATR 변동성 계수에 따라 최고점/최저점 대비 실시간으로 방어선을 상향/하향 조정한다.
        </div>

        <div class="security-notice">
            <strong>4. 보안 및 기밀 유지 (Strict Confidentiality)</strong><br><br>
            <ul>
                <li>승인되지 않은 환경에서의 포트 포워딩 및 키 노출을 엄격히 금지한다.</li>
            </ul>
        </div>
    </div>
</body>
</html>
