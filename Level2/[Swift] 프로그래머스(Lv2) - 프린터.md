# [Swift] 프로그래머스(Lv2) - 프린터

[문제출처 프로그래머스 - 프린터](https://school.programmers.co.kr/learn/courses/30/lessons/42587)

# 나의 풀이

```swift
func solution(_ priorities: [Int], _ location: Int) -> Int {
    var tuples = [(Int, Int)]()
    var sorted = [(Int, Int)]()
    var answer = 0
    
    for (index, value) in priorities.enumerated() {
        tuples.append((value, index))
    }
        
    while !tuples.isEmpty {
        let first = tuples.removeFirst()
        
        if tuples.contains(where: { first.0 < $0.0 }) {
            tuples.append(first)
        } else {
            sorted.append(first)
        }
    }
    
    for i in sorted.indices {
        if sorted[i].1 == location {
            answer = i + 1
        }
    }
    
    return answer
}
```

- 반복문을 돌리고 튜플을 활용하여 `tuples` 배열에 `(중요도, 인덱스)` 형식으로 저장해 주었습니다.
- 중요도 순으로 정렬을 해보겠습니다
    - 맨앞 요소의 중요도를 기준으로 더 큰 중요도를 가진 요소가 뒤에 있다면 맨앞의 요소를 뒤로 옮겨주면서 제일 큰 중요도를 가진 요소를 찾아갑니다.
    - 찾았다면 `sorted`에 넣어줍니다.
- 중요도 순으로 정렬된 `sorted` 배열을 돌면서 `sorted[i].1`에 `location`과 같은 요소를 찾게 되면 해당 인덱스에 `+1`한 값을 반환하도록 합니다.
