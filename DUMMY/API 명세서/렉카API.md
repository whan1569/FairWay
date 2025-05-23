
### 🚛 **렉카 대기 등록 API** (출근)

| 항목               | 설명                         |
| ---------------- | -------------------------- |
| **URL**          | `POST /api/towing/waiting` |
| **설명**           | 렉카가 출근하여 대기 상태로 등록         |
| **인증**           | 렉카 사용자 인증 토큰               |
| **Request Body** |                            |

```json
{
  "towing_user_id": "uuid_of_towing_user",
  "latitude": 37.567, 
  "longitude": 126.978
}
```

\| **Response** |

```json
{
  "message": "렉카가 대기 상태로 등록되었습니다.",
  "waiting_id": "uuid_of_waiting_state"
}
```

---

### 🚛 **렉카 대기 종료 API** (퇴근)

| 항목               | 설명                           |
| ---------------- | ---------------------------- |
| **URL**          | `DELETE /api/towing/waiting` |
| **설명**           | 렉카가 퇴근하여 대기 상태를 종료           |
| **인증**           | 렉카 사용자 인증 토큰                 |
| **Request Body** | 없음                           |
| **Response**     |                              |

```json
{
  "message": "렉카 대기 상태가 종료되었습니다."
}
```

---

### 🚛 **렉카 대기 상태 조회 API** (현재 대기 중인 렉카 정보)

| 항목           | 설명                                         |
| ------------ | ------------------------------------------ |
| **URL**      | `GET /api/towing/waiting/{towing_user_id}` |
| **설명**       | 렉카 대기 상태 조회 (현재 대기 중인 렉카가 있는지 확인)          |
| **인증**       | 렉카 사용자 인증 토큰                               |
| **Response** |                                            |

```json
{
  "towing_user_id": "uuid_of_towing_user",
  "latitude": 37.567, 
  "longitude": 126.978,
  "created_at": "2025-05-07T10:00:00"
}
```

---

### 🚛 **렉카 대기 목록 조회 API** (전체 대기 중인 렉카 목록)

| 항목          | 설명                        |
| ----------- | ------------------------- |
| **URL**     | `GET /api/towing/waiting` |
| **설명**      | 전체 대기 중인 렉카 목록 조회         |
| **쿼리 파라미터** |                           |

```http
?status=waiting
```

\| **Response** |

```json
[
  {
    "towing_user_id": "uuid_of_towing_user",
    "latitude": 37.567,
    "longitude": 126.978,
    "created_at": "2025-05-07T10:00:00"
  },
  ...
]
```

---

### 🚛 **렉카 위치 갱신 API** (대기 중일 때 위치 변경)

| 항목               | 설명                                           |
| ---------------- | -------------------------------------------- |
| **URL**          | `PATCH /api/towing/waiting/{towing_user_id}` |
| **설명**           | 대기 중인 렉카가 위치를 갱신                             |
| **인증**           | 렉카 사용자 인증 토큰                                 |
| **Request Body** |                                              |

```json
{
  "latitude": 37.567, 
  "longitude": 126.978
}
```

\| **Response** |

```json
{
  "message": "렉카 위치가 갱신되었습니다."
}
```

