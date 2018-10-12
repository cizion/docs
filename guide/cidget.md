# 라이브리 클라이언트 위젯 설치 가이드

## 1. 라이브리 클라이언트 위젯 설치

라이브리 위젯 설치에는 발급 받은 `id` 및 `uid`가 필요합니다.

다음은 라이브리 위젯 설치코드 입니다. 위젯을 설치하고 싶은 영역에 다음 설치코드를 붙여 넣으십시오.

```html
<div id="lv-widget" data-id="[ID]" data-uid="[UID]">
	<script type="text/javascript">
		window.livereWidgetOptions = {
			list: ['twitter', 'facebook', 'naver', 'kakao', 'mixi'], // SNS 선택
			target: 'cidget', // cidget 고정
            size: 72, // 위젯 크기 선택
			width: '300px', // 위젯 가로 사이즈 설정
			height: '80px', // 위젯 세로 사이즈 설정
			align: 'right' // 위젯 정렬 설정,
			facebookAppId: ... // 페이스북 App Id 설정
			kakaoAppKey: ... // 카카오톡 App Key 설정
		};

		(function(d, s) {
			var j, e = d.getElementsByTagName(s)[0];

			if (typeof Toybox === 'function') { return; }

			j = d.createElement(s);
			j.src = 'https://101.livere.co.kr/js/toybox.dist.js';
			j.async = true;

			e.parentNode.insertBefore(j, e);
		})(document, 'script');
	</script>
	<noscript>라이브리 공유를 위해 JavaScript를 활성화 해주세요.</noscript>
</div>
```

## 2. 공유 SNS 설정

공유할 SNS를 설정하기 위해서는 `window` 스코프, 즉 전역 스코프에 `livereWidgetOptions` 변수가 선언되어 있어야 합니다.

공유 SNS를 설정하는 방법은 다음과 같습니다.

```
window.livereWidgetOptions = {
	list: ['twitter', 'facebook', 'naver', 'kakao', 'mixi']
};
```

원하시는 SNS 종류를 설정에 따라 자유롭게 변경할 수 있습니다.

지원 현황은 다음과 같습니다.

| 이름       | 파라메터        | 모바일 전용 | 사용 파라메터                        |
| :------- | :---------- | :----- | :----------------------------- |
| 트위터      | twitter     | X      | title, url                     |
| 페이스북     | facebook    | X      | url                            |
| 구글 플러스   | google_plus | X      | url                            |
| 네이버      | naver       | X      | title, url                     |
| 카카오톡     | kakao       | O      | title, url                     |
| 카카오 스토리  | story       | X      | url                            |
| 믹시       | mixi        | X      | url                            |
| 라인       | line        | O      | title, url                     |
| 웨이보      | weibo       | X      | title, url, image              |
| 런런왕      | renren      | X      | title, url, description, image |
| Qzone    | qzone       | X      | title, url                     |
| 카이신왕     | kaixin      | X      | title, url, image              |
| 도우반      | douban      | X      | title, url, image              |
| QQ       | qq          | X      | title, url, description, image |
| 위챗 QR 코드 | wechat      | PC     | url                            |

테스트 페이지는 다음 URL을 참고해주시기 바랍니다.
[테스트 페이지](http://test.livere.co.kr/city/cidget.html)

## 3. 위젯 아이콘 크기 설정

위젯 아이콘 크기를 설정하기 위해서는 `window` 스코프, 즉 전역 스코프에 `livereWidgetOptions` 변수가 선언되어 있어야 합니다. 아이콘 크기는 PC용(24, 48), 모바일용으로(37, 72, 74, 99) 구분되어 있습니다.

아이콘 크기를 설정하는 방법은 다음과 같습니다.

```
window.livereWidgetOptions = {
	...
	size: 72 // 24,37,48,72,74,99 중 선택
};
```

## 4. 위젯 사이즈(가로 세로 크기) 설정

사이즈를 설정하기 위해서는 `window` 스코프, 즉 전역 스코프에 `livereWidgetOptions` 변수가 선언되어 있어야 합니다. 사이즈를 설정하는 방법은 다음과 같습니다.

```
window.livereWidgetOptions = {
	...
	width: '250px',
	height: '50px'
};
```

또한 위젯 내부 사이즈를 다음과 같이 설정할 수 있습니다.

```
window.livereWidgetOptions = {
	...
	innerWidth: '350px'
};
```

## 5. 위젯 정렬 설정

위젯을 정렬하기 위해서는 `window` 스코프, 즉 전역 스코프에 `livereWidgetOptions` 변수가 선언되어 있어야 합니다. 위젯 정렬은 `left`, `center`, `right` 중 선택 가능합니다.(기본 `center`) 위젯을 정렬하는 방법은 다음과 같습니다.

```
window.livereWidgetOptions = {
	...
	align: 'right'
};
```

## 6. 페이스북, 카카오톡 공유 설정

페이스북과 카카오톡은 공유 URL에 대한 검증을 진행합니다. 따라서 `appId`와 `appKey`를 각각 발급받아 이를 설정해 줘야 합니다. 다음 URL에서 `appId`와 `appKey`를 발급 받으시길 바랍니다.

[페이스북 개발자 사이트](https://developer.facebook.com)

[카카오 개발자 사이트](https://dev.kakao.com)

`appId`와 `appKey`를 설정하는 방법은 다음과 같습니다.

```
window.livereWidgetOptions = {
	...
	facebookAppId: ... // 페이스북 App Id 설정
	kakaoAppKey: ... // 카카오톡 App Key 설정
};
```

## 7. 모바일 앱 내 WebView 사용 시

페이스북의 경우 공유 후 공유 결과에 대한 메세지가 뜨지 않고 `window.close` 메소드를 호출하기 때문에 별도의 `redirect_uri` 설정이 필요합니다. 따라서 WebView에서 사용 시 다음 내용을 `livereWidgetOptions` 내에 설정하셔야 합니다.

```
window.livereWidgetOptions = {
	...
	webview: true // WebView 설정
};
```

해당 옵션은 PC, 모바일 웹에서는 사용하지 마시고 꼭 WebView 내에서 페이지가 로드되었을 때만 사용해야 합니다.

## 8. 커스텀 CSS 스타일

`iframe` 내 `CSS` 스타일을 다음과 같이 임의로 지정할 수 있습니다.

```
window.livereWidgetOptions = {
	...
	customStyle: '#container .list { padding: 19px 12px 20px 18px } #container button { margin: 0px 11.5px; } #container button:first-child { margin-left: 0px; } #container button:last-child { margin-right: 0px; }'
};
```
`livereWidgetOptions.margin` 옵션을 사용하는 경우 `margin` 옵션은 무시됩니다

## 9. 위챗 공유에 대해서

위챗은 해당 페이지 주소의 QR 코드가 생기고 모바일 기기의 위챗 앱에서 QR 코드를 스캔하여 공유하는 방식으로 되어있습니다. 따라서 PC에서만 지원됩니다. 기본적으로 모바일에서 버튼을 숨기진 않지만, 고객사에서 숨겨도 괜찮습니다 (고객사에서 CSS 파일로 수정해주세요). 모바일에서 버튼을 누를 경우 alert 창으로 경고가 뜨게 됩니다. 위챗 공유 버튼을 누르게 되면 Modal (Layer popup) 형식으로 위챗 공유를 위한 QR 코드가 뜨게 됩니다 (현재 Modal 옵션 밖에 없습니다).

Modal 형식이 싫으시다면 `#wechat-share-wrapper`의 ID를 가진 `div`가 생기므로 이를 `CSS important`를 사용하여 직접 제어해주세요.
