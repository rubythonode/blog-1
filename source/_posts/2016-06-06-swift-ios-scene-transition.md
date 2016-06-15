title: Swift를 이용한 iOS 화면 전환
date: 2016-06-06 00:46:32
category:
- Dev
- iOS
tags:
- iOS
- swift
- 화면 전환
---
iOS에서 화면전환에는 크게 4가지가 있다고 한다.

1. 뷰 컨트롤러의 뷰 위에 다른 뷰컨트롤러의 뷰를 가져와 바꿔치기
2. 뷰 컨트롤러에서 다른 뷰 컨트롤러를 호출하기
3. 내비게이션 컨트롤러 사용하기
4. 화면 전환용 객체 세그웨이(Segueway) 사용하기

위에서 1번 방식은 하나의 뷰 컨트롤러가 두 개 이상의 싱글 뷰를 관리하기에 좋은 방법이 아니라 잘 사용하지 않는다.
<!-- more -->

기본적으로 iOS의 화면 전환은 stack과 같은 느낌이다. 화면이 바뀔 때마다 원래 있던 화면 위에 새 화면이 올라가는 형식이다. 여기서 주의 해야할 부분은 이전 화면으로 돌아갈 때엔 새 화면을 올릴 때처럼 이전화면을 새로 위에 올리는게 아니라 stack에서 pop을 하는 것처럼 올렸던 화면을 빼야한다는 것이다. 이전 화면으로 돌아갈 때 현재화면의 pop이 아니라 이전화면의 push로 구현시엔 인스턴스가 중복되어 에러가 발생할 수도 있고, 에러가 나지 않더라도 메모리 낭비로 이어지기때문에 주의해야한다. 때문에 위에서 언급한 화면 전환 기법들엔 push에 해당하는 메소드와 pop에 해당하는 메소드가 쌍을 이루고있다.

### 2. 뷰 컨트롤러에서 다른 뷰 컨트롤러를 호출하기
- `presentViewController(_:animated:completion:)`
- `dismissViewControllerAnimated(_:completion:)`

이 방법에서 주의해야할 점은 위 메소드를 호출하는 객체가 동일하다는 것이다.
뷰 컨트롤러 A가 `self.presentViewController(_:animated:completion:)`로 뷰 컨트롤러 B를 호출하면 B와 A사이에는 다음과 같은 속성을 갖는다.
A.presentedViewController => B
B.presentingViewController => A

여기서 B에서 A로 돌아갈 땐 A에서 B로 갈 때처럼 `self.dismissViewControllerAnimated(_:completion:)`이 아니라 다음과 같이 A를 참조하여  호출한다. `self.presentingViewController?.dismissViewControllerAnimated(_:completion:)`

### 3. 네비게이션 컨트롤러 사용하기
- `pushViewController(_:animated:)`
- `popViewControllerAnimated(_:)`

이 방법은 전체적인 화면 관리를 navigationController가 맡아서 한다는 것이 특징이다. 2번 방법은 뷰 컨트롤러가 직접 메소드를 호출했지만, 3번 방법은 2가지 메소드 모두 아래와 같이 navigationController가 호출한다.
`self.navigationController?.pushViewController(_:animated:)`
`self.navigationController?.popViewControllerAnimated(_:)`

네비게이션 컨트롤러를 이용한 방법의 특징 중 하나는 `pushViewController(_:animated:)`를 이용하여 화면 전환을 하게 되면 전환된 뷰 컨트롤러에 자동으로 네비게이션 바가 탑재되고, 네비게이션 바 왼쪽에 아이템을 별도로 지정하지 않았을 경우 자동으로 `popViewControllerAnimated(_:)`를 이용한 Unwind가 구현된다는 것이다.

### 4. 화면 전환용 객체 세그웨이(Segueway) 사용하기
- Action Segue
- Manual Segue

세그웨이는 스위프트 코드가 아닌 스토리보드 상에서 그래피컬하게 전환을 설정한다. 세부적으로 코딩도 들어가긴 하지만 기본적인 전환 관계를 스토리보드상에서 연결하며 직관적으로 알아보기 쉽게 화살표로 표시를 해준다. 세그웨이엔 2가지 'Action Segue'와 'Manual Segue'가 존재한다. 액션 세그는 버튼등 이벤트가 발생하는 객체와 뷰 컨트롤러를 직접 연결할 때 사용하며, 매뉴얼 세그는 뷰 컨트롤러와 뷰 컨트롤러를 직접 연결하며 `performSegueWithIdentifier(_:sender:)`를 특정 시점에 호출하여 전환을 한다. 세그웨이를 이용한 전환에서 Unwind의 경우 스토리보드에서 뷰 컨트롤러를 선택시 윗 부분에 나타나는 3개의 아이콘 중에서 오른쪽 끝에 있는 Exit 아이콘에 연결하여 복귀할 뷰 컨트롤러 클래스에서 정의된 Unwind 메소드를 지정함으로써 구현한다.

세그웨이는 UIStoryboardSegue 클래스를 상속받아 커스텀 세그를 구현할 수 있다. 이미 정의되어있는 `perform(_:)` 메소드를 override하여 구현한다.

`prepareForSegue(segue: UIStoryboardSegue, sender: Anyobject?)` 메소드를 전환 되는 시점의 뷰 컨트롤러 클래스에 재정의(override)하여 세그웨이가 실행되기 전 특정 코드를 실행할 수 있다.
