# Social Media

> UIActivityViewController, navigationItem

[![Swift Version][swift-image]][swift-url]

## 목차

1. [Setting up](#setting-up)

2. [UIActivityViewController explained](#uiactivityviewcontroller-explained)

## Setting up

- StormViewer 복사하여 프로젝트 생성

## UIActivityViewController explained

- navigationbutton 추가

  `DetailViewController.swift` 에 `viewDidLoad()` 안에 다음을 추가

  ```swift
  navigationItem.rightBarButtonItem = UIBarButtonItem(barButtonSystemItem: .action, target: self, action: #selector(shareTapped))
  ```

  `.action` 은 상자에서 위쪽으로 나가는 화살표 아이콘을 의미힌디. `target` 과 `action` 은 어떤 메소드를 실행해야 할지 알려주는 역할

  `#selector`는 shareTapped라는 메소드가 존재하며 버튼을 누를 때 실행되어야 한다고 컴파일에 알려주는 역할

- `shareTapped()`추가

  ```swift
  @objc func shareTapped() {
      guard let image = imageView.image?.jpegData(compressionQuality: 0.8) else {
          print("No image found")
          return
      }
  
      let vc = UIActivityViewController(activityItems: [image], applicationActivities: [])
      vc.popoverPresentationController?.barButtonItem = navigationItem.rightBarButtonItem
      present(vc, animated: true)
  }
  
  ```

  `@objc` 는 메소드가 Objective C 기반에서 호출될수 있도록 설정, `#selector`로 호출하려면 붙여야한다 
  
  `UIImage.jpegData(compreessionQuality:)` 는 UIImage를 jpeg로 화질(최대 1.0, 최소 0.0)으로 설정하여 보낼 수 있다.
  
  `UIActtivityViewController(activityItems:, applicationActivities:)` 는 다을 어플과 데이터를 공유 할 수 있게 해준다
  
  `vc.popoverPresentationController?.barButtonItem`는 `UIActivityViewController` 의 위치 장소를 결정 iPone에서는 화면 중앙에 위치하지만 아이패드에서 보면 화면오른쪽 버튼에 보인다

### 앨범에 저장 권한 넣기

- `Info.plist` 에서 권한 수정

  `Privacy - Photo Library Additions Usage Description` 추가하기 오른쪽 value에 앨범으로 어떤것을 할지 입력할 수 있다

## 정보

[https://www.hackingwithswift.com/100/22]

[swift-image]:https://img.shields.io/badge/swift-5-orange.svg
[swift-url]:https://swift.org
