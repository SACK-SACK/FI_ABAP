# 💼 FI_ABAP

SAP S/4HANA 기반의 **FI (Financial Accounting)** 모듈 전용 ABAP 프로그램 모음입니다.  
본 저장소는 **회계 전표 처리부터 결산, 채권/채무, 손익계산서, 재무상태표 출력까지** 다양한 재무 업무를 지원합니다.

---

## 📁 디렉토리 구조

각 디렉토리는 ABAP 프로그램 단위로 구성되어 있으며, 해당 프로그램에는 Screen, Include, Function Module, Dictionary 객체들이 포함되어 있습니다.

```
FI_ABAP/
├── sapmzca_fi100          # [FI] 전표 수기 입력
├── zca_fi010              # [FI] 결산 관리
├── zca_fi030              # [FI] 임시전표 검토
├── zca_fi040              # [FI] 전표 조회
├── zca_fi050              # [FI] 채권(AR) 수금처리
├── zca_fi080              # [FI] 재무상태표 출력
├── zca_fi090              # [FI] 손익계산서 출력
├── zca_fi100              # [FI] 계정유형별 상세전표 조회
├── zca_fi110              # [FI] 채무(AP) 지급처리
├── zca_fi130              # [FI] 송장 처리 (MM → FI)
├── zca_fi130_bdc          # MM 연동 BDC 처리
├── zca_fi140              # [FI] 판매 처리 (SD → FI)
├── zca_fi140_bdc          # SD 연동 BDC 처리
├── zca_fi150              # [FI] 폐기 전표 자동 처리 (BG)
├── function_modules/      # Function Modules (ZCA_FI_POST_DOCU 등)
└── README.md              # 프로젝트 설명 문서
```

---

## ⚙️ 주요 프로그램 설명

| 프로그램 | 설명 |
|----------|------|
| **sapmzca_fi100** | 전표 수기 입력 화면 (헤더/아이템 직접 입력) |
| **zca_fi010** | 주차 단위 결산 처리 및 시산표 집계 |
| **zca_fi030** | 임시 전표 승인/반려, 결재 프로세스 |
| **zca_fi040** | 전표 상세 내역 조회 (헤더/아이템) |
| **zca_fi050** | 채권 수금 처리 (AR 원장 반영) |
| **zca_fi080** | 재무상태표 출력 (차변/대변 기준 집계) |
| **zca_fi090** | 손익계산서 출력 (매출/매출원가/당기순이익 등) |
| **zca_fi100** | 계정유형별 전표 상세조회 |
| **zca_fi110** | 채무 지급 처리 (AP 원장 반영) |
| **zca_fi130** | MM 송장 연동 → 임시전표 생성 → 승인 후 실전표 |
| **zca_fi140** | SD 대금관리 연동 → 전표 처리 |
| **zca_fi150** | 재고 폐기 데이터 기반 전표 자동 생성 (배치 실행용) |

---

## 📦 주요 Function Modules (`ZCA_FI_*`)

FI 모듈의 전표 처리, 수금/지급, 결산 계산, 외부 모듈 연동(MM/SD/PP) 등에서 재사용 가능한 기능을 제공합니다.

| Function Name | 설명 |
|---------------|------|
| **ZCA_FI_POST_DOCU** | 실 전표 생성 (FI 전표 헤더/아이템) |
| **ZCA_FI_POST_TEMP_DOCU** | 임시 전표 생성 (승인 전 단계) |
| **ZCA_FI_POST_RECON_CUST** | AR(고객) 원장 반제 처리 |
| **ZCA_FI_POST_RECON_VEN** | AP(벤더) 원장 반제 처리 |
| **ZCA_FI_CALCULATE_PROFIT** | 결산 테이블 기반 손익 계산 |
| **ZCA_FI_TRASH_DOCU** | 폐기 정보 기반 자동 전표 생성 |
| **ZCA_FI_POST_PP_DOCU** | 생산 확정 → 전표 생성 (PP 연계) |
| **ZCA_FI_DELIVERY_POST** | 출고 완료 시 전표 생성 (SD 연계) |
| **ZCA_FI_XBLNR** | 외부참조번호 조회 함수 |
| **ZCA_FI_TBSL** | 전기키 기반 차변/대변 식별 |
| **ZCA_FI_BP_POPUP / _SMALL** | BP 정보 조회 및 팝업 표시 |
| **ZCA_FI_CALC_TVZBR_CUST / VEN** | 고객/벤더 지급조건 계산 |
| **ZCA_FI_DOCU_DETAIL** | 복합 전표 상세 조회 |
| **ZCA_FI_CAL_COST** | 출고/판매 관련 비용 계산 |
| **ZCA_FI_PRO_TYPE** | 판매 유형 분류 |
| **ZCA_FI_BANKCODE** | 계좌 선택 기능 (ListBox/Popup) |
| **ZCA_FI_POST_COST_DOC** | 비용 기반 전표 자동 생성 |
| **ZCA_FI_FIT030** | 전표유형 처리 보조 기능 |

> 대부분의 함수는 공통 `Include LZCA_FI_FG04F01` 또는 `LZCA_FI_FG02F01`를 사용하며, SAP 표준 및 커스텀 테이블을 연동합니다.

---

## 🔧 주요 기술 구성

- **ABAP Module Pool**
- **Function Modules (`ZCA_FI_*`)**
- **Custom Dictionary Tables**
  - `ZCA_FIT040` / `050` : 전표 헤더 / 아이템
  - `ZCA_FIT060` / `070` : 임시전표 헤더 / 아이템
  - `ZCA_FIT080` / `090` / `100` : 은행, 고객, 벤더 원장
  - `ZCA_FIT110` : 결산 데이터
  - `ZCA_DATE` : 주차/날짜 관리 테이블
  - `ZCA_FIT010` / `010T` : G/L 계정 마스터 및 텍스트
- **SmartForms** 출력
- **BDC 처리** (`ZCA_FI130_BDC`, `ZCA_FI140_BDC`)
- **Cross-module 연동** (PP/MM/SD ↔ FI)

---

## 📄 기타 참고 사항

- 모든 전표 처리 로직은 전기키(`ZCA_TBSL`)를 기반으로 차변/대변 구분을 수행합니다.
- 결산 로직은 주차별로 집계되어, 특정 주차의 손익과 재무 상태를 정확히 출력할 수 있습니다.
- 승인 기반 결재 흐름 (`상신 → 승인/반려`)이 구현되어 있어, 실제 전표는 검토 후 생성됩니다.

---

## 👨‍💻 작성자

- [**주현정**](https://github.com/hyun-jung-joo) 
- [**진소정**](https://github.com/jinsojeong)

> SAP SYNC ACADEMY 6기 | ERP FI 통합 개발 프로젝트
