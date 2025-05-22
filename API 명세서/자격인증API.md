## ✅ 사용자 유형별 공공 마이데이터 자격 검증 구조 (실제 운영 기준, 암호화/기관 연계 포함)

---

### 👤 1. 일반 사용자 (운전자)

#### 🔍 검증 목적

운전면허 유효성 및 종류 확인

#### 📤 외부 검증 기관 API

* **기관**: [도로교통공단](https://www.koroad.or.kr/)
* **API 제공 플랫폼**: 공공 마이데이터 API → 도로교통공단 운전면허 정보 연계

#### 📥 요청 데이터 (암호화 적용)

```json
{
  "encrypted_name": "Base64로 인코딩된 AES-256 암호문",
  "birthdate": "900101", // YYMMDD, 평문
  "license_number": "20-12-345678-90",
  "pwdsn": "W1EK7A", // 면허증 하단 암호일련번호
  "consent_nonce": "Base64난수",
  "request_signature": "Base64전자서명값"
}
```

#### 📤 반환 데이터

```json
{
  "valid": true,
  "license_type": "2종 보통",
  "issue_date": "2020-03-10",
  "expiration_date": "2030-03-10",
  "issuing_authority": "도로교통공단",
  "err_code": "0000",
  "err_msg": "정상"
}
```

---

### 🚛 2. 렉카 기사

#### 🔍 검증 목적

* 특수 차량(렉카) 운전 자격 보유 여부
* 면허 종류: **1종 대형** 또는 **특수면허** 등

#### 📤 외부 검증 기관 API

* **기관**: [경찰청 교통민원24(이파인)](https://www.efine.go.kr/)
* **API 제공 플랫폼**: 공공 마이데이터 API → 경찰청 면허정보 시스템 연계

#### 📥 요청 데이터 (암호화 적용)

```json
{
  "encrypted_name": "Base64로 인코딩된 AES-256 암호문",
  "birthdate": "900101",
  "license_number": "18-00-999999-00",
  "pwdsn": "K9X5Q2",
  "consent_nonce": "Base64난수",
  "request_signature": "Base64전자서명값"
}
```

#### 📤 반환 데이터

```json
{
  "valid": true,
  "license_type": "1종 대형",
  "special_conditions": ["렉카 운전 가능"],
  "expiration_date": "2029-12-31",
  "issuing_authority": "경찰청",
  "err_code": "0000",
  "err_msg": "정상"
}
```

---

### ⚖️ 3. 판단자 (사고판단자)

#### 🔍 검증 목적

* 교통사고 감정 자격증, 법률/사정 자격 유무 확인

#### 📤 외부 검증 기관 API

* **기관 예시**:
  * [한국교통안전공단 감정인 등록부](https://www.kotsa.or.kr/)
  * [법원행정처 사법경력관리](https://www.scourt.go.kr/)
  * [행정안전부 자격정보](https://www.mois.go.kr/)

* **API 플랫폼**: 공공 마이데이터 API → 자격증 검증기관 연동

#### 📥 요청 데이터 (암호화 적용)

```json
{
  "encrypted_name": "Base64로 인코딩된 AES-256 암호문",
  "birthdate": "800101",
  "certificate_type": "TRAFFIC_ACCIDENT",
  "certificate_number": "JD-2023-0001",
  "issuer_code": "1341000", // 행정표준기관코드(한국교통안전공단)
  "consent_nonce": "Base64난수",
  "request_signature": "Base64전자서명값"
}
```

#### 📤 반환 데이터

```json
{
  "valid": true,
  "qualification": {
    "type": "교통사고감정인",
    "level": "1급",
    "certification_date": "2021-01-01",
    "expiry_date": "2026-01-01"
  },
  "issuing_organization": "한국교통안전공단",
  "err_code": "0000",
  "err_msg": "정상"
}
```

---

## 🔐 연계 시스템 흐름도 (실제 암호화/기관연계 기준)

```plaintext
[클라이언트]
    ↓ (AES-256 암호화, 전자서명 포함 요청)
[자체 서버]
    ↓ (mTLS, 암호화된 데이터로 공공 마이데이터 허브에 요청)
[공공 마이데이터 허브]
    ↓ (기관별 표준 포맷 변환, 기관코드/자격유형 매핑)
[기관 (도로교통공단 / 경찰청 / 한국교통안전공단 등)]
    ↓ (자격 검증 결과 반환)
[공공 마이데이터 허브]
    ↓ (결과 통합, 오류코드 표준화)
[자체 서버] → 사용자에게 결과 제공
```

---

## ⚠️ 보안 및 운영 참고사항

- **암호화**: 개인정보 필드(AES-256/CBC/PKCS7 + Base64), 전송구간 mTLS
- **전자서명**: 요청 본문 SHA-256 해시 후 기관 인증서로 서명
- **Nonce**: consent_nonce(128bit, Base64)로 재전송 방지
- **오류코드**: 기관별 코드 통합, 예) "1004": "암호일련번호 불일치", "1999": "시스템 오류"
- **기관코드**: 행정표준기관코드 사용(예: 1341000=한국교통안전공단)
- **트래픽 제한**: 초당 10건(기관별 상이)
- **개인정보**: 주민등록번호 전체 직접 전송 불가, 생년월일+암호화명 조합만 허용

---

## 📌 참고 링크

- [공공 마이데이터 업무포털](https://adm.mydata.go.kr/)
- [도로교통공단 면허서비스](https://www.koroad.or.kr/)
- [경찰청 교통민원24](https://www.efine.go.kr/)
- [한국교통안전공단](https://www.kotsa.or.kr/)
- [공공 마이데이터 서비스 가이드 (PDF)](https://adm.mydata.go.kr/images/guide02.pdf)
