---
layout: post
title: "Popup ViewController refactoring"
author: "한현수"
---



### **NoTitlePopupViewController** 리팩토링
EP의 팝업을 담당하는 컨트롤러인 `NoTitlePopupViewController`의 모든 소스 코드를 다시 작성했다.  

>  `viewmode: Int` 값으로 팝업을 구분한 미친 코드는 삭제.  
>  `enum`을 활용해서 정상적으로 팝업을 구분.

```swift
enum PopupKey {
	case deleteFile
	case deleteFolder
	case leaveChannel
	case banUserInChannel
	case chatToChannel
	...
}
```

대충 이렇게 키 배열을 만들고, 각각의 `function`을 각 case에 달고 끝냈다. 

```swift
fileprivate func confirm() {
	guard let key = key else { return }
	switch (key) {
	case .deleteFile: deleteFile()
	case .deleteFolder: deleteFolder()
	...
	case .chatToChannel: chatToChannel()
	}
}
```

굉장히 간단하지만, 이곳저곳 퍼져있는 `viewmode` 값을 `key: PopupKey?`로 변경하려니 시간이 조금 걸렸다. 각각 배치된 함수들은 잘 동작한다. 

---

2018년 6월 7일, 오류 내역.  

> - `EP 문의센터 나가기` 값 오류  
> - 채팅에서 이미지 여러 장 전송 시 팅김.
> - 채널에서 `SBDAdminMessage`에 대해서 `LongTapGestureRecognizer` 제거.
> - `SBDConnectionDelegate`가 구현되지 않은 ViewController에서 재연결 시, 잘 되지 않는 듯 하다. (어디까지나 추측..)

위의 세 개 이슈는 바로 해결 가능, 마지막 이슈가 문제다.  
다 고치면, 관련 글을 기술할 예정이다.

끝.
