# 라이브리 회원 연동 API 명세
``rev.1.201609191443`` ``Tamm``

## 1. Overview

- 라이브리 회원 연동 API는 고객사의 일반 회원 계정을 라이브리 소셜 계정과 연동하는 역할을 합니다.
- 고객사의 회원 정보를 읽어오기 위해 `인증 및 유효성 검사` 과정이 필요하며, 이를 구현해 주셔야 합니다.

![](http://test.livere.co.kr/passport.sample/external.png)

### 준비해 주셔야 하는 내용들

1. 고객사 BI 아이콘
2. 기본 프로필 이미지
3. 로그인, 로그아웃 페이지 URL 혹은 API
4. 개발 사항
	- 클라이언트: 로그인, 로그아웃 시 라이브리 서버로 해당 사용자의 토큰을 전달하는 함수 작성
	- 서버: 해당 사용자의 유효성 검사를 위한 API 작성

* 아이콘 제원에 대한 정보는 별첨된 가이드라인을 참고해주시기 바랍니다.

## 2. 회원 로그인

1. 사용자가 로그인 할 때 서버에서 해시를 생성 하십시오.
	- 생성한 해시는(이하 ``CODE``) 세션 혹은 인증이 유지되고 있을 때 해당 사용자를 식별할 수 있는 고유한 값 입니다.
	- 해당 `CODE`는 사용자 ID를 기반으로 난독화 하시거나 완전한 랜덤 해시를 생성하는 것을 권장 드립니다.
	- 해당 ``CODE``에 사용자 정보를 입력하지 마십시오.(예: ``davidphil0912``)
		- 적절한 ``CODE``의 예 : ``ef55562fc6aa943ce34``
	- 해시를 저장하기 위해 서버 내 자체 캐시를 구현하시거나 외부 캐시를 사용하시는걸 권장 드립니다.

2. 사용자의 로그인 응답을 전송할 때 해당 ``CODE``를 클라이언트로 전달 하십시오.
3. 클라이언트는 서버로부터 응답을 받았다면 다음 API에 해당 `CODE`를 전달 하십시오.

**[Request]**

```
GET /v1/auth/external HTTP/1.1
Host: passport.livere.com
```

위 API를 아래 파라메터들과 함께 클라이언트에서 요청 합니다.

| 키  | 설명 | 필수         |
| :-------- | :-------------------- | :--: |
| code     | 위 과정에서 생성한 사용자 토큰  | O   |
| target   | 전달된 고객사 ID      | O   |
| async    | 비동기 유무(true/false)          | X   |
| type      | `strong` 고정        | O    |
| uid      | 전달된 `uid`        | O    |

* 비동기 요청 시 `async` 값을 `true`로 설정해주시기 바랍니다.

예)

```
curl -v -X GET https://passport.livere.com/v1/auth/external \
 -d 'target=sbs' \m
 -d 'code={CODE}'
 -d 'type=strong'
 -d 'async=true'
 -d 'uid={UID}'
```

- 다음으로 라이브리 서버는 해당 토큰의 유효성을 검사하기 위해 유효성 검사 API를 호출 합니다.
	- 따라서 유효성을 검사할 수 있는 API를 작성해 주셔야 합니다.
		- 예) `https://www.abcdef.com/api/valid`
	- 사용자의 ID는 자체적으로 암호화 혹은 난독화해서 전달해 주시는 것을 권장 드립니다.

- 회원 정보를 안전하게 전달 받기 위해 사전에 비밀 토큰을 전달 드립니다.
- 유효성 검사 API 검증 과정에서 해당 비밀 토큰을 체크해주시기 바랍니다.

예)

```
	if (req.query.token !== STORED_TOKEN) {
		return res.send(401);
	}
```

- 라이브리는 다음과 같이 유효성 검사 API를 호출 합니다.

**[Request]**

```
curl -v -X GET {{VALID_API_URI}} \
 -d 'code={CODE}'
 -d 'token={TOKEN}'
```

- 해당 API의 응답 값은 다음과 같이 설정해 주십시오.

**[Response]**

```
HTTP/1.1 200 OK
{
	status: 200,
	id: {ENCRYPTED_ID}, // ef55562fc6aa943ce34
    username: {USERNAME}, // Angry Neeson
	profile: {PROFILE_IMAGE} // https://.../2015727/10613-fzftvw.jpg
}
```

* 사용자의 프로필 이미지가 없는 경우 생략해 주시기 바랍니다.

해당 과정이 완료되면 회원 연동이 완료됩니다.

## 3. 회원 로그아웃

- 사용자가 로그아웃 할 때 클라이언트에서 다음 API를 비동기로 호출 하십시오.

**[Request]**

```
GET /v1/logout HTTP/1.1
Host: passport.livere.com
```

```
curl -v -X GET https://passport.livere.com/v1/logout
```
