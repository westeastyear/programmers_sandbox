# [Swift] 프로그래머스(Lv2) - 주차 요금 계산

[문제출처 프로그래머스 - 주차 요금 계산](https://school.programmers.co.kr/learn/courses/30/lessons/92341)

# 나의 풀이

```swift
func convertToMinutes(_ str: String) -> Int {
    let arr = str.components(separatedBy: ":")
    return (Int(arr[0])! * 60) + Int(arr[1])!
}

func calculateFee(_ fees: [Int], _ usageTime: Int) -> Int {
    let defaultTime = fees[0]
    let defaultFee = fees[1]
    let unitTime = fees[2]
    let unitFee = fees[3]
    
    if usageTime <= defaultTime {
        return defaultFee
    } else {
        return defaultFee + Int(ceil(Double(usageTime - defaultTime) / Double(unitTime))) * unitFee
    }
}

func solution(_ fees: [Int], _ records: [String]) -> [Int] {
    let arr = records.map { $0.components(separatedBy: " ") }
    var parkingLot = [String:Int]()
    var accumulatedParkingTime = [String:Int]()
    
    arr.forEach {
        let minutes = convertToMinutes($0[0])
        let carNumber = $0[1]
        
        if parkingLot[carNumber] == nil {
            parkingLot.updateValue(minutes, forKey: carNumber)
            if accumulatedParkingTime[carNumber] == nil {
                accumulatedParkingTime.updateValue(0, forKey: carNumber)
            }
        } else {
            let enteredTime = parkingLot[carNumber]!
            let usageTime = minutes - enteredTime
            
            accumulatedParkingTime[carNumber]! += usageTime
            parkingLot[carNumber] = nil
        }
    }
    
    while !parkingLot.isEmpty {
        parkingLot.forEach {
            let carNumber = $0.key
            let enteredTime = $0.value
            let usageTime = convertToMinutes("23:59") - enteredTime

            accumulatedParkingTime[carNumber]! += usageTime
            parkingLot[carNumber] = nil
        }
    }
    
    return accumulatedParkingTime.sorted(by: { $0.key < $1.key }).map { calculateFee(fees, $0.value)}
}
```

<br>

- `“HH:MM”` 형식으로 주어지는 시간 문자열을 분으로 변환하는 `convertToMinutes()` 메서드를 정의하였습니다.
    
```swift
// 리펙토링을 해본다면 이런식으로 바꿔볼 수 있겠다...!
    
func convertToMinutes(_ str: String) -> Int {
    let arr = str.components(separatedBy: ":").map { Int($0)! }
    return arr[0] * 60 + arr[1]
}
```

<br>

- 요금표 정보를 가지고 있는 `fees` 배열과, 차량의 누적 주차 시간 `usageTime`을 매개변수로 받는 `calculateFee()` 메서드를 정의하였습니다.
- 누적된 주차 시간이 기본시간보다 작거나 같다면 기본요금을 반환합니다. 그렇지 안다면 아래와 같은 공식을 통해 주차 요금을 계산하게 됩니다.
    
```swift
주차요금 = 기본요금 + 올림((누적 주차 시간 - 기본시간) / 단위시간) * 단위요금
```
    
- 소수점 올림을 하기 위해선 `ceil()` 메서드를 사용하게 되는데, `Double` 타입만 들어갈 수 있습니다.
- 소수점까지 나오는 나누기를 하기 위해선, 나누기 연산자(`”/”`) 양옆을 모두 `Double`타입으로 만들어 주어야 합니다.
- 처음에는 한쪽만 `Double`타입으로 만들었어서 값이 이상하게 나왔었는데, 오류를 발견하였고 반대쪽도 `Double`타입으로 만들어서 해결하였습니다.

<br>

- `records`의 요소들을 공백기준으로 나누어주고 이중배열로 반환합니다.
- 주차장이라는 이름의 `parkingLot` 딕셔너리와, 누적 주차 시간이라는 이름의 `accumulatedParkingTime` 딕셔너리를 활용하였습니다.
    
```swift
let arr = records.map { $0.components(separatedBy: " ") }
var parkingLot = [String:Int]()
var accumulatedParkingTime = [String:Int]()
```

<br>

- `arr`의 요소에 접근하고 인덱싱을 통해 `minutes`과 `carNumber`에 데이터를 할당합니다.
- `parkingLot`에 `carNumber`가 없다면 입차를 의미하므로, 값을 업데이트 해줍니다.
- `accumulatedParkingTime`에도 `carNumber`가 없다면 업데이트 해줍니다.
    - `if`문으로 감싼 이유는 중복 업데이트를 방지하기 위함합니다. 처음에 들어오고 나갔다가 다시들어온 경우를 예외처리 하였습니다.
- `parkingLot`에 `carNumber`가 있다면 출차를 의미하므로, 전에 입차한 시간과 현재 출차하는 시간의 차이를 구하고, `accumulatedParkingTime`에 주차장을 사용한 시간을 더해줍니다.
- `parkingLot[carNumber]` 에 `nil`을 할당합니다.
    
```swift
arr.forEach {
    let minutes = convertToMinutes($0[0])
    let carNumber = $0[1]
        
    if parkingLot[carNumber] == nil {
        parkingLot.updateValue(minutes, forKey: carNumber)
        if accumulatedParkingTime[carNumber] == nil {
            accumulatedParkingTime.updateValue(0, forKey: carNumber)
        }
    } else {
        let enteredTime = parkingLot[carNumber]!
        let usageTime = minutes - enteredTime
            
        accumulatedParkingTime[carNumber]! += usageTime
        parkingLot[carNumber] = nil
    }
}
```

<br>

- 어떤 차량이 입차된 후에 출차된 내역이 없다면, `“23:59”`에 출차된 것으로 간주한다는 조건이 있기 때문에 `parkingLot`에 남아있는 차량이 있는지 검증합니다.
- 차량이 있다면 입차된 시간과 `“23:59”`까지의 차이를 구해 `accumulatedParkingTime`에 사용한 시간을 더해줍니다.
- `parkingLot[carNumber]` 에 `nil`을 할당합니다.
    
```swift
while !parkingLot.isEmpty {
    parkingLot.forEach {
        let carNumber = $0.key
        let enteredTime = $0.value
        let usageTime = convertToMinutes("23:59") - enteredTime

        accumulatedParkingTime[carNumber]! += usageTime
        parkingLot[carNumber] = nil
    }
}
```

<br>

- “차량번호가 작은 자동차부터 차례로…”라는 조건에 맞게 `accumulatedParkingTime`요소의 `key`값에 있는 차량번호를 오름차순으로 정렬합니다.
- 요금표 정보를 가지고 있는 `fees` 배열과, 정렬된 요소의 `value`를 `calculateFee()`에 매개변수로 넣어 주차요금을 계산하고 계산된 배열을 반환합니다!
    
```swift
return accumulatedParkingTime.sorted(by: { $0.key < $1.key }).map { calculateFee(fees, $0.value) }
```
