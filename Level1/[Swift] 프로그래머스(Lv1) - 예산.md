# [Swift] 프로그래머스(Lv1) - 예산

[문제출처 프로그래머스 - 예산](https://school.programmers.co.kr/learn/courses/30/lessons/12982)

# 나의 풀이

```swift
func solution(_ d: [Int], _ budget: Int) -> Int {
    
    var count = 0
    var sum = 0
    let sorted = d.sorted()
    
    for i in 0..<sorted.count {
        sum += sorted[i]
        guard sum <= budget else { return count }
        count += 1
    }
    return count
}
```

- 논리상으로는 맞는 코드이지만 뭔가 아쉽다…

# 다른 풀이

```swift
func solution(_ d: [Int], _ budget: Int) -> Int {
    var budget = budget
    
    return d.sorted().filter {
        budget = budget - $0
        return budget >= 0
    }.count
}
```

- 좋아요를 많이 받은 깔끔한 코드이다
- 받은 배열을 정렬하고 `filter`를 해준다.
- 전체예산(`budget`)에서 정렬된 배열의 작은 요소부터 가감후 바로 할당해줌으로서 `budget`을 업데이트 한다.
- 0보다 같거나 큰 `budget` 일때 각 요소의 `Bool`값을 반환한다.
- `filter`된 배열에서 `true`의 갯수를 반환한다.👏👏👏
