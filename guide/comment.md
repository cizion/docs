# 라이브리 댓글 동기화 가이드

## 1. 개요

- 라이브리는 사용자가 해당 라이브리에서 글을 작성하거나 삭제 할 때 마다 아래 설정된 함수로 데이터를 전달합니다. 아래 내용들을 참고하셔서 넘겨 받은 데이터를 자체 데이터베이스에 저장하시거나 갱신 하시면 됩니다.

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
	content: (TEXT) "댓글 내용",
	id: (STRING) "사용자 SNS 고유 ID",
	memberGroupSeq: (INTEGER) "사용자의 라이브리 고유 회원 번호",
	name: (STRING) "사용자 이름",
	profile: (TEXT) "사용자 프로필 이미지 URL",
	regdate: (DATETIME) "작성일(Date 객체)",
	replySeq: (INTEGER) "댓글 번호",
	septSns: (STRING) "공유한 SNS"
	strategy: (STRING) "사용자 SNS 종류"
	type: (STRING) "write"
}
```

- 사용자가 댓글 삭제 했을 경우 전달되는 내용 입니다.

```
{
	id: (STRING) "사용자 SNS 고유 ID", 
	memberGroupSeq: (INTEGER) "사용자의 라이브리 고유 회원 번호",
	replySeq: (INTEGER) "댓글 번호", 
	type: (STRING) "remove"
}
```
