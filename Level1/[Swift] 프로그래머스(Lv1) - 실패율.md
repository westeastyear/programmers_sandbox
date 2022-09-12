# [Swift] 프로그래머스(Lv1) - 실패율

[문제출처 프로그래머스 - 실패율](https://school.programmers.co.kr/learn/courses/30/lessons/42889?language=swift#)

# 나의 코드

```swift
func solution(_ N: Int, _ stages: [Int]) -> [Int] {
    var point = [Int:Double]()
    
    for i in 1...N {
        let a = stages.filter { i == $0 }.count
        let b = stages.filter { i <= $0 }.count
        let c = Double(a) / Double(b)
        point.updateValue(c, forKey: i)
    }
    let d = point.sorted(by: <).sorted(by: { $0.value > $1.value })
    return d.map { $0.key }
}
```

- 딕셔너리 요소에 추가하기 `updateValues(_:_:)`
- 무지성으로 코드를 작성하다 보니 시간초과가 나왔다.
- 반복문 안에서 `filter`로 갯수를 세주는게 시간이 오래걸린다는 것을 알게 되었다.
- 밖에서 미리 `count`를 해주면 괜찮아진다는 정보를 캐치하고 해보도록 하겠다.

# 나의 풀이 2

```swift
func solution(_ N: Int, _ stages: [Int]) -> [Int] {
    var point = [Int:Double]()
    var players = stages.count
    
    for i in 1...N {
        let n = stages.filter { i == $0 }.count
        point[i] = Double(n) / Double(players)
        players -= n
    }
    return point.sorted(by: <).sorted(by: { $0.value > $1.value }).map { $0.key }
}
```

- 또 시간초과가 뜬다…ㅠ
- `Dictionary` → `tuple`, `filter` → `for-loop`하라는 조언을 알게 되었다.

# 다른 풀이

```swift
func solution(_ N: Int, _ stages: [Int]) -> [Int] {
    var point = [(Int, Double)]()
    var players = stages.count
    
    for i in 1...N {
        var current = 0
        for j in 0..<stages.count {
            if stages[j] == i {
                current += 1
            }
        }
        let rate = Double(current) / Double(players)
        point.append((i, rate))
        players -= current
    }
    return point.sorted(by: { $0.1 > $1.1 }).map { $0.0 }
}
```

- `tuple`과 `for-loop`를 사용한 풀이이다.
- 어색한 튜플이지만 사용방법은 쉬웠다.
- 그런데 시간이 엄청나게 빨라지는 풀이를 발견하게 되는데?!?!

# 다른 풀이 from dudu

[출처 - 두두 블로그](https://velog.io/@aurora_97/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EC%8B%A4%ED%8C%A8%EC%9C%A8-Swift)

```swift
func solution(_ N:Int, _ stages:[Int]) -> [Int] {
    var clear = Array(repeating: 0, count: N+2)
    var stop = Array(repeating: 0, count: N+2)
    
    for stage in stages {
        stop[stage] += 1
        clear[stage - 1] += 1
    }
    
    for i in stride(from: N, through: 0, by: -1) {
        clear[i] += clear[i+1]
    }
    
    return zip(clear[1...], stop[1...]).enumerated()
        .map { (index: Int, value: (c: Int, s: Int)) -> (Int, Double) in
            let total = value.c + value.s
            
            if total == 0 {
                return (index+1, 0)
            } else {
                return (index+1, Double(value.s) / Double(total))
            }
        }
        .dropLast()
        .sorted {
            if $0.1 == $1.1 {
                return $0.0 < $1.0
            } else {
                return $0.1 > $1.1
            }
        }
        .map { $0.0 }
}
```

- 어마무시한 코드를 발견해버렸다!!
- 겁나게 어렵다…ㅠㅠ 나중에 도전해보자

# 누적합이란?

[누적합 유튜브 강의](https://www.youtube.com/watch?v=GnG4-Et7GkY)

- 강의를 통해 찍먹해 보았다.
- 새로운 개념을 알게 되었다!
- 개념은 간단한거 같다.
