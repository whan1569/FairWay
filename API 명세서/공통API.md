### 1. 공통 응답 포맷

모든 API 응답은 아래와 같은 공통 포맷을 따릅니다.

#### 성공 응답 예시

```json
{
  "status": 200,
  "message": "Request successful",
  "data": {
    // 실제 데이터 내용
  }
}
```

#### 에러 응답 예시

```json
{
  "status": 400,
  "message": "Bad Request",
  "errors": [
    {
      "field": "username",
      "message": "Username is required"
    }
  ]
}
```

### 2. HTTP 상태 코드

#### 2.1 성공 응답 코드

* **200 OK**
  요청이 성공적으로 처리되었습니다.

#### 2.2 클라이언트 오류 응답 코드

* **400 Bad Request**
  클라이언트 요청에 문제가 있습니다. (예: 잘못된 파라미터, 누락된 필드 등)

* **401 Unauthorized**
  인증이 필요하거나, 인증이 유효하지 않습니다.

* **403 Forbidden**
  요청이 금지되어 있습니다. 권한이 없을 수 있습니다.

* **404 Not Found**
  요청한 리소스를 찾을 수 없습니다.

* **422 Unprocessable Entity**
  요청이 서버에서 처리할 수 없습니다. (주로 잘못된 데이터 형식)

#### 2.3 서버 오류 응답 코드

* **500 Internal Server Error**
  서버에서 알 수 없는 오류가 발생했습니다.

* **502 Bad Gateway**
  서버가 게이트웨이 역할을 하는 동안 문제가 발생했습니다.

* **503 Service Unavailable**
  서버가 일시적으로 사용할 수 없습니다. (예: 서버 과부하)

### 3. 에러 포맷

모든 에러 응답은 아래와 같은 포맷을 따릅니다.

#### 에러 응답 예시

##### 3.1 클라이언트 오류 (400 Bad Request)

```json
{
  "status": 400,
  "message": "Bad Request",
  "errors": [
    {
      "field": "username",
      "message": "Username is required"
    },
    {
      "field": "email",
      "message": "Email format is invalid"
    }
  ]
}
```

##### 3.2 인증 오류 (401 Unauthorized)

```json
{
  "status": 401,
  "message": "Unauthorized",
  "errors": [
    {
      "message": "Authentication token is missing or invalid"
    }
  ]
}
```

##### 3.3 서버 오류 (500 Internal Server Error)

```json
{
  "status": 500,
  "message": "Internal Server Error",
  "errors": [
    {
      "message": "Unexpected server error occurred"
    }
  ]
}
```

### 4. 기타 사항

* **errors 필드**는 배열 형태로 여러 개의 오류 메시지를 반환할 수 있습니다.
* 각 **errors** 객체는 오류가 발생한 필드와 해당 오류 메시지를 포함할 수 있습니다.

