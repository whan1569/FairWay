### 손해사정사/변호사 자격증 진위 확인: **웹 스크래핑 방식**

---

## 1️⃣ **손해사정사 (금융감독원 조회)**  
### **요청 구조**
| 항목 | 내용 |
|------|------|
| **URL** | `https://www.fss.or.kr/fss/job/gyerythaMng/list.do?menuNo=200610` |
| **요청 방식** | `POST` |
| **필수 입력값** | `name`(이름), `regNo`(등록번호) |
| **결과 형태** | HTML 테이블 (이름, 등록번호, 종목구분, 등록일) |

### **자동화 절차**
1. **POST 요청 전송**  
   ```python
   import requests
   from bs4 import BeautifulSoup

   url = "https://www.fss.or.kr/fss/job/gyerythaMng/list.do?menuNo=200610"
   data = {"name": "홍길동", "regNo": "VE00003001"}  # 예시 등록번호

   response = requests.post(url, data=data)
   soup = BeautifulSoup(response.text, "html.parser")
   ```

2. **결과 파싱**  
   ```python
   for row in soup.select("table tr"):
       columns = [td.get_text(strip=True) for td in row.find_all("td")]
       if columns:
           print(f"등록번호: {columns[0]}, 이름: {columns[2]}, 종목: {columns[3]}")
   ```

---

## 2️⃣ **변호사 (대한변호사협회 조회)**  
### **요청 구조**
| 항목 | 내용 |
|------|------|
| **URL** | `https://lawyersearch.koreanbar.or.kr/search` |
| **요청 방식** | `POST` |
| **필수 입력값** | `name`(이름), `regNo`(등록번호) |
| **결과 형태** | HTML 테이블 (이름, 사무실, 연락처) |

### **자동화 절차**
1. **POST 요청 전송**  
   ```python
   url = "https://lawyersearch.koreanbar.or.kr/search"
   data = {"name": "홍길동", "regNo": "2022-12345"}  # 예시 등록번호

   response = requests.post(url, data=data)
   soup = BeautifulSoup(response.text, "html.parser")
   ```

2. **결과 파싱**  
   ```python
   for row in soup.select("table tr"):
       columns = [td.get_text(strip=True) for td in row.find_all("td")]
       if columns:
           print(f"이름: {columns[0]}, 사무실: {columns[1]}, 전화: {columns[2]}")
   ```

---

## ⚠️ **주의사항**
- **법적 검토 필요**: 웹 스크래핑은 기관의 이용약관 위반 가능성이 있음.
- **유지보수**: 웹사이트 구조 변경 시 코드 수정 필수.
- **트래픽 제한**: 과도한 요청 시 IP 차단될 수 있음.
- **캡차 대응**: 변호사 조회 시 신분증 진위확인은 별도 프로그램 필요 ([검색결과 5]).

---

## ✅ **결론**
- **손해사정사**: 금융감독원 웹 폼 → `POST` 요청 → 테이블 파싱.
- **변호사**: 대한변협 검색 → `POST` 요청 → 테이블 파싱.
- **기술적 난이도**: 초보자도 구현 가능 (Python + Requests/BeautifulSoup).  
- **실제 적용 시**: 반드시 기관 정책 및 법적 리스크 확인 필요!
