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

---

# 투포인터 알고리즘(Two Pointers Algorithm)

- 투포인터 알고리즘(Two Pointers Algorithm) 또는 슬라이딩 윈도우(Sliding Window)라고 부릅니다.
- 1차원 배열이 있고, 이 배열에서 각자 다른 원소를 가리키고 있는 2개의 포인터를 조작해가면서 원하는 것을 얻는 형태입니다.

# 다른 풀이

```swift
func solution(_ queue1: [Int], _ queue2: [Int]) -> Int {
    let merged = queue1 + queue2
    let half = queue1.count
    var sum1 = queue1.reduce(0, +)
    var sum2 = queue2.reduce(0, +)
    var answer = 0
    
    var q1 = 0
    var q2 = half
    
    func addIndex(_ idx: Int) -> Int {
        return idx+1 == 2*half ? 0 : idx+1
    }
    
    while answer <= 4*half {
        if sum1 > sum2 {
            sum1 -= merged[q1]
            sum2 += merged[q1]
            q1 = addIndex(q1)
        } else if sum1 < sum2 {
            sum1 += merged[q2]
            sum2 -= merged[q2]
            q2 = addIndex(q2)
        } else {
            return answer
        }
        answer += 1
    }
    return -1
}
```

- 입력으로 주어지는 두개의 `queue`배열을 하나의 배열로 합치고, 각 `queue`배열의 시작부분에 포인터를 두어 구분 짓도록 하였습니다.
- 두 배열의 합을 같게 하기 위해서는, 배열의 합이 큰 배열에서 작은 배열로 값을 넘겨주어야 합니다.
    - 합이 큰 배열 `dequeue` → 합이 작은 배열 `inqueue`
- 큐 자료구조의 `dequeue`, `inqueue`를 위해 `removeFirst()`, `append()`를 하게 되면 시간초과가 나기 때문에 첨자접근을 활용하여 각 `queue` 요소들을 미리 더해두었던 `sum`값에 `+`, `-`를 해줍니다.
- 인덱스 값이 증가하여 `merged`된 큐의 갯수와 같아지게 되면 다시 `0`으로 초기화 해줍니다.
    - `addIndex()` 메서드 정의!
- 배열의 포인터가 처음상태와 같아지는 상태까지 반복해줍니다. 이는 `4*half(한 queue배열의 길이)` 만큼 움직였을 때 입니다.
    - 이동 횟수가 `4*half`를 넘어간다면 반복을 종료하고 `-1`를 반환합니다.
