
## 1. 회원가입

### 🔗 `POST /api/auth/register`

### 📥 Request Body

```json
{
  "name": "홍길동",
  "phone": "010-1234-5678",
  "email": "user@example.com",
  "password": "비밀번호"
}
```

> `user_category`는 요청에 포함되더라도 무시되며, 서버에서 기본값 `"user"`로 설정됩니다.

### 📤 Response (201 Created)

```json
{
  "id": "uuid",
  "user_category": "user",
  "created_at": "2025-05-12T15:00:00Z"
}
```

---

## 2. 로그인

### 🔗 `POST /api/auth/login`

### 📥 Request Body

```json
{
  "email": "user@example.com",
  "password": "비밀번호"
}
```

### 📤 Response

```json
{
  "access_token": "JWT_TOKEN",
  "user": {
    "id": "uuid",
    "user_category": "user",
    "name": "홍길동"
  }
}
```

---

## 3. 내 정보 조회

### 🔗 `GET /api/users/me`

> 헤더: `Authorization: Bearer <token>`

### 📤 Response

```json
{
  "id": "uuid",
  "user_category": "judge",
  "name": "홍길동",
  "phone": "010-1234-5678",
  "email": "judge@example.com",
  "created_at": "2025-05-12T15:00:00Z"
}
```

---

## 4. 내 정보 수정

### 🔗 `PUT /api/users/me`

> 전달한 필드만 수정됩니다
> 헤더: `Authorization: Bearer <token>`

### 📥 Request Body

```json
{
  "phone": "010-0000-0000"
}
```

### 📤 Response

```json
{
  "message": "정보가 수정되었습니다."
}
```

---

## 5. 회원 탈퇴

### 🔗 `DELETE /api/users/me`

> 헤더: `Authorization: Bearer <token>`

### 📤 Response

```http
204 No Content
```

---

## 6. 토큰 갱신 (JWT Refresh)

### 🔗 `POST /api/users/token/refresh`

### 📥 Request Headers

```json
{
  "Authorization": "Bearer <refresh_token>",
  "Content-Type": "application/json"
}
```

### 📤 Response (200 OK)

```json
{
  "access_token": "new_access_token_string",
  "refresh_token": "new_refresh_token_string" // 선택사항: 재발급 시
}
```

---

### ❌ ERROR

| status | message                        | 설명                    |
| ------ | ------------------------------ | --------------------- |
| 400    | "Invalid refresh token"        | 토큰 형식이 올바르지 않음        |
| 401    | "Expired refresh token"        | 토큰이 만료됨               |
| 403    | "Refresh token reuse detected" | 재사용 공격 탐지 (이미 사용된 토큰) |
| 500    | "Internal server error"        | 서버 오류                 |

