# 라이브리 소셜 로그인 API 명세 
``rev.1.201510051400`` ``Tamm``

## 1. 라이브리 소셜 로그인 인증 프로세스
- 전체적인 라이브리 소셜 로그인 인증 프로세스는 다음과 같습니다.
- 해당 프로세는 ``1. 로그인 / 추가 회원연동 API`` 및 ``3. 회원 연동 해제 API`` 에서 사용 됩니다.

![소셜 로그인 프로세스](http://test.livere.co.kr/passport.sample/sociallogin.png)

## 2. API 명세

### 1. 로그인 / 추가 회원연동 API

1. **로그인 / 추가 회원연동 요청**
	
	**[Request]**
	
	```
	GET /v1/auth/{{SOCIAL_MEDIA_NAME}} HTTP/1.1
	Host: passport.livere.com
	```	
	
	**[Request 예]**
	
	```
	curl -v -X GET https://passport.livere.com/v1/auth/facebook
	 -d 'redirectUrl=https://www.shilladfs.com/../passport/redirect.html'
	```
	* 아래 파라메터들을 포함해 클라이언트에서 ``window.open`` 함수를 통해 요청 합니다.
	
	| 키  | 설명 | 필수         |
	| :-------- | :-------------------- | :--: |
	| redirectUrl   | 로그인 후 리다이렉트 할 신라면세점 페이지(전달된 양식)      | O   |
	
	* ``redirectUrl`` 파라메터의 경우 전달된 HTML 페이지를 신라면세점 쪽 도메인에 업로드 해주시면 됩니다.(``1. 라이브리 소셜 로그인 인증 프로세스`` 참고)

2. **로그인 후 회원 정보 CODE 전달**

	* 위 프로세스에서 파라메터로 전달된 리다이렉트 페이지에서 상위 부모 페이지의 함수를 호출합니다. 따라서 로그인 API를 요청한 페이지에 ``회원 정보 CODE``를 전달 받을 함수를 작성해 주셔서 합니다.
	
	**[호출 예(라이브리 쪽 작업)]**
	
	```
	function sendAuthCode(code) {
		return window.opener.setAuthCode(code);
	}
	```
	
	**[함수 작성 예(신라면세점 쪽 작업)]**
	
	```
	function setAuthCode(code) {
		// 신라면세점 쪽 인증 프로세스
		$.ajax({
			url: ...,
			data: {
				code: code
			}
		})...
	}
	```
	
3. **회원 정보 요청**

	* 클라이언트로 전달된 ``CODE``를 통해 현재 라이브리에 로그인 한 사용자의 정보를 받아옵니다.
	* **반드시 서버에서 요청을 진행해 주시기 바랍니다.**
	
	**[Request]**

	```
	GET /v1/valid HTTP/1.1
	Host: passport.livere.com
	```	
	
	아래 파라메터들을 GET으로 요청 합니다.
	
	| 키  | 설명 | 필수         |
	| :-------- | :-------------------- | :--: |
	| token   | 사전에 전달된 비밀 토큰      | O   |
	| code     | 회원 정보 요청 CODE  | O   |
	
	**[Request 예]**
	
	```
	curl -v -X GET https://passport.livere.com/v1/auth/facebook
	 -d 'token={{secretToken}}' \
	 -d 'code={{code}}'
	```
	
	* 정상적으로 인증이 완료되면 다음과 같이 현재 로그인 되어 있는 회원 정보를 받을 수 있습니다. 
	
	**[Response]**
	
	```
	HTTP/1.1 200 OK
	{
        "facebook": {
            "memberSeq": 14,
            "memberId": "100001468893891",
            "memberName": "Tae Min Kwun",
            "memberDomain": "facebook",
            "memberUrl": "http://www.facebook.com/100001468893891",
            "email": null,
            "gender": 0,
            "birthday": null,
            "age": 0,
            "memberGroupSeq": 1090,
            "memberIcon": "https://graph.facebook.com/100001468893891/picture",
            "regdate": "2015-09-08T08:20:06.000Z"
        },
        "kakao": {
            "memberSeq": 40,
            "memberId": "51314296",
            "memberName": "Tamm",
            "memberDomain": "kakao",
            "memberUrl": "https://story.kakao.com/_KP5Q35",
            "email": null,
            "gender": 0,
            "birthday": null,
            "age": 0,
            "memberGroupSeq": 1090,
            "memberIcon": "http://mud-kage.kakao.co.kr/14/dn/btqcf6VqmlN/JPl6zDNqbb7LzZ3cC5oWc0/o.jpg",
            "regdate": "2015-09-21T01:28:09.000Z"
        },
        "memberGroupSeq": 1090,
        "primary": "facebook"
	}
	```

### 2. 로그아웃 API

**[Request]**

```
GET /v1/logout HTTP/1.1
Host: passport.livere.com
```	

**[Request 예]**

```
curl -v -X GET https://passport.livere.com/v1/logout
```

* 로그인, 로그아웃 API의 경우 [테스트 페이지](http://test.livere.co.kr/passport.sample) 소스 코드를 참고 하시기 바랍니다.

### 3. 회원 연동 해제 API

1. **연동 해제 요청**

	* 연동 해제 API는 로그인 프로세스와 동일한 프로세스로 진행 됩니다.
	
	**[Request]**
	
	```
	GET /v1/auth/release/{{SOCIAL_MEDIA_NAME}} HTTP/1.1
	Host: passport.livere.com
	```	
	
	**[Request 예]**
	
	```
	curl -v -X GET https://passport.livere.com/v1/auth/release/facebook
	```
	
	**[Response]**
	
	```
	HTTP/1.1 200 OK
	{
        "code": "7f7761cffd492153cbad728c",
	}
	```
	
2. **회원 정보 요청**

	* 전달된 ``CODE``를 통해 현재 라이브리에 로그인 한 사용자의 정보를 갱신합니다.
	* ``로그인 API`` 문서의 ``1-3 회원 정보 요청`` 부분과 동일한 API 입니다.
	* **반드시 서버에서 요청을 진행해 주시기 바랍니다.**
	
	**[Request]**

	```
	GET /v1/valid HTTP/1.1
	Host: passport.livere.com
	```	
	
	클라이언트에서 아래 파라메터들을 ajax로(GET) 요청 합니다.
	
	| 키  | 설명 | 필수         |
	| :-------- | :-------------------- | :--: |
	| token   | 사전에 전달된 비밀 토큰      | O   |
	| code     | 회원 정보 요청 CODE  | O   |
	
	**[Request 예]**
	
	```
	curl -v -X GET https://passport.livere.com/v1/auth/facebook
	 -d 'token={{secretToken}}' \
	 -d 'code={{code}}'
	```
	
	* 정상적으로 인증이 완료되면 다음과 같이 현재 로그인 되어 있는 회원 정보를 받을 수 있습니다. 
	
	**[Response]**
	
	```
	HTTP/1.1 200 OK
	{
        "facebook": {
            "memberSeq": 14,
            "memberId": "100001468893891",
            "memberName": "Tae Min Kwun",
            "memberDomain": "facebook",
            "memberUrl": "http://www.facebook.com/100001468893891",
            "email": null,
            "gender": 0,
            "birthday": null,
            "age": 0,
            "memberGroupSeq": 1090,
            "memberIcon": "https://graph.facebook.com/100001468893891/picture",
            "regdate": "2015-09-08T08:20:06.000Z"
        },
        "kakao": {
            "memberSeq": 40,
            "memberId": "51314296",
            "memberName": "Tamm",
            "memberDomain": "kakao",
            "memberUrl": "https://story.kakao.com/_KP5Q35",
            "email": null,
            "gender": 0,
            "birthday": null,
            "age": 0,
            "memberGroupSeq": 1090,
            "memberIcon": "http://mud-kage.kakao.co.kr/14/dn/btqcf6VqmlN/JPl6zDNqbb7LzZ3cC5oWc0/o.jpg",
            "regdate": "2015-09-21T01:28:09.000Z"
        },
        "memberGroupSeq": 1090,
        "primary": "facebook"
	}
	```
	
## 3. 소셜 미디어 파라메터

* API 요청 시 파라메터로 들어가는 ``소셜 미디어 파라메터(SOCIAL_MEDIA_NAME)`` 정보입니다.

| 이름  | 파라메터 | 테스트 가능 여부         |
| :-------- | :-------------------- | :--: |
| 트위터   | twitter      | O   |
| 페이스북     | facebook  | O   |
| 구글     | google_plus  | O   |
| 링크드인     | linkedin  | O   |
| 네이버     | naver  | O   |
| 카카오톡     | kakao  | O   |
| 웨이보     | weibo  | X   |
| 바이두     | baidu  | X   |
| QQ     | qq  | X   |
| WeChat     | wechat  | X   |
| Yahoo Japan     | open_y_jp  | X   |
| Mixi     | mixi  | X   |

