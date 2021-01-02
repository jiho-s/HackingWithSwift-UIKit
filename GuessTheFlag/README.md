# Guess The Flag

> UIButton Custom, UIAlertController

[![Swift Version][swift-image]][swift-url]

## 목차

1. [Setting up](#setting-up)
2. [Designing your layout](#designing-your-layout)
3. [Making the basic game work](#making-the-basic-game-work)
4. [Guess which flag: random numbers](#guess-which-flag:-random-numbers)
5. [From outlets to actions: creating an IBAction](#from-outlets-to-actions:-creating-an-ibaction)
6. [Wrap up](#wrap-up)

## Setting up

- 프로젝트 생성

  1. iOS -> App 

  2. Product Name 및 Organization Identifier 설정

     Interface Storyboard 선택

  3. 저장 위치 선택


## Designing your layout

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

## Making the basic game work

- 변수 생성

  ```swift
  var contries = [String]()
  var score = 0
  ```

- 배열에 추가하기

  배열에 국가를 추가한다 `viewDidLoad()` 에 다음을 추가

  ```swift
  countries.append("estonia")
  countries.append("france")
  countries.append("germany")
  countries.append("ireland")
  countries.append("italy")
  countries.append("monaco")
  countries.append("nigeria")
  countries.append("poland")
  countries.append("russia")
  countries.append("spain")
  countries.append("uk")
  countries.append("us")
  ```

- Button에 이미지 추가하기

  ```swift
  func askQuestion() {
      button1.setImage(UIImage(named: countries[0]), for: .normal)
      button2.setImage(UIImage(named: countries[1]), for: .normal)
      button3.setImage(UIImage(named: countries[2]), for: .normal)
  }
  ```

- viewDidLoad()에 함수추가

  askQuestion을 추가한다 

- `CALayer`

  UIView의 layer프로퍼티로 Core Animation이 제공해준다

- 테두리 추가하기

  CALayer를 이용한 테두리 추가하기

  `viewDidLoad()` 에 다음을 추가

  ```
  button1.layer.borderWidth = 1
  button2.layer.borderWidth = 1
  button3.layer.borderWidth = 1
  ```

- 테두리에 색상 변경

  CALayer은 UIColor을 사용할 수 없고 CGColor를 사용할 수 있다. 따라서 UIColor를 CGColor로 바꾸어 사용한다.

  `viewDidLoad()` 에 다음을 추가한다

  ```swift
  button1.layer.borderColor = UIColor.lightGray.cgColor
  button2.layer.borderColor = UIColor.lightGray.cgColor
  button3.layer.borderColor = UIColor.lightGray.cgColor
  ```

  

# Guess which flag: random numbers

- 배열을 섞기

  `askQuestion()` 에 `shuffle()`을 이용해 배열을 섞는다

  ```swift
      func askQuestion() {
          countries.shuffle()
          button1.setImage(UIImage(named: countries[0]), for: .normal)
          button1.setImage(UIImage(named: countries[0]), for: .normal)
          button1.setImage(UIImage(named: countries[0]), for: .normal)
      }
  ```

- 정답 지정하기

  controller에 변수 `var correctAnswer = 0` 을 선언하고`Int.random(in:)` 을 이용해 랜덤 숫자를 만든다

  ```swift
      func askQuestion() {
          countries.shuffle()
          button1.setImage(UIImage(named: countries[0]), for: .normal)
          button1.setImage(UIImage(named: countries[0]), for: .normal)
          button1.setImage(UIImage(named: countries[0]), for: .normal)
          correctAnswer = Int.random(in: 0..<3)
      }
  ```

- 지정한 국가를 Title로 표시

  `askQuestion()` 마지막에 다음을 추가하여 Title을 설정한다

  ```swift
  title = countries[correctAnswer].uppercased()
  ```

# From outlets to actions: creating an IBAction

- Button의 IBAction 만들기

  스토리보드에서 버튼을 선택후 메소드 생성 Connection을 Action으로 Name을 `buttonTapped` Type을 `UIButton` 으로 설정한다.

  3개의 버튼을 모두 같은 메소드에 연결한다 

- 버튼을 구별하기

  같은 메서드에 버튼을 모두 연결 시켰으므로 구분을 해야한다 따라서 버튼을 선택후 attribute inspector로 이동해서 View에 tag를 0, 1, 2로 설정한다

- `buttonTapped()` 메소드 작성

  ```swift
  var title: String
  
  if sender.tag == correctAnswer {
      title = "Correct"
      score += 1
  } else {
      title = "Wrong"
      score -= 1
  }
  ```

- `UIAlertController`로 경고창 출력

  `buttonTapped()` 메소드 아래에 다음을 입력한다

  ```swift
  let ac = UIAlertController(title: title, message: "Your score is \(score).", preferredStyle: .alert)
  ac.addAction(UIAlertAction(title: "Continue", style: .default, handler: askQuestion))
  present(ac, animated: true)
  ```

  `preferredStyle`에는 `.alert` 와 `.actionSheet` 가 있으며 `.alert` 는 화면 중앙에 경고창을 띄워주며 `.actionSheet` 는 화면 아래에 경고창을 띄워준다

  `UIAlertAction` 은 경고창에 버튼을 추가시킨다

- `askQuestion()` 에 파라미터 추가

  UIAlertAction으로 클로저를 전달하려면 파라미터로 메소드에 파라미터로 `UIAlertAction` 이 있어야 한다.

  ```swift
      func askQuestion(action: UIAlertAction! = nil) {
          countries.shuffle()
          button1.setImage(UIImage(named: countries[0]), for: .normal)
          button1.setImage(UIImage(named: countries[0]), for: .normal)
          button1.setImage(UIImage(named: countries[0]), for: .normal)
          correctAnswer = Int.random(in: 0..<3)
      }
  ```


## Wrap up

### Challenge

- 사용자 점수 `navigationbar` 에 표시

  `askQuestion()` 부분에 `title` 설정하는 부분을 수정

  ```swift
  title = countries[correctAnswer].uppercased() + "   score: \(score)"
  
  ```

- 질문 개수를 기록하고 10개가 되면 최종 점수 출력

  `controller` 에 `var count = 0` 을 추가하고 `buttonTapped` 에 다음을 추가

  ```swift
      @IBAction func buttonTapped(_ sender: UIButton) {
          var title: String
  
          if sender.tag == correctAnswer {
              title = "Correct"
              score += 1
          } else {
              title = "Wrong"
              score -= 1
          }
          count += 1
          var ac: UIAlertController
          if count == 10 {
              ac = UIAlertController(title: "Final", message: "Your final score is \(score).", preferredStyle: .alert)
              count = 0
              score = 0
          } else {
              ac = UIAlertController(title: title, message: "Your score is \(score).", preferredStyle: .alert)
          }
          ac.addAction(UIAlertAction(title: "Continue", style: .default, handler: askQuestion))
          present(ac, animated: true)
      }
  ```

- 잘못된 국기를 눌렀을 때 정답 알려주기

  ```swift
      @IBAction func buttonTapped(_ sender: UIButton) {
          var title: String
  
          if sender.tag == correctAnswer {
              title = "Correct"
              score += 1
          } else {
              title = "Wrong That’s the flag of \(countries[sender.tag].uppercased())"
              score -= 1
          }
          //...
       }
  ```

## 정보

[https://www.hackingwithswift.com/100/19]

[swift-image]:https://img.shields.io/badge/swift-5-orange.svg
[swift-url]:https://swift.org