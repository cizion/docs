# 라이브리 댓글 동기화 가이드

## 1. 기본 설정

- 라이브리 댓글 동기화를 위해서는 라이브리가 설치된 페이지에 다음과 같은 JavaScript 코드를 삽입해 주셔야 합니다.

```
window.livereHooks = {
	// 댓글 작성 시 데이터를 받을 함수
	write: function(data) {},
	// 댓글 삭제 시 데이터를 받을 함수
	remove: function(data) {}
};
```

- ``livereHooks`` 객체 내의 ``write``, ``remove`` 프로퍼티에 다음과 같이 데이터를 받을 함수를 연결하거나 정의해 주셔야 합니다.

```
// 예제 함수
function addComment(data) {
	console.log(data);
}

window.livereHooks = {
	write: function(data) {
		return addComment(data);
	},
	remove: addComment
};
```

## 2. 전달되는 데이터 명세

- 사용자가 댓글을 새로 작성했을 경우 전달되는 내용 입니다.

```
{
	content: "댓글 내용",
	id: "사용자 SNS 고유 ID",
	name: "사용자 이름",
	profile: "사용자 프로필 이미지 URL",
	regdate: "작성일(Date 객체)",
	replySeq: "댓글 번호",
	strategy: "사용자 SNS 종류"
	type: "write"
}
```

- 사용자가 댓글 삭제 했을 경우 전달되는 내용 입니다.

```
{
	id: "사용자 SNS 고유 ID", 
	replySeq: "댓글 번호", 
	type: "remove"
}
```
