

## 🚗 사고 본체 API 명세서

---

### 1. 사고 등록

**\[POST]** `/api/accidents`

#### 🔐 Headers

```
Authorization: Bearer <token>
Content-Type: application/json
```

#### 📥 Request Body

```json
{
  "accident_type_id": "uuid",
  "description": "신호 위반으로 인한 추돌 사고",
  "accident_time": "2025-05-06T14:30:00Z",
  "location": "서울특별시 강남구 역삼동",
  "judge_id": "uuid (optional)"
}
```

#### ✅ Response: 201 Created

```json
{
  "id": "accident-uuid",
  "created_at": "2025-05-06T14:32:00Z"
}
```

---

### 2. 사고 목록 조회

**\[GET]** `/api/accidents`

#### 🔐 Headers

```
Authorization: Bearer <token>
```

#### 🔍 Query Parameters (선택)

| 이름        | 설명         |
| --------- | ---------- |
| user\_id  | 사용자 ID로 필터 |
| judge\_id | 판단자 ID로 필터 |

#### ✅ Response: 200 OK

```json
[
  {
    "id": "accident-uuid",
    "accident_time": "2025-05-06T14:30:00Z",
    "location": "서울특별시 강남구",
    "description": "신호 위반 사고",
    "fault_percentage": 40,
    "insurance_status": "처리 중"
  },
  ...
]
```

---

### 3. 사고 정보 수정

**\[PUT]** `/api/accidents/{accident_id}`

#### 🔐 Headers

```
Authorization: Bearer <token>
Content-Type: application/json
```

#### 📥 Request Body (필드 선택적으로 전달 가능)

```json
{
  "description": "수정된 사고 설명",
  "location": "서울특별시 서초구",
  "fault_percentage": 60,
  "insurance_status": "처리 완료"
}
```

#### ✅ Response: 200 OK

```json
{
  "message": "Accident updated successfully"
}
```

---

### 4. 사고 삭제 (선택적 기능)

**\[DELETE]** `/api/accidents/{accident_id}`

> 보통은 soft delete (예: is\_deleted 플래그)로 처리 권장

#### 🔐 Headers

```
Authorization: Bearer <token>
```

#### ✅ Response: 204 No Content


## 📸 사고 사진 API 명세서

---

### 1. 사고 사진 등록

**\[POST]** `/api/accidents/{accident_id}/photos`

#### 🔐 Headers

```
Authorization: Bearer <token>
Content-Type: multipart/form-data
```

#### 📥 Request Form Data

| 필드명                 | 타입            | 설명          |
| ------------------- | ------------- | ----------- |
| photo               | file          | 업로드할 이미지 파일 |
| photo\_category\_id | string (uuid) | 사진 카테고리 ID  |
| description         | string        | 사진 설명 (선택)  |

#### ✅ Response: 201 Created

```json
{
  "id": "photo-uuid",
  "photo_url": "https://cdn.example.com/photos/photo.jpg",
  "saved_at": "2025-05-06T15:00:00Z"
}
```

---

### 2. 사고 사진 조회

**\[GET]** `/api/accidents/{accident_id}/photos`

#### 🔐 Headers

```
Authorization: Bearer <token>
```

#### ✅ Response: 200 OK

```json
[
  {
    "id": "photo-uuid-1",
    "photo_url": "https://cdn.example.com/photos/1.jpg",
    "description": "피해 차량 정면",
    "photo_category": {
      "id": "category-uuid",
      "name": "차량 정면"
    },
    "saved_at": "2025-05-06T15:00:00Z"
  },
  ...
]
```

---

### 3. 사고 사진 삭제

**\[DELETE]** `/api/accidents/photos/{photo_id}`

#### 🔐 Headers

```
Authorization: Bearer <token>
```

#### ✅ Response: 204 No Content


## 🎥 사고 영상 API 명세서

---

### 1. 사고 영상 등록

**\[POST]** `/api/accidents/{accident_id}/videos`

#### 🔐 Headers

```
Authorization: Bearer <token>
Content-Type: multipart/form-data
```

#### 📥 Request Form Data

| 필드명                 | 타입            | 설명         |
| ------------------- | ------------- | ---------- |
| video               | file          | 업로드할 영상 파일 |
| video\_category\_id | string (uuid) | 영상 카테고리 ID |
| description         | string        | 영상 설명 (선택) |

#### ✅ Response: 201 Created

```json
{
  "id": "video-uuid",
  "video_url": "https://cdn.example.com/videos/video.mp4",
  "saved_at": "2025-05-06T15:10:00Z"
}
```

---

### 2. 사고 영상 조회

**\[GET]** `/api/accidents/{accident_id}/videos`

#### 🔐 Headers

```
Authorization: Bearer <token>
```

#### ✅ Response: 200 OK

```json
[
  {
    "id": "video-uuid-1",
    "video_url": "https://cdn.example.com/videos/1.mp4",
    "video_category": {
      "id": "category-uuid",
      "name": "대시캠 전방"
    },
    "saved_at": "2025-05-06T15:10:00Z"
  },
  ...
]
```

---

### 3. 사고 영상 삭제

**\[DELETE]** `/api/accidents/videos/{video_id}`

#### 🔐 Headers

```
Authorization: Bearer <token>
```

#### ✅ Response: 204 No Content

## 🧑‍💬 사고 목격자 API 명세서

---

### 1. 사고 목격자 등록

**\[POST]** `/api/accidents/{accident_id}/witnesses`

#### 🔐 Headers

```
Authorization: Bearer <token>
Content-Type: application/json
```

#### 📥 Request Body

```json
{
  "name": "홍길동",
  "contact_email": "witness@example.com",
  "contact_phone": "010-1234-5678",
  "statement": "정면에서 신호 위반 차량이 추돌하는 걸 목격했습니다."
}
```

#### ✅ Response: 201 Created

```json
{
  "id": "witness-uuid",
  "created_at": "2025-05-06T15:20:00Z"
}
```

---

### 2. 사고 목격자 조회

**\[GET]** `/api/accidents/{accident_id}/witnesses`

#### 🔐 Headers

```
Authorization: Bearer <token>
```

#### ✅ Response: 200 OK

```json
[
  {
    "id": "witness-uuid-1",
    "name": "홍길동",
    "contact_email": "witness@example.com",
    "contact_phone": "010-1234-5678",
    "statement": "신호 위반 차량이 먼저 진입했습니다.",
    "created_at": "2025-05-06T15:20:00Z"
  },
  ...
]
```

---

### 3. 사고 목격자 수정

**\[PUT]** `/api/accidents/witnesses/{witness_id}`

#### 🔐 Headers

```
Authorization: Bearer <token>
Content-Type: application/json
```

#### 📥 Request Body (선택 필드 수정 가능)

```json
{
  "contact_email": "witness_new@example.com",
  "statement": "수정된 진술 내용입니다."
}
```

#### ✅ Response: 200 OK

```json
{
  "message": "Witness updated successfully"
}
```

---

### 4. 사고 목격자 삭제

**\[DELETE]** `/api/accidents/witnesses/{witness_id}`

#### 🔐 Headers

```
Authorization: Bearer <token>
```

#### ✅ Response: 204 No Content
