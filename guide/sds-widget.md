# 라이브리 위젯 설치 가이드(삼성SDS)

## 1. 라이브리 위젯 설치

- 라이브리 위젯 설치에는 발급 받은 `id` 및 `uid`가 필요합니다.
- 다음은 라이브리 위젯 설치코드 입니다. 위젯을 설치하고 싶은 영역에 다음 설치코드를 붙여 넣으십시오.

- 한국

```
<div id="lv-widget" data-id="samsung_sds" data-uid="MTIxNC8yNzg2NC80NDQy">
    <script type="text/javascript">
        window.livereWidgetOptions = {
            /*카카오톡은 모바일에서만 노출 됩니다.*/
            list: ['twitter', 'facebook', 'linkedIn', 'kakaotalk'],
            customCss: 'https://cdn-city.livere.com/consumers/sds/sds.css',
            /*카카오톡 app key 설정*/
            kakaoAppKey: 'KAKAO_APP_KEY',
            lazy: true
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
    <noscript>라이브리 댓글 작성을 위해 JavaScript를 활성화 해주세요.</noscript>
</div>
```

- 중국

```
<div id="lv-widget" data-id="samsung_sds" data-uid="MTIxNC8yNzg2NC80NDQy">
    <script type="text/javascript">
        window.livereWidgetOptions = {
            list: ['weibo_sina', 'renren'],
            customCss: 'https://cdn-city.livere.com/consumers/sds/sds.css',
            lazy: true
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
    <noscript>라이브리 댓글 작성을 위해 JavaScript를 활성화 해주세요.</noscript>
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
- 지원 되는 SNS는 크게 2가지 부류로 나뉩니다.
    1. REST API를 사용하는 SNS
    2. 클라이언트 API, 혹은 링크 공유를 지원하는 SNS

- 지원 현황은 다음과 같습니다.

| 이름  | 파라메터 | 클라이언트 API         |
| :-------- | :-------------------- | :--: |
| 트위터   | twitter      | X   |
| 페이스북     | facebook  | X   |
| 네이버     | naver  | X   |
| 카카오 스토리     | kakao  | X   |
| 카카오톡  | kakaotalk | O |
| 링크드인   | linkedIn | X  |
| Mixi     | mixi  | X   |
| 웨이보     | weibo_sina  | X   |
| 런런왕     | renren  | X   |
| Qzone(QQ)     | qq  | O   |
| 텐센트 웨이보     | tencent  | O   |

- 테스트 페이지는 다음 URL을 참고해주시기 바랍니다.
[한국 테스트 페이지](http://test.livere.co.kr/city/sds-widget.html)
[중국 테스트 페이지](http://test.livere.co.kr/city/sds-widget-cn.html)

## 3. 가로 사이즈 설정

- 가로 사이즈를 설정하기 위해서는 `window` 스코프, 즉 전역 스코프에 `livereWidgetOptions` 변수가 선언되어 있어야 합니다.
- 가로 사이즈를 설정하는 방법은 다음과 같습니다.

```
window.livereWidgetOptions = {
    width: 250px
};
```

- 최소 200px, 최대 300px 사이에서 설정할 수 있습니다.

## 4. 카카오톡 공유 설정

- 카카오톡은 공유 URL에 대한 검증을 진행합니다.
- 따라서 `appId`와 `appKey`를 각각 발급받아 이를 설정해 줘야 합니다.
- 다음 URL에서 `appId`와 `appKey`를 발급 받으시길 바랍니다.

[카카오 개발자 사이트](https://dev.kakao.com)

- `appId`와 `appKey`를 설정하는 방법은 다음과 같습니다.

```
window.livereWidgetOptions = {
    ...
    kakaoAppKey: ... // 카카오톡 App Key 설정
};
```

## 5. 커스텀 CSS 스타일

- `iframe` 내 `CSS` 스타일을 다음과 같이 임의로 지정할 수 있습니다.

```
window.livereWidgetOptions = {
    ...
    customStyle: '#container .list { padding: 19px 12px 20px 18px } #container button { margin: 0px 11.5px; }'
};
```

## 6. 공유 파라메터 설정

- 다음과 같이 공유 시 SNS에 같이 노출될 파라메터를 설정할 수 있습니다.
- 각 SNS마다 공유 시 노출되는 파라메터는 상이합니다.

```
<meta property="og:description" content="삼성 SDS 테스트" />
<meta property="og:url" content="http://www.samsungsds.com" />
<meta property="og:image" content="http://image.samsungsds.com/global/ko/images/logo.png" />
<title>삼성 SDS 테스트</title>
```

## 7. SDS 닷컴 적용 사항

- 현재 SDS 닷컴 형태의 공유 기능을 사용하기 위해서는 다음과 같은 적용 과정이 필요합니다.
(http://www.samsungsds.com/global/support/resources?lang=ko 플로우 참고)

1. 페이지 로드 시 공유하기 버튼이 들어갈 각 영역에 다음 div를 생성합니다.

```
<div class="test1" data-id="samsung_sds" data-uid="MTIxNC8yNzg2NC80NDQy" data-url="URL" data-title="TITLE" data-image="IMG_URL"></div>
```

- `data-id`, `data-uid`는 필수적으로 들어가야 하며 `data-url`, `data-title`, `data-image`는 선택 사항입니다.

2. `window.livereOptions` 내 `lazy: true` 설정 시 최초 스크립트 실행 때 위젯이 로드되지 않게 설정할 수 있습니다.
3. 다음으로 사용자가 [공유하기] 버튼을 눌렀을 때 다음 스크립트를 실행하십시오.

```
Toybox.reloadTo(SELECTOR);
// e.g) Toybox.reloadTo('.test1');
```

- 해당 플로우에 대한 테스트 페이지는 [한국 테스트 페이지](http://test.livere.co.kr/city/sds-widget.html) 에서 확인할 수 있습니다.