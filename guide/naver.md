# 라이브리 네이버 API 변경 관련 가이드

## 개요

네이버 로그인 변경과 관련해 다음과 같은 작업이 필요합니다.

1. 로그인 페이지 호출 API 변경
2. 로그인 후 `passport.livere.com`으로 `CODE` 전달
3. 응답으로 내려오는 `CODE`를 통해 본래 프로세스 진행
3. 네이버 로그인 `Callback URL` 변경

## 상세

### 1. 로그인 페이지 호출 API 변경 작업

기존:
`passport.livere.com/v1/auth/naver`

변경:
`https://passport.livere.com/redirect/to?url=https://czo.website.com/v1/auth/naver&force=true`


- 기존에 사용하시던 파라메터(e.g: `force=true`) 등은 `url` 파라메터 뒤에 붙여서 그대로 사용하시면 됩니다.
- `url` 파라메터의 경우 `encodeURIComponent`를 통해 인코딩 해주시기 바랍니다.

### 2. 로그인 후 passport.livere.com으로 CODE 전달

로그인 후 리다이렉트 페이지를 통해 전달되는 `CODE`를 다음과 같이 비동기 호출하여 주시기 바랍니다.(브라우저)

```
curl -v -X GET https://passport.livere.com/v1/auth/dfsn
	 -d 'code=4f50edcdc8ad594da319dbe8258
```

비동기 호출 예)

```
$.ajax({
   url: 'https://passport.livere.com/v1/auth/dfsn',
   method: 'GET',
   dataType: 'jsonp',
   data: {
      code: '4f50edcdc8ad594da319dbe8258'    
   }
}).done(function(data) {
	// data => { message: 'Okay, livere', 'code: 30vmzl2dz' }
});
```

### 3. 응답으로 내려오는 `CODE`를 통해 본래 프로세스 진행

2번 내용을 진행하게 되면 응답 값으로 새로운 `CODE`가 내려오게 됩니다. 해당 `CODE`를 통해 기존 로그인 프로세스를 진행하시면 됩니다.

### 4. 네이버 로그인 Callback URL 변경

네이버 로그인 개발자 센터 내 `Callback URL` 을 다음과 같이 변경 해주시기 바랍니다.

1. `http://cizion.website.com/v1/auth/naver/callback`
2. `https://cizion.website.com/v1/auth/naver/callback`
