# [Swift] 프로그래머스(Lv2) - 두 큐 합 같게 만들기

[문제출처 프로그래머스 - 두 큐 합 같게 만들기](https://school.programmers.co.kr/learn/courses/30/lessons/118667)

# 나의 풀이

```swift
extension Array where Element == Int {
    var sum: Int {
        return self.reduce(0, +)
    }
}

func solution(_ queue1: [Int], _ queue2: [Int]) -> Int {
    var queue1 = queue1
    var queue2 = queue2
    var count = 0

    while queue1.sum != queue2.sum {
        if queue1.isEmpty || queue2.isEmpty { return -1 }
        count += 1
        
        if queue1.sum < queue2.sum {
            queue1.append(queue2.removeFirst())
        } else {
            queue2.append(queue1.removeFirst())
        }
    }
    return count
}
```

- 문제설명을 정직하게 구현해낸 풀이라고 할 수 있겠다. 하지만 시간초과가…(크흡)

# 리펙토링

```swift
func solution(_ queue1: [Int], _ queue2: [Int]) -> Int {
    var queue1 = queue1
    var queue2 = queue2
    var queue1Sum = queue1.reduce(0, +)
    var queue2Sum = queue2.reduce(0, +)
    var count = 0

    while queue1Sum != queue2Sum {
        if queue1.isEmpty || queue2.isEmpty { return -1 }
        count += 1
        
        if queue1Sum < queue2Sum {
            let value = queue2.removeFirst()
            queue1Sum += value
            queue2Sum -= value
            queue1.append(value)
        } else {
            let value = queue1.removeFirst()
            queue1Sum -= value
            queue2Sum += value
            queue2.append(value)
        }
    }
    return count
}
```

- 한번 반복할 때마다 `reduce()` 연산하는것을 한번만 하도록 수정하고, `dequeue`된 값들을 더하고 빼주는 식으로 리펙토링 하였으나 실패...
