# [Swift] 프로그래머스(Lv2) - [3차] n진수 게임

[문제출처 프로그래머스 - [3차] n진수 게임](https://school.programmers.co.kr/learn/courses/30/lessons/17687)

# 나의 풀이

```swift
func solution(_ n: Int, _ t: Int, _ m: Int, _ p: Int) -> String {
    var result = ""
    var temp = ""
    var number = 0
    
    while temp.count < t*m {
        temp += String(number, radix: n).uppercased()
        number += 1
    }
    
    let arr = temp.map { String($0) }
    var index = p-1
    while result.count < t {
        result.append(arr[index])
        index += m
    }
    
    return result
}
```

- 시간초과…

# 리펙토링

```swift
func solution(_ n: Int, _ t: Int, _ m: Int, _ p: Int) -> String {
    var result = ""
    var temp = ""
    
    for i in 0..<t*m {
        temp += String(i, radix: n, uppercase: true)
    }
    
    let arr = Array(temp)
    for i in 0..<t {
        let index = i * m + p - 1
        result.append(arr[index])
    }
    return result
}
```

- `for`문이 짱이다!
- `String(i, radix: n, uppercase: true)` 대문자화까지 지원되는줄은 몰랐는데, 이번에 강력한 친구라는걸 다시금 느끼게 되었다.
