# [Swift] 프로그래머스(Lv2) - 기능개발

[문제출처 프로그래머스 - 기능개발](https://school.programmers.co.kr/learn/courses/30/lessons/42586)

# 나의 풀이

```swift
func solution(_ progresses: [Int], _ speeds: [Int]) -> [Int] {
    var days = 0
    var progresses = progresses
    var daysArr = [Int]()
    var result = [Int]()
    
    for i in 0..<progresses.count {
        days = 0
        while progresses[i] < 100 {
            progresses[i] += speeds[i]
            days += 1
        }
        daysArr.append(days)
    }
    
    while !daysArr.isEmpty {
        var count = 1
        let temp = daysArr.removeFirst()
        
        while !daysArr.isEmpty && temp >= daysArr.first! {
            daysArr.removeFirst()
            count += 1
        }
        result.append(count)
    }
    return result
}
```

- 각각의 `progresses`에 맞게 `speeds`를 더해주면서 기능개발에 필요한 일수를 구해줍니다.
- `daysArr`의 일수에 접근하여 첫 일수와 다음에 오는 일수를 비교합니다.
- `removeFirst`를 하며 반복하고 다음의 오는 일수가 큰 값이 올때까지 `count`를 `+1`해줍니다.
- 배포별 기능의 개수가 포함된 `result`를 반환합니다.
- 그런데 배열의 `removeFirst()` 연산은 큐의 `pop()` 과 달리 `O(N)`의 복잡도를 가지며 이를 최대 `N`번 반복 수행한다고 한다.

# 다른 풀이

```swift
func solution(_ progresses: [Int], _ speeds: [Int]) -> [Int] {
    var daysArr = [Int]()
    var result = [Int]()
    
    for i in progresses.indices {
        let progress = Double(100 - progresses[i])
        let speed = Double(speeds[i])
        daysArr.append(Int(ceil(progress / speed)))
    }
    
    var cur = 0
    while cur < daysArr.count {
        let nowDelay = daysArr[cur]
        var count = 0
        
        while cur < daysArr.count && daysArr[cur] <= nowDelay {
            count += 1
            cur += 1
        }
        result.append(count)
    }
    return result
}
```

- `ceil()` 메서드를 활용하여 나눈 몫을 올린값으로 활용하게 하였습니다.
- 배열큐와 커서를 활용한 코드입니다.
- `first!` 프로퍼티와 `removeFirst()` 메서드사용이 없어지고, 대신 `cur`라는 변수를 사용하고 있습니다.
- 배열의 첨자 접근 소요 시간복잡도는 `O(1)`이기 때문에 `removeFirst()` 메서드 사용시보다 효율적인 시간복잡도 성능을 보이게 됩니다.
- 나중에도 단순 배열큐를 사용할때, `removeFirst()` 등의 메서드 사용에는 많은 고민을 해야 할거 같습니다!
