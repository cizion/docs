# 라이브리 소셜 로그인 API 명세

## 1. 라이브리 소셜 로그인 인증 프로세스
- 전체적인 라이브리 소셜 로그인 인증 프로세스는 다음과 같습니다.
- 해당 프로세는 ``1. 로그인 / 추가 회원연동 API`` 및 ``3. 회원 연동 해제 API`` 에서 사용 됩니다.

![소셜 로그인 프로세스](http://cdn-city.livere.com/images/passport.sample.png)

## 2. API 명세

### 1. 로그인 / 추가 회원연동 API

1. **로그인 / 추가 회원연동 요청**

	**[Request]**

	```
	GET /v1/auth/{{SOCIAL_MEDIA_NAME}} HTTP/1.1
	Host: passport.livere.com
	```

	**[Request 예]**

	```GET
	curl -v -X GET https://passport.livere.com/v1/auth/facebook
	 -d 'redirectUrl=https://www.shilladfs.com/../passport/redirect.html'
	```
	* 아래 파라메터들을 포함해 클라이언트에서 ``window.open`` 함수를 통해 요청 합니다.

	| 키  | 설명 | 필수         | 예시 |
	| :-------- | :-------------------- | :--: | --- |
	| redirectUrl   | 로그인 후 리다이렉트 할 고객사 페이지(전달된 양식)      | O   | |
	| force    | 추가 연동이 아닌 해당 SNS로 로그인   | O   | `true`   |

	* ``redirectUrl`` 파라메터의 경우 전달된 HTML 페이지를 고객사 쪽 도메인에 업로드 해주시면 됩니다.(``1. 라이브리 소셜 로그인 인증 프로세스`` 참고)
  * 추가 회원연동이 아닌 해당 SNS로 로그인하려면 force=true 옵션을 주셔야 합니다. 자세한 것은 FAQ를 참고해주세요.

2. **로그인 후 회원 정보 CODE 전달**

	* 위 프로세스에서 파라메터로 전달된 리다이렉트 페이지에서 상위 부모 페이지의 함수를 호출합니다. 따라서 로그인 API를 요청한 페이지에 ``회원 정보 CODE``를 전달 받을 함수를 작성해 주셔서 합니다.

	**[호출 예(라이브리 쪽 작업)]**

	```
	function sendAuthCode(code) {
		return window.opener.setAuthCode(code);
	}
	```

	**[함수 작성 예(고객사 쪽 작업)]**

	```
	function setAuthCode(code) {
		// 고객사 쪽 인증 프로세스
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
	* ``secretToken``은 절대 외부로 유출되면 안되기 때문에 **반드시 서버에서 요청을 진행해 주시기 바랍니다.**

	**[Request]**

	```
	GET /v1/valid HTTP/1.1
	Host: passport.livere.com
	```

	아래 파라메터들을 서버에서 GET으로 요청 합니다.

	| 키  | 설명 | 필수         |
	| :-------- | :-------------------- | :--: |
	| secretToken   | 사전에 전달된 비밀 토큰      | O   |
	| code     | 회원 정보 요청 CODE  | O   |

	**[Request 예]**

	```
	curl -v -X GET https://passport.livere.com/v1/valid
	 -d 'secretToken={{secretToken}}' \
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
	* 해당 API는 클라이언트에서 호출해야 합니다.

	**[Request]**

	```
	GET /v1/release HTTP/1.1
	Host: passport.livere.com
	```

	아래 파라메터들을 클라이언트에서 GET으로(AJAX) 요청 합니다.

	| 키  | 설명 | 필수         |
	| :-------- | :-------------------- | :--: |
	| memberSeq   | 연동 해제할 memberSeq      | O   |


	**[Request 예]**

	```
	curl -v -X GET https://passport.livere.com/v1/release
	-d 'memberSeq=1239532'
	```

	**[Response]**

	```
	HTTP/1.1 200 OK
	{
		"message": "Release complete",
        "code": "7f7761cffd492153cbad728c",
	}
	```

2. **회원 정보 요청**

	* 전달된 ``CODE``를 통해 현재 라이브리에 로그인 한 사용자의 정보를 갱신합니다.
	* ``로그인 API`` 문서의 ``1-3 회원 정보 요청`` 부분과 동일한 API 입니다.
	* ``secretToken``은 절대 외부로 유출되면 안되기 때문에 **반드시 서버에서 요청을 진행해 주시기 바랍니다.**

	**[Request]**

	```
	GET /v1/valid HTTP/1.1
	Host: passport.livere.com
	```

	아래 파라메터들을 서버에서 GET으로 요청 합니다.

	| 키  | 설명 | 필수         |
	| :-------- | :-------------------- | :--: |
	| secretToken   | 사전에 전달된 비밀 토큰      | O   |
	| code     | 회원 정보 요청 CODE  | O   |

	**[Request 예]**

	```
	curl -v -X GET https://passport.livere.com/v1/valid
	 -d 'secretToken={{secretToken}}' \
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
| 구글     | google  | O   |
| 링크드인     | linkedin  | O   |
| 네이버     | naver  | O   |
| 카카오톡     | kakao  | O   |
| 인스타그램     | instagram  | O   |
| 웨이보     | weibo  | O   |
| 런런왕     | renren  | O   |
| 도우반     | douban  | O   |
| 바이두     | baidu  | O   |
| QQ     | qq  | O   |
| WeChat     | wechat  | O   |
| Yahoo Japan     | yj  | O   |
| Mixi     | mixi  | O   |
| Line     | line  | O |
| Pinterest | pinterest | O |

## 4. 각 SNS별 JSON 응답 키

* API 응답 시 내려오는 JSON SNS 키 목록입니다.

| 이름  | Key |
| :-------- | :-------------------- |
| 트위터   | twitter      |
| 페이스북     | facebook  |
| 구글     | google_plus  |
| 링크드인     | linkedIn  |
| 네이버     | naver  |
| 카카오톡     | kakao  |
| 인스타그램     | instagram  |
| 웨이보     | weibo_sina  |
| 런런왕     | renren  |
| 도우반     | douban  |
| 바이두     | baidu  |
| QQ     | qq  |
| WeChat     | wechat  |
| Yahoo Japan     | open\_y_jp  |
| Mixi     | mixi  |
| Line     | line  |
| Pinterest | pinterest |

## 5. 각 SNS별 Mobile WebView 로그인 지원

* 일부 SNS는 각 사 정책으로 Mobile WebView 로그인을 지원하지 않고 별도 로그인 방식을 요구합니다. 

| 이름  | Mobile WebView 로그인 지원 여부 |
| :-------- | :-------------------- |
| 트위터   | O      |
| 페이스북     | O  |
| 구글     | O  |
| 링크드인     | O  |
| 네이버     | O  |
| 카카오톡     | O  |
| 인스타그램     | O  |
| 웨이보     | O  |
| 런런왕     | O  |
| 도우반     | O  |
| 바이두     | O  |
| QQ     | X  |
| WeChat     | X  |
| AliPay     | X  |
| Yahoo Japan     | O  |
| Mixi     | O  |
| Line     | O  |
| Pinterest | O |

## 6. FAQ

* facebook이 대표계정인 사용자가 baidu로 로그인하는 경우 대표계정이 facebook으로 처리됩니다.
	* `force=true` 파라미터를 추가하시면 됩니다.

       ```https://passport.livere.com/v1/auth/baidu?force=true```
       
* 핸드폰에서 실행 시 **바이두** 로그인 화면이 모바일 화면이 아닌 PC 화면으로 보입니다.
    * `display=mobile` 파라미터를 추가하시면 됩니다.

       ```https://passport.livere.com/v1/auth/baidu?display=mobile```
       
*  passport.livere.com IP 주소가 계속 변경됩니다.
    * 시지온 패스포트 서비스는 안정성을 위해서 AWS에서 실행됩니다. AWS 정책상 Load balancer는 고정 IP 유지가 어려워서 유동 IP를 사용합니다. 

*  회사 방화벽 정책으로 인해서 passport.livere.com 접속 시 고정 IP가 필요합니다. 
    * 대부분의 passport 기능은 일반 사용자 브라우저에서 실행되기 때문에 방화벽 정책에 의존적이지 않습니다.
    * /v1/valid 등의 서버에서 사용하는 내부 API의 경우에는 담당자를 통해서 별도로 문의주세요.  

*  모바일에서 위챗 로그인시 오류가 발생합니다.
    * AppID, AppSecret이 맞는지 확인합니다.
    * Android의 경우에는 package name과 signature를 등록해야 하고, iOS의 경우에는 bundle name을 등록해야 합니다.
    * Android의 경우에 Proguard에 WeChat SDK 관련 package가 예외처리 되어 있는지 확인합니다.
    * 위 사항이 모두 이상이 없는데도 로그인 오류 발생시 담당자를 통해서 별도로 문의주세요.
