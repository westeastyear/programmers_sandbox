# [Swift] 프로그래머스(Lv2) - 2개 이하로 작은 비트

[문제출처 프로그래머스 - 2개 이하로 작은 비트](https://school.programmers.co.kr/learn/courses/30/lessons/77885)

# 나의 풀이

```swift
func solution(_ numbers: [Int64]) -> [Int64] {
    var result = [Int64]()
    
    for number in numbers {
        var targetBit = String(number, radix: 2)
        var count = number
        var signal = true

        while signal {
            count += 1
            let convert = String(count, radix: 2)
            
            if targetBit.count != convert.count {
                targetBit = String(repeating: "0", count: convert.count-targetBit.count) + targetBit
            }
            
            var defferentCount = 0
            for i in zip(targetBit, convert) {
                if i.0 != i.1 {
                    defferentCount += 1
                }
            }
            signal = defferentCount > 2
        }
        result.append(count)
    }
    return result
}
```

- 처음에는 1씩 더해가면서 가장 처음 등장하는 비트의 차이가 1이나 2인 숫자를 찾아내었습니다.
- 전부다 비교하면서 구하는 방법인데 당연하게도 시간초과가 났습니다.

# 다른 풀이

```swift
// 업뎃 예정
```
