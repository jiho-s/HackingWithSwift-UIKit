# Guess The Flag

> FileManager로 사진 이름 조회후 사진 표시

[![Swift Version][swift-image]][swift-url]

## 목차

1. [Setting up](#1.-setting-up)
2. [Designing your layout](#2.-designing-your-layout)

## 1. Setting up

- 프로젝트 생성

  1. iOS -> App 

  2. Product Name 및 Organization Identifier 설정

     Interface Storyboard 선택

  3. 저장 위치 선택


## 2. Designing your layout

- NavigationController 추가

  Embed in -> Navigation Controller

  

- Button 추가

  - 버튼 3개 추가 및 200*100 사이즈 설정

    Size Inspector에서 width, Height로 설정 가능

  - 버튼 3개의 y position을 100 230 360에 각각 배치

  - Auto Layout 설정하기

    - 첫번째 버튼 position 설정

      Control 을 누르고 밖으로 드래그

      shift를 누루고 Top Space To safe Area, Center Horizontally in Safe Area 선택

    - 두번째 버튼 position 설정

      control을 누르고 첫번쨰 버튼으로 드래그

      shift를 누루고 Vertical Spacing, CenterHorizontally 체크

    - 세번째 버튼 position 설정

      두번째 버튼으로 드래그

      두번째 버튼 처럼 설정

    - Editer -> Resolve Auto Layout Issues -> Update Frames

  - OutLet 추가하기

    - control + option + command + return으로 Assistant Editor 활성화
    - button1, button2, button3 추가

- 이미지 추가하기

  - Assets.xcassets에 이미지 추가하기

    이미지가 @2x, @3x 로 나누어져 있는데 AppStore에서 다운시 각각 기기에 맞는 해상도의 이미지만 다운로드 한다

## 정보

[https://www.hackingwithswift.com/100/19]

[swift-image]:https://img.shields.io/badge/swift-5-orange.svg
[swift-url]:https://swift.org