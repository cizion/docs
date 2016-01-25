# 라이브리 설치 가이드

## 1. 라이브리 설치

- 라이브리 댓글 설치에는 발급 받은 `id` 및 `uid`가 필요합니다.
- 일반 사용자의 `id`는 `city`로 고정됩니다.
- 다음은 라이브리 댓글 설치코드 입니다. 댓글을 설치하고 싶은 영역에 다음 설치코드를 붙여 넣으세요.

```
<div id="lv-container" data-id="[ID]" data-uid="[UID]">
	<script type="text/javascript">
		window.livereOptions = {
			// options
		};
	
		(function(d, s) {
			var j, e = d.getElementsByTagName(s)[0];

			if (typeof LivereTower === 'function') { return; }

			j = d.createElement(s);
			j.src = 'https://cdn-city.livere.com/js/embed.dist.js';
			j.async = true;

			e.parentNode.insertBefore(j, e);
		})(document, 'script');
	</script>
	<noscript>라이브리 댓글 작성을 위해 JavaScript를 활성화 해주세요.</noscript>
</div>
```

- 테스트 페이지는 다음 링크를 참고하시기 바랍니다.
[테스트 페이지](http://test.livere.co.kr/city/sample.html)

## 2. 라이브리 페이지 URL(refer) 설정

- 라이브리는 기본적으로 페이지 URL을 자동으로 설정합니다.
- 하지만 사이트 특성 상 별도의 `permalink`가 존재하거나 페이지를 `queryString`을 이용해 구분한다면 직접 `refer`를 설정해주셔야 합니다.

- `refer`는 최대 150자까지 설정할 수 있으며 길이를 초과할 경우 150자까지만 인식합니다.
- `http`, `https`와 같은 프로토콜은 제외하고 입력해주셔야 합니다.
- `refer`를 설정하는 방법은 다음과 같습니다.

```
window.livereOptions = {
	refer: 'livere.com/p/1'
};
```

## 3. 기타 SNS 공유 설정

- 라이브리는 SNS 공유 시 필요한 파라메터를 얻기 위해 페이스북의 `ogp`를 기본적으로 채용하고 있습니다.
- 지원하는 항목은 `title`, `description`, `url`, `logo` 입니다.
- `ogp`를 설정하는 방법은 다음과 같습니다.

```
<head>
	...
	<meta property="og:description" content="[DESCRIPTION]" />
	<meta property="og:url" content="[URL]" />
	<meta property="og:image" content="[IMAGE URL]" />
	<meta property="og:title" content="[TITLEL]" />
	...
</head>
```

- 대부분의 SNS는 댓글 전송 시 파라메터를 함께 전송하지만 카카오 스토리의 경우 공유 시 카카오 스토리 서버에서 직접 `ogp`를 읽어갑니다.
- 만약 `ogp`가 설정되지 않았다면 `title`, `url`의 경우 각각 `document.title` 정보와 `refer`를 기반으로 생성합니다.

## 4. 브라우저 지원 현황

- 라이브리는 다양한 기능을 제공하기 위해 공식적으로 `IE10`부터 지원합니다.
- 일부 구형 브라우저에서는 사용이 가능하나 기능들의 정상 동작을 보장하지 않습니다.

1. Desktop browser
	- Chrome
	- Safari
	- Internet Explorer 10+

2. Mobile browser
	- Android Browser(Android 4.0+)
	- Chrome for Android
	- Safari(iOS 6+)
	- Internet Explorer(Windows Phone 7.5+)

- `IE` 브라우저에 대한 자세한 내용은 다음 표를 참고하시기 바랍니다.

| 구분 | IE8> | IE8 | IE9 | IE9< 
| :-------- | :--- | :--- | :--- | :---
| 댓글 리스트 보기 | 스크롤 형태로 지원 | O | O | O
| 댓글에 대한 액션 | O | O | O | O
| 이미지, 동영상 첨부 | X | O | O | O
| 이미지 드래그 & 드랍 | X | X | X | O
| 마이페이지 지원 | X | O | O | O
| 댓글 내 URL 썸네일 | X | O | O | O
| 실시간 댓글 상호작용 기능 | X | X | O | O
| 댓글 인용 기능 | X | X | O | O