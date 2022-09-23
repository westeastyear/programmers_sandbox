# [Swift] 프로그래머스(Lv2) - H-Index

[문제출처 프로그래머스 - H-Index](https://school.programmers.co.kr/learn/courses/30/lessons/42747)

# 다른 풀이

```swift
func solution(_ citations: [Int]) -> Int {
    let sorted = citations.sorted(by: >)
    
    for i in 0..<sorted.count {
        if i >= sorted[i] {
            return i
        }
    }
    return sorted.count
}
```

- 발표한 논문 `n`개중 `h`번 이상 인용된 논문이 `h`편 이상이고, 나머지 논문이 `h`번 이하로 인용되면 `h`의 최댓값이 답?!?!
- `citations`가 순차적으로 정렬이 되어 있다면 배열의 `index`가 `h-index`를 대변할 수 있습니다.
- 내림차순으로 정렬합니다. → `[6, 5, 4, 1, 0 ]`
    - h-index: 0, num: 6 → 인용횟수가 0 이상인 논문의 수: 5개, 인용 횟수가 0 이하인 논문의 수: 1개
    - h-index: 1, num: 5 → 인용횟수가 1 이상인 논문의 수: 4개, 인용 횟수가 1 이하인 논문의 수: 2개
    - h-index: 2, num: 4 → 인용횟수가 2 이상인 논문의 수: 3개, 인용 횟수가 2 이하인 논문의 수: 2개
    - h-index: 3, num: 1 → 인용횟수가 3 이상인 논문의 수: 3개, 인용 횟수가 3 이하인 논문의 수: 2개 → 성립
- 배열을 돌면서 `index`값이 인용 횟수보다 커지거나 같아지는 지점을 찾아주고, 해당 `index`값을 반환합니다.
- 조건문에 해당이 안되는 경우, 인용 횟수에 비해 논문의 수가 적은경우에 `h-index`는 논문의 수가 되게 됩니다.
