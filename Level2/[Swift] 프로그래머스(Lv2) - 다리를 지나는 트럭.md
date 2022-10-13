# [Swift] 프로그래머스(Lv2) - 다리를 지나는 트럭

[문제출처 프로그래머스 - 다리를 지나는 트럭](https://school.programmers.co.kr/learn/courses/30/lessons/42583#qna)

# 나의 풀이

```swift
func solution(_ bridge_length: Int, _ weight: Int, _ truck_weights: [Int]) -> Int {
    var bridge = Array(repeating: 0, count: bridge_length)
    var trucks = truck_weights
    var bridgeWeight = 0
    var second = 0
    
    while !bridge.isEmpty {
        bridgeWeight -= bridge.removeFirst()
        second += 1
                
        if let truck = trucks.first {
            if truck + bridgeWeight <= weight {
                bridgeWeight += trucks.removeFirst()
                bridge.append(truck)
            } else {
                bridge.append(0)
            }
        }
    }
    return second
}
```

- `bridge_length`의 갯수만큼 `0`을 가지고 있는 `bridge`를 만들었습니다.
- `bridge`가 다 없어질 때까지 반복하게 됩니다.
- `trucks`의 첫번째 값을 바인딩 할 수 있다면 조건문을 실행하게 됩니다.
- `bridgeWeight`와 `truck`의 무게를 합친 값이, 다리의 제한무게인 `weight`이하라면 다리를 건널수 있으므로 `bridge`에 `truck`을 추가해줍니다.
- 제한무게를 초과하게 된다면 `bridge`에 `0`을 추가해줍니다.
- 한번의 반복마다 `+1`씩 추가되는 `second`를 반환합니다.
