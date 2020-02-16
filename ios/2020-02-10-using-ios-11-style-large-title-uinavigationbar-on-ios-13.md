# iOS 13에서 iOS 11 스타일의 Large Title UINavigationBar 사용하기
iOS 13부터 [`UINavigationBar`][1]가 Large Title 상태일 때 기존의 반투명 배경 대신 완전 투명한 배경을 사용하도록 변경되었다.

![iOS 12와 iOS 13에서의 설정 앱 비교. 반투명한 배경 대신 루트 뷰의 배경이 보이는 것을 알 수 있다.][image-1]

이러한 변화와 함께 시스템 막대의 외관 설정을 손쉽게 도와주는 [`UIBarAppearance`][2] 클래스가 도입되었는데, 그 서브클래스인 [`UINavigationBarAppearance`][3]를 사용하면 내비게이션 막대를 간단하게 이전 스타일로 되돌릴 수 있다.
## UINavigationBarAppearance 객체 만들기
`UINavigationBarAppearance` 인스턴스는 다음과 같이 생성한다.
```swift
let appearance = UINavigationBarAppearance()
```
이 인스턴스는 [`WKWebViewConfiguration`][4]과 마찬가지로 커스텀 옵션 값들을 담는 객체이다. 때문에 초기화만으로는 아무 정보도 담지 못하고, 추가적인 프로퍼티 값 변경이나 메소드 호출을 통해 옵션을 주어야 한다.
`UIBarAppearance`는 배경 설정을 위한 메소드 3가지를 제공한다.
- [**`configureWithDefaultBackground()`**][5]: 반투명한 기본 배경과 그림자 값을 갖도록 설정한다.
- [**`configureWithOpaqueBackground()`**][6]: 현재 테마에 적절한 불투명한 색상을 갖도록 설정한다.
- [**`configureWithTransparentBackground()`**][7]: 그림자 없이 투명한 배경을 갖도록 설정한다.

여기서는 우리가 원하는 기본 반투명한 배경으로 설정을 덮어쓰기 위해 `configureWithDefaultBackground()`을 호출했다.
```swift
appearance.configureWithDefaultBackground()
```
이것으로 appearance 객체의 설정은 끝났다.
## UINavigationBarAppearance 객체 적용하기
이제는 방금 만든 appearance 객체를 내비게이션 막대에 적용할 일만 남았다. iOS 13에는 내비게이션 막대가 표시되는 3가지 상황이 있는데, 각 상황에 맞게 appearance 객체를 따로 적용할 수 있도록 되어있다.
- [**`standardAppearance`**][8]: 표준 높이의 내비게이션 막대에서 사용하는 외관 설정 (세로 모드에서 쓰임)
- [**`compactAppearance`**][9]: 콤팩트한 높이의 내비게이션 막대에서 사용하는 외관 설정 (가로 모드에서 쓰임)
- [**`scrollEdgeAppearance`**][10]: 스크롤 가능한 콘텐트가 맞닿은 내비게이션 막대의 가장자리에 닿을 때 사용하는 외관 설정 (Large Title이 보여질 때 쓰임)

이 프로퍼티는 `UINavigationBar`와 그것의 [전역 외관 프록시][11]인 [`UINavigationBar.appearance()`][12], 그리고 [`UINavigationItem`][13]에서 사용할 수 있다. 뷰 컨트롤러 내에서 설정하는 일반적인 상황이라면, 다음과 같은 방법으로 3가지 객체에 적용할 수 있다.
```swift
// 현재 뷰 컨트롤러의 UINavigationBar에만 새로운 appearance를 적용한다.
self.navigationItem.scrollEdgeAppearance = appearance
```

```swift
// 현재 내비게이션 컨트롤러의 모든 UINavigationBar에 새로운 appearance를 적용한다.
self.navigationController?.navigationBar.scrollEdgeAppearance = appearance
```

```swift
// 앱 내 모든 UINavigationBar에 새로운 appearance를 전역으로 적용한다.
UINavigationBar.appearance().scrollEdgeAppearance = appearance
```
끝이다! 완성된 전체 코드는 다음과 같다. 여기서는 현재 뷰 컨트롤러에서만 기본 배경 스타일이 필요한 상황을 가정하고 `UINavigationItem`에만 appearance를 적용했다.
```swift
let appearance = UINavigationBarAppearance()
appearance.configureWithDefaultBackground()
self.navigationItem.scrollEdgeAppearance = appearance
```
## 그 외
[`UIToolbar`][14]와 [`UITabBar`][15]의 외관도 같은 방법으로 [`UIToolbarAppearance`][16]와 [`UITabBarAppearance`][17]를 사용하여 변경할 수 있다.
## 참고
- [Modernizing Your UI for iOS 13 - WWDC 2019][18] - [@apple][19]
- [UINavigationBar changes in iOS 13][20] - [@sarunw][21]

[1]:	https://developer.apple.com/documentation/uikit/uinavigationbar
[2]:	https://developer.apple.com/documentation/uikit/uibarappearance
[3]:	https://developer.apple.com/documentation/uikit/uinavigationbarappearance
[4]:	https://developer.apple.com/documentation/webkit/wkwebviewconfiguration
[5]:	https://developer.apple.com/documentation/uikit/uibarappearance/3197997-configurewithdefaultbackground
[6]:	https://developer.apple.com/documentation/uikit/uibarappearance/3197998-configurewithopaquebackground
[7]:	https://developer.apple.com/documentation/uikit/uibarappearance/3197999-configurewithtransparentbackgrou
[8]:	https://developer.apple.com/documentation/uikit/uinavigationbar/3198028-standardappearance
[9]:	https://developer.apple.com/documentation/uikit/uinavigationbar/3198026-compactappearance
[10]:	https://developer.apple.com/documentation/uikit/uinavigationbar/3198027-scrolledgeappearance
[11]:	https://developer.apple.com/documentation/uikit/uiappearance "UIAppearance"
[12]:	https://developer.apple.com/documentation/uikit/uiappearance/1615010-appearance
[13]:	https://developer.apple.com/documentation/uikit/uinavigationitem
[14]:	https://developer.apple.com/documentation/uikit/uitoolbar
[15]:	https://developer.apple.com/documentation/uikit/uitabbar
[16]:	https://developer.apple.com/documentation/uikit/uitoolbarappearance
[17]:	https://developer.apple.com/documentation/uikit/uitabbarappearance
[18]:	https://developer.apple.com/videos/play/wwdc2019/224
[19]:	https://github.com/apple
[20]:	https://sarunw.com/posts/uinavigationbar-changes-in-ios13/
[21]:	https://github.com/sarunw

[image-1]:	/assets/2020-02-10-settings-ios-12-vs-13.svg "iOS 12와 iOS 13에서의 설정 앱 비교"