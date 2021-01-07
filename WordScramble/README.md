# Word Scramble

> FileManager로 사진 이름 조회후 사진 표시

[![Swift: 5][swift-image]][swift-url]

## 목차

1. [Capture lists in Swift: what's the difference between weak, strong, and unowned references](#capture-lists-in-swift)
2. [Setting up](#setting-up)

## Capture lists in Swift

먼저 다음과 같은 간단한 클래스가 있다.

```swift
class Singer {
    func playSong() {
        print("Shake it off!")
    }
}
```

다음으로 `Singer` 인스턴스를 만들고 `Singer`의 `playSong()`메소드를 호출하는 함수를 작성하자. 함수는 클로저를 리턴한다.

```swift
func sing() -> () -> Void {
    let taylor = Singer()

    let singing = {
        taylor.playSong()
        return
    }

    return singing
}
```

`sing()`을 다음과 같이 사용할 수 있다.

```swift
let singFunction = sing()
singFunction()
```

### Strong capturing

기본 값으로 Swift는 Strong capturing을 사용한다. 클로져는 클로져 내부에서 사용되는 모든 외부 값을 캡쳐하고, 절대 파괴되지 않도록한다는 것을 의미한다.

```swift
func sing() -> () -> Void {
    let taylor = Singer()

    let singing = {
        taylor.playSong()
        return
    }

    return singing
}
```

`taylor`는 `sing()` 함수 안에서 생성되므로, 일반적으로 함수가 끝나면 소멸된다. 그러나,  클러져 내부에서 사용되었기 때문에 Swift는 함수가 리턴된 이후에도 자동적으로 클로져가 존재하는한 존재하게 한다.

### Week capturing

클로저 내부에서 사용된 값이 어떻게 캡쳐되어야하는지 정하기 위한 캡처 목록을 지정할 수 있다. Strong capturing과 두가지 차이점이 있다.

1. 약하게 캡처가 된 값을 클로저에 의해 유지되지 않으므로 파괴 될수 있고 nil로 설정될 수 있다.
2. 약하게 캡처가 된값은 1의 이유로 인해 `optional`이다. 따라서 존재한다고 추측하면 안된다.

```swift
func sing() -> () -> Void {
    let taylor = Singer()

    let singing = { [weak taylor] in
        taylor?.playSong()
        return
    }

    return singing
}
```

`[weak taylor]`부분이 캡처 리스트 이며, 값을 챕처하는 방법을 제공하는 클로저의 문법이다.

`singFunction()`을 사용하면 어떤것도 출력되지 않음을 알 수 있는데. 반환하는 클로져가 `taylor`가 `sing()`안에서만 존재하기 때문이다.

### Unowned capturing

`weak`와 비슷하게 캡처된 값이 nil이 되게 하지만, 암시적으로 래핑안된 optional 값처럼 사용한다. 그러므로 optional을 해제하지 않아도 된다.

```swift
func sing() -> () -> Void {
    let taylor = Singer()

    let singing = { [unowned taylor] in
        taylor.playSong()
        return
    }

    return singing
}
```

`singFunction()`을 사용하면 충돌이 발생한다.

### 일반적인 문제

클로저 캡처를 사용할때 발생하는 문제가 있다.

- 클로저에 매개변수가 있을때, 캡처 리스트가  어떤 것인지 모른다
- 강한 참조 사이클을 만들어 메모리 누수를 발생시킨다
- 여러 캡처를 사용할때 실수로 강한 참조 사용
- 클로저를 여러개 복사하고 데이터를 공유

#### 매개변수와 캡쳐리스트 동시사용

캡쳐 목록과 매개변수를 사용하는 경우 챕쳐 리스트가 항상 먼저 와야하며 `in`이 클로저 바디의 시작부분을 표시해야한다. 클로저 매개변수 뒤에 캡쳐리스트를 사용하면 컴파일이 중지된다. 

```swift
writeToLog { [weak self] user, message in 
    self?.addToLog("\(user) triggered event: \(message)")
}
```

#### Strong reference cycles

```swift
class House {
    var ownerDetails: (() -> Void)?

    func printDetails() {
        print("This is a great house.")
    }

    deinit {
        print("I'm being demolished!")
    }
}
```

`Hoese`는 하나의 프로퍼티, 하나의 메소드 그리고 `deinit`을 사용하여 파괴될때 메시지를 출력한다.

```swift
class Owner {
    var houseDetails: (() -> Void)?

    func printDetails() {
        print("I own a house.")
    }

    deinit {
        print("I'm dying!")
    }
}
```

`Owner`도 하나느이 프로퍼티, 메소드 그리고 `deinit`을 사용하여 파괴될때 메시지를 출력한다.

`do`블록을 사용해서 인스턴스를 생성해보자

```swift
print("Creating a house and an owner")

do {
    let house = House()
    let owner = Owner()
}

print("Done")
```

`do` 블록을 나오면 `deinit`이 실행되는 것을 확인 할 수 있다.

강한 참조 사이클을 만들어 보자

```swift
print("Creating a house and an owner")

do {
    let house = House()
    let owner = Owner()
    house.ownerDetails = owner.printDetails
    owner.houseDetails = house.printDetails
}

print("Done")
```

이를 실행하면 `deinit`이 실행되지 않는 것을 확인 할 수 있는데 서로 순환참조가 발생하여 메모리가 해제되지 않고 메모리 누수를 사용한다.

이것을 해결하기 위해 약한 참조를 사용할 수 있다.

```swift
print("Creating a house and an owner")

do {
    let house = House()
    let owner = Owner()
    house.ownerDetails = { [weak owner] in owner?.printDetails() }
    owner.houseDetails = { [weak house] in house?.printDetails() }
}

print("Done")
```

두개다 약한 참조일 필요는 없고 하나만 설정하면된다.

#### 우발적인 강한 참조

swift는 기본적으로 강한 참조를 사용하므로 의도하지 않는 문제가 발생할 수 있다.

```swift
func sing() -> () -> Void {
    let taylor = Singer()
    let adele = Singer()

    let singing = { [unowned taylor, adele] in
        taylor.playSong()
        adele.playSong()
        return
    }

    return singing
}
```

`adele`은 강한 참조고 `taylor`는 unowned 참조이다. 

#### 클로저 복사

복사본들 사이에 캡처된 데이터가 공유된다.

다음 예에서 클로저는 호출될때마다 값을 증가시키고 출력할 수 있도록 외부에서 생성 된 Int `numberOfLinesLogged` 를 캡처하는 클러저 이다.

```swift
var numberOfLinesLogged = 0

let logger1 = {
    numberOfLinesLogged += 1
    print("Lines logged: \(numberOfLinesLogged)")
}

logger1()
```

```swift
let logger2 = logger1
logger2()
logger1()
logger2()
```

`logger2`를 사용해도 `numberOfLinesLogged`가 증가하는것을 확인 할 수 있다.

### 결론

- 클로저가 사용될 동안 캡쳐된 값이 사라지지 않을 것을 알고 있으면 `unowned`를 사용할 수 있다.  사라질수 있는 경우에는 `weak`를 사용하여 `guard let`을 사용하여 옵셔널을 해제한다.
- 강한 참조 사이클이 생성되는 경우 둘중 하나는 약한 캡처를 사용해야한다. 그중에서 먼저 사라지는 것을 약한 캡처를 사용한다
- 강한 참조 순환이 발생하지 않는 경우 강한  캡처를 사용할 수 있다.

## Setting up

## 정보

[https://www.hackingwithswift.com/100/16]

[swift-image]:https://img.shields.io/badge/swift-5-orange.svg
[swift-url]:https://swift.org
