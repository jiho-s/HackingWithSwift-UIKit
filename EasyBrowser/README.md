# Easy Browser

> FileManager로 사진 이름 조회후 사진 표시

[![Swift Version][swift-image]][swift-url]

## 목차

1. [Setting up](#setting-up)
2. [Creating a simple browser with WKWebView](#creating-a-simple-browser-with-wkwebview)
3. [Choosing a website: UIAlertController action sheets](#choosing-a-website:-uialertcontroller-action-sheets)
4. [Monitoring page loads: UIToolbar and UIProgressView](#monitoring-page-loads:-uitoolbar-and-UIProgressView)

## Setting up

- 프로젝트 생성
- Navigation Controller 설정

# Creating a simple browser with WKWebView

- WkWebView 추가

  ```swift
  import WebKit
  ```

  컨트롤러의 프로퍼티 추가

  ```swift
  var webView: WKWebView!
  ```

- `loadView()`메소드 추가

  ```swift
  override func loadView() {
      webView = WKWebView()
      webView.navigationDelegate = self
      view = webView
  }
  ```

- 프로토콜 구현

  ```swift
  class ViewController: UIViewController, WKNavigationDelegate {
  }
  ```

- `viewDidLoad()`에 추가

  ```swift
  let url = URL(string: "https://www.naver.com")!
  webView.load(URLRequest(url: url))
  webView.allowsBackForwardNavigationGestures = true
  ```

  `load` `URLRequest`에 해당 URL로 새로운 객채를 만들고 로드할 webView에 할당

  `allowBackForwardNavigaionGestures`는 가장자리에서 스와이프하여 이동하는거 활성화

##   Choosing a website: UIAlertController action sheets

- BarItem 추가

  `viewDidLoad()`에 다음을 추가

  ```swift
  navigationItem.rightBarButtonItem = UIBarButtonItem(title: "Open", style: .plain, target: self, action: #selector(openTapped))
  ```

- `openTapped()`메소드 작성

  ```swift
  @objc func openTapped() {
      let ac = UIAlertController(title: "Open page…", message: nil, preferredStyle: .actionSheet)
      ac.addAction(UIAlertAction(title: "apple.com", style: .default, handler: openPage))
      ac.addAction(UIAlertAction(title: "hackingwithswift.com", style: .default, handler: openPage))
      ac.addAction(UIAlertAction(title: "Cancel", style: .cancel))
      ac.popoverPresentationController?.barButtonItem = self.navigationItem.rightBarButtonItem
      present(ac, animated: true)
  }
  ```

- `openPage()` 메소드 작성

  ```swift
  func openPage(action: UIAlertAction) {
      let url = URL(string: "https://" + action.title!)!
      webView.load(URLRequest(url: url))
  }
  ```

- navigationTitle 설정

  `webView(_:, didFisnish)`를 이용하여 최근에 로드된 page로 title을 설정 할 수 있다

## Monitoring page loads: UIToolbar and UIProgressView

### `UIToolbar` 활성화

- `UIToolbar`, `UIProgressView`

  - `UIToolbar`

    `UIToolbar`는 사용자가 클릭할 수 있는 `UIBarButtonItem` 모음을 보여준다.

    `UINavigationController`안에 자동으로 `toolbarItems`와 같이 제공된다.

  - `UIProgressView`

    `UIProgressView`는 전체 진행상황을 확인 할 수 있다.

- `UIBarButtonItem` 추가

  `viewDidLoad()`에 다음을 추가한다.

  ```swift
  let spacer = UIBarButtonItem(barButtonSystemItem: .flexibleSpace, target: nil, action: nil)
  let refresh = UIBarButtonItem(barButtonSystemItem: .refresh, target: webView, action: #selector(webView.reload))
  
  toolbarItems = [spacer, refresh]
  navigationController?.isToolbarHidden = false
  ```

  `.flexibleSpace`는 클릭할 수 없고 공간 만 차지하는 버튼을 생성

  두번쨰 줄에 부튼은 새로고침 형태의 버튼 제공

### `UIProgressView` 추가

- `UIProgressView`추가시 문제점
    - `UIView`의 서브 클래스인 `UIProgressView`를 바로 `UIToolbar`에 넣을 수 없다. 넣기 위해서는 `UIBarButoonItem` 형태로 래핑해야 한다.
    - `WKNavigationDelegate`는 페이지 로드 정도를 알려주는 `estimatedProgress`가 언제 바뀌는지 알려주지 않는다. 따라서 이를 key-value observing를 iOS에게 요청해야한다

- `UIProgressView` 배치

  `ViewController`에 프로퍼티 추가

  ```swift
  var progressView: UIProgressView!
  ```

  `viewDidLoad()` 에 배치

  ```swift
  progressView = UIProgressView(progressViewStyle: .default)
  progressView.sizeToFit()
  let progressButton = UIBarButtonItem(customView: progressView)
  ```

  마지막 줄은 `progressView`가 `UIBarButtonItem`으로 들어갈 수 있게 해준다.

  `toolbarItems`에 넣는다.

  ```swift
  toolbarItems = [progressButton, spacer, refresh]
  ```

- `progressView`에 `estimatedProgress` 값 표시하기

  `viewDidLoad()`에 `observer`를 `self`로 추가한다.

  ```swift
  webView.addObserver(self, forKeyPath: #keyPath(WKWebView.estimatedProgress), options: .new, context: nil)
  ```

  `addObsserver()` 메소드는 `observer`가 누구인지 설정

  `forKeyPath`는 단지 프로퍼티의 이름을 설정하는것이 아니고 다른 프로퍼티 안에 있는 프로퍼티등 특정한 경로를 입력 할 수 있다. `#keyPath`는 컴파일시 `WKWebView`클래스가 실제 `estimatedProgress`를 가지고 있는지 확인 할 수 있게 해준다.

  `context`는 호출된 `observer`가 맟는지 확인한다.

  > 복잡한 프로그래밍에서는 `removeObserver()`로 `observer`를 해제해야한다.

  `observer`를 등록한 후에는 `observeValue()`메소드를 구현해야한다.

  ```swift
  override func observeValue(forKeyPath keyPath: String?, of object: Any?, change: [NSKeyValueChangeKey : Any]?, context: UnsafeMutableRawPointer?) {
      if keyPath == "estimatedProgress" {
          progressView.progress = Float(webView.estimatedProgress)
      }
  }
  ```

  

## 정보

[https://www.hackingwithswift.com/100/24]

[swift-image]:https://img.shields.io/badge/swift-5-orange.svg
[swift-url]:https://swift.org
