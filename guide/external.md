# 라이브리 회원 연동 가이드

## 회원 연동을 위한 준비

1. 고객사 BI 아이콘 스프라이트 2종(일반, HiDPI)
2. 로그인, 로그아웃 페이지 URL 혹은 API
3. 기본 프로필 이미지(선택)
4. 개발 사항
	- 클라이언트: 로그인, 로그아웃 시 라이브리 서버로 해당 사용자의 토큰을 전달하는 함수 작성
	- 서버: 해당 사용자의 유효성 검사를 위한 API 작성

## 회원 로그인

1. 사용자가 로그인 할 때 서버에서 랜덤한 해시를 생성 하십시오.
	- 생성한 해시는(이하 토큰) 세션 혹은 인증이 유지되고 있을 때 해당 사용자를 식별할 수 있는 고유한 값 입니다.
	- 이는 사용자가 로그인 할 때 마다 변경되게 하는 것을 권장 드립니다.
	- 해당 토큰에 사용자 정보를 입력하지 마십시오.(예: davidphil0912)
		- 적절한 토큰의 예 : ef55562fc6aa943ce34

2. 사용자의 로그인 응답을 전송할 때 해당 토큰을 클라이언트로 전달 하십시오.
3. 클라이언트는 서버로부터 응답을 받았다면 제공되는 라이브리 로그인 API에 해당 토큰을 전송 하십시오.

**[Request]**

```
GET /API_Service/ HTTP/1.1
Host: api-city.livere.com
```	

아래 파라메터들을 GET으로 요청 합니다.

| 키  | 설명 | 필수         |
| :-------- | :-------------------- | :--: |
| command   | externalLogin 고정      | O   |
| token     | 로그인 시 생성한 사용자 토큰  | O   |
| service   | livere 고정             | O   |

예)

```
curl -v -X GET https://api-city.livere.com/API_Service \
 -d 'command=externalLogin' \
 -d 'token={token}' \
 -d 'service=livere'
```

- 다음으로 라이브리 서버는 해당 토큰의 유효성을 검사하기 위해 유효성 검사 API를 호출 합니다.
	- 따라서 유효성을 검사할 수 있는 API를 작성해 주셔야 합니다.
	- 사용자의 ID는 자체적으로 암호화 혹은 난독화해서 전달해 주십시오.

- 라이브리는 다음과 같이 유효성 검사 API를 호출 합니다.

**[Request]**

```
curl -v -X GET {{VALID API URI}} \
 -d 'token={token}'
```

- 해당 API의 응답 값은 다음과 같이 설정해 주십시오.
	
**[Response]**

```
HTTP/1.1 200 OK
{
	status: 200,
	responseType: login,
	id: dfk10fks-dkofko103
	username: David
}
```

해당 과정이 완료되면 사용자는 라이브리에 로그인 됩니다.

## 회원 로그아웃

1. 사용자가 로그아웃 할 때 로그인 과정에서 생성한 토큰을 클라이언트로 전달 하십시오.
2. 클라이언트는 서버로부터 응답을 받았다면 제공되는 라이브리 로그아웃 API에 해당 토큰을 전송 하십시오.

**[Request]**

```
GET /API_Service/ HTTP/1.1
Host: api-city.livere.com
```	

아래 파라메터들을 GET으로 요청 합니다.

| 키  | 설명 | 필수         |
| :-------- | :-------------------- | :--: |
| command   | externalLogout 고정      | O   |
| token     | 로그인 시 생성한 사용자 토큰  | O   |
| service   | livere 고정             | O   |

예)

```
curl -v -X GET https://api-city.livere.com/API_Service \
 -d 'command=externalLogout' \
 -d 'token={token}' \
 -d 'service=livere'
```

- 로그인 과정과 마찬가지로 라이브리 서버는 해당 토큰의 유효성을 검사하기 위해 귀하의 API를 호출할 것 입니다.

**[Request]**

```
curl -v -X GET {{VALID API URI}} \
 -d 'token={token}'
```

- 해당 API의 응답 값은 다음과 같이 설정해 주십시오.
	
```
HTTP STATUS: 200
{
	status: 200,
	responseType: logout
}
```
해당 과정이 완료되면 사용자는 라이브리에 로그아웃이 완료 됩니다.