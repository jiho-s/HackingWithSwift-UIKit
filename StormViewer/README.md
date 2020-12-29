# StormViewer

> 

[![Swift Version][swift-image]][swift-url]

## 목차

[1. Setting up](#1.-setting-up)
[2. Listing images with FileManager](#2.-listing-images-with-filemanager)
[3. Designing our interface](#3.-designing-our-interface)
[4. Building a detail screen](#4.-building-a-detail-screen)

## 1. Setting up

- 프로젝트 생성

  1. iOS -> App 

  2. Product Name 및 Organization Identifier 설정

     Interface Storyboard 선택

  3. 저장 위치 선택

- 이미지 넣기

  - Copy Items if needed 체크
  - Create Groups 체크
  - Add to targets에 선택

## 2. Listing images with FileManager

- UIViewController

   애플의 기본 스크린 타입

  - viewDidLoad()

    view가 메모리에 로드 되었을 때 호출되는 함수

    view에 필요한 파일을 로드하기 등 view초기화시 사용

- FileManager

  - default

    싱글톤 FileManger 인스턴스 제공

  - contentsOfDirectory(atPath:)

    설정한 경로의 디렉토리 안에 있는 파일들을 [String]으로 반환

- Bundle

  실행 가능한 코드와 그 코드가 사용하는 자원을 포함

  - main

    현재 실행중인 디렉토리의 자원에 접근할 수 있다

    - resourcePath

      Bundle의 리소스에 접근할 수 있는 서브디렉토리 경로

## 3. Designing our interface

- UITableViewController

  

- StoryBoard에서 ViewController를 TableViewController로 교체

  - 기존 ViewController삭제 및 TableViewController추가

  - TableViewController에서 Class 선택

    Identity inspector로 이동해서 클래스에 기존 ViewController.swift 파일 선택

  - Initial ViewController로 지정

    Attributes Inspector로 이동해서 InitalViewController에 체크

  - NavigationController 추가

    Editer -> Embedded In 에 NavigationController 추가

  - Cell 설정

    - Style 설정

      Attributes Inspector로 이동해서 Style을 Basic으로 변경

    - Identifier 설정

      Attributes Inspector로 이동해서 Identifier을 Picture로 변경

- ViewController.swift에서 cell 표시하기

  - tableView(_ tableView: numberOfRowsInSection:)

    tableView의 row의 개수를 설정

  - tableView(**_** tableView: UITableView, cellForRowAt indexPath: IndexPath)

    Cell 각각에 어떤 데이터를 넣을 건지

    - tableView.dequeueReusableCell(withIdentifier:String, for:IndexPath)

      iOS에서는 cell을 화면에 표시되는 개수만큼만 생성하고 재활용 해서 사용한다 그 재활용하는 cell을 가져오기 위한 메소드

## 4. Building a detail screen

- DetailViewController.swift 만들기
  - Menu->file->iOS->Cocoa Touch Class 선택
  - DetailViewController로 이름 설정
  - Subclass Of에 UIViewController 선택, Also create XIB file은 선택하지 않음

- Main.storyboard파일에 추가하기
  - View Controller 추가
  - Identity inspector로 이동해서 클래스에 기존 DetailViewController.swift 파일 선택
  - Identity Inspector로 이동해서 Storyboard Id에 Detail로 설정

- UIImageVIew 추가하기

  - Object에서 UIImageView 추가하기

    추가한후 화면크기 꽉차게

- Auto Layout 설정
  - Editor->Resolve Auto Laout issues -> Reset To Suggested Constraints

- Image VIew를 코드와 연결
  - ^ + option + command + return 키를 이용해 Assistant Editor를 연다
  - ImageView를 클래스 안으로 넣고 이름을 imageView strong으로 설정
    - @IBOutlet : Interface Builder와 연결되어 있음을 알려준다

## 정보

[https://www.hackingwithswift.com/100/16]

[swift-image]:https://img.shields.io/badge/swift-5-orange.svg
[swift-url]:https://swift.org
