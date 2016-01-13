# 라이브리 클라이언트 위젯 설치 가이드

## 1. 라이브리 클라이언트 위젯 설치

- 라이브리 위젯 설치에는 발급 받은 `id` 및 `uid`가 필요합니다.
- 다음은 라이브리 위젯 설치코드 입니다. 위젯을 설치하고 싶은 영역에 다음 설치코드를 붙여 넣으십시오.

```
<div id="lv-widget" data-id="[ID]" data-uid="[UID]">
	<script type="text/javascript">
		window.livereWidgetOptions = {
			list: ['twitter', 'facebook', 'naver', 'kakao', 'mixi'], // SNS 선택
			target: 'cidget', // cidget 고정
            size: 72, // 위젯 크기 선택
			width: '300px', // 위젯 가로 사이즈 설정
			height: '80px' // 위젯 세로 사이즈 설정
		};

		(function(d, s) {
			var j, e = d.getElementsByTagName(s)[0];

			if (typeof Toybox === 'function') { return; }

			j = d.createElement(s);
			j.src = 'https://cdn-city.livere.com/js/toybox.dist.js';
			j.async = true;

			e.parentNode.insertBefore(j, e);
		})(document, 'script');
	</script>
	<noscript>라이브리 공유를 위해 JavaScript를 활성화 해주세요.</noscript>
</div>
```

## 2. 공유 SNS 설정

- 공유할 SNS를 설정하기 위해서는 `window` 스코프, 즉 전역 스코프에 `livereWidgetOptions` 변수가 선언되어 있어야 합니다.
- 공유 SNS를 설정하는 방법은 다음과 같습니다.


```
window.livereWidgetOptions = {
	list: ['twitter', 'facebook', 'naver', 'kakao', 'mixi']
};
```

- 원하시는 SNS 종류를 설정에 따라 자유롭게 변경할 수 있습니다.

- 지원 현황은 다음과 같습니다.

| 이름  | 파라메터 | 모바일 전용 |
| :-------- | :-------------------- | :--- |
| 트위터   | twitter      | X |
| 페이스북     | facebook  | X |
| 구글 플러스 | google_plus | X |
| 네이버     | naver  | X |
| 카카오톡     | kakao  | O |
| 카카오 스토리     | story  | X |
| 믹시     | mixi  | X |
| 라인 | line | O |
| 웨이보     | weibo  | X |
| 런런왕     | renren  | X |
| Qzone     | qzone  | X |
| 텐센트 웨이보     | tencent  | X |
| 카이신왕     | kaixin  | X |
| 도우반     | douban  | X |
| 바이두 티에바     | tieba  | X |

- 테스트 페이지는 다음 URL을 참고해주시기 바랍니다.
[테스트 페이지](http://test.livere.co.kr/city/cidget.html)

## 3. 위젯 아이콘 크기 설정

- 위젯 아이콘 크기를 설정하기 위해서는 `window` 스코프, 즉 전역 스코프에 `livereWidgetOptions` 변수가 선언되어 있어야 합니다.
- 아이콘 크기는 PC용(24, 48), 모바일용으로(72, 99) 구분되어 있습니다.
- 아이콘 크기를 설정하는 방법은 다음과 같습니다.

```
window.livereWidgetOptions = {
	...
	size: 72 // 24,48,72,99 중 선택
};
```

## 4. 위젯 사이즈(가로 세로 크기) 설정

- 사이즈를 설정하기 위해서는 `window` 스코프, 즉 전역 스코프에 `livereWidgetOptions` 변수가 선언되어 있어야 합니다.
- 사이즈를 설정하는 방법은 다음과 같습니다.

```
window.livereWidgetOptions = {
	...
	width: '250px',
	height: '50px'
};
```