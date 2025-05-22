
### 🧠 **리포트 생성 API**

| 항목               | 설명                   |
| ---------------- | -------------------- |
| **URL**          | `POST /api/reports`  |
| **설명**           | 판단자가 사고에 대한 리포트를 생성함 |
| **인증**           | 판단자 인증 토큰            |
| **Request Body** |                      |

```json
{
  "accidentId": "uuid-accident-id",
  "reportCategoryId": "uuid-report-category-id",
  "pdfUrl": "https://cdn.example.com/reports/abc123.pdf",
  "pdfHash": "d41d8cd98f00b204e9800998ecf8427e"
}
```

\| **Response**     |                                   |

```json
{
  "reportId": "uuid-report-id",
  "message": "리포트가 성공적으로 생성되었습니다."
}
```

---

### 🧠 **리포트 상세 조회 API**

| 항목           | 설명                            |
| ------------ | ----------------------------- |
| **URL**      | `GET /api/reports/{reportId}` |
| **설명**       | 특정 리포트의 상세 정보를 조회함            |
| **인증**       | 판단자 또는 관리자 인증 토큰              |
| **Response** |                               |

```json
{
  "reportId": "uuid",
  "accidentId": "uuid",
  "judgeId": "uuid",
  "reportCategory": {
    "id": "uuid",
    "name": "LEVEL_1",
    "description": "간단 감정 보고서"
  },
  "pdfUrl": "https://cdn.example.com/reports/abc123.pdf",
  "pdfHash": "d41d8cd98f00b204e9800998ecf8427e",
  "reportDate": "2025-05-09T13:45:00Z",
  "createdAt": "2025-05-09T13:45:00Z",
  "updatedAt": "2025-05-09T14:10:00Z"
}
```

---

### 🧠 **리포트 수정 API**

| 항목               | 설명                            |
| ---------------- | ----------------------------- |
| **URL**          | `PUT /api/reports/{reportId}` |
| **설명**           | 판단자가 리포트를 수정함                 |
| **인증**           | 판단자 본인 또는 관리자 인증 토큰           |
| **Request Body** |                               |

```json
{
  "reportCategoryId": "uuid-report-category-id",
  "pdfUrl": "https://cdn.example.com/reports/abc123_v2.pdf",
  "pdfHash": "updatedhashvalue"
}
```

\| **Response**     |                                |

```json
{
  "message": "리포트가 성공적으로 수정되었습니다."
}
```

---

### 🧠 **사고별 리포트 목록 조회 API**

| 항목           | 설명                                        |
| ------------ | ----------------------------------------- |
| **URL**      | `GET /api/accidents/{accidentId}/reports` |
| **설명**       | 해당 사고에 등록된 리포트 목록 조회                      |
| **인증**       | 사고 작성자, 판단자, 관리자 인증 토큰                    |
| **쿼리 파라미터**  | `reportCategoryId` (선택) - 유형 필터링 가능       |
| **Response** |                                           |

```json
[
  {
    "reportId": "uuid",
    "reportCategory": {
      "id": "uuid",
      "name": "LEVEL_2",
      "description": "정밀 리포트"
    },
    "pdfUrl": "https://cdn.example.com/reports/abc123.pdf",
    "createdAt": "2025-05-09T13:45:00Z"
  }
]
```

---

### 🧠 **리포트 삭제 API**

| 항목           | 설명                               |
| ------------ | -------------------------------- |
| **URL**      | `DELETE /api/reports/{reportId}` |
| **설명**       | 판단자가 자신의 리포트를 삭제함                |
| **인증**       | 판단자 본인 또는 관리자 인증 토큰              |
| **Response** |                                  |

```json
{
  "message": "리포트가 삭제되었습니다."
}
```

