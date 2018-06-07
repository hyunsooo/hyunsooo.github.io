---
layout: post
title: "팝업 컨트롤러 리팩토링"
author: "한현수"
---



### 2018. 6. 7_ **NoTitlePopupViewController** 리팩토링
EP의 팝업을 담당하는 컨트롤러인 `NoTitlePopupViewController`의 모든 소스코드를 갈아엎었다.  

목적은 `viewmode`라는 쓰레기를 없애버리고 싶었다.
`viewmode`는 `Int`형 데이터로 팝업의 구분값으로 1년 반 넘게 사용되고 있었다. 이를 `enum` 형태의 Key 배열로 전환, 깔끔한 `switch (key) { - }`로 팝업을 구분 짓고 싶었다.

```[swift]
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

퇴근시간까지는 시간이 좀 남아서, 나머지 오류가 났던 부분들도 조금씩 손보고 퇴근했다.  
퇴근시간에 다시 앱을 켜서 무작위 테스트 진행 결과, 또 수정해야할 부분이 나왔다.  

> - `EP 문의센터 나가기` 값 오류  
> - 채팅에서 이미지 여러 장 전송 시 팅김.
> - 채널에서 `SBDAdminMessage`에 대해서 `LongTapGestureRecognizer` 제거.
> - `SBDConnectionDelegate`가 구현되지 않은 ViewController에서 재연결 시, 잘 되지 않는 듯 하다. (어디까지나 추측..)

위에 세 가지는 내일 오전 내로 해결할 수 있을 것 같은데, 마지막 것이 문제이다.. 오류를 내기가 쉽지 않다. 어떤 이유로 발현되는지 아직 알아내지 못했기 때문이다. 못난 디버깅 실력을 탓해야지... 
