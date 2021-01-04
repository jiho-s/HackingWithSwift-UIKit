# Easy Browser

> FileManager로 사진 이름 조회후 사진 표시

[![Swift Version][swift-image]][swift-url]

## 목차

1. [Setting up](#setting-up)
2. [Creating a simple browser with WKWebView](#creating-a-simple-browser-with-wkwebview)
3. [Choosing a website: UIAlertController action sheets](#choosing-a-website:-uialertcontroller-action-sheets)

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

## 정보

[https://www.hackingwithswift.com/100/24]

[swift-image]:https://img.shields.io/badge/swift-5-orange.svg
[swift-url]:https://swift.org
