# [Swift] 프로그래머스(Lv2) - 최솟값 만들기

[문제출처 프로그래머스 - 최솟값 만들기](https://school.programmers.co.kr/learn/courses/30/lessons/12941)

# 나의 풀이

```swift
func solution(_ A: [Int], _ B: [Int]) -> Int {
    var result = 0
    let a = A.sorted(by: <)
    let b = B.sorted(by: >)
    
    for i in 0..<a.count {
        result += a[i] * b[i]
    }
    
    return result
}
```

- 여러 경우의 수에서 결국 최솟값을 얻기 위해서는 배열 `A`의 최솟값과 배열 `B`최댓값이 곱해져야 하고, 배열 `A`의 최댓값과 배열 `B`의 최솟값이 곱해져야 한다.
- 배열 `A`를 오름차순으로 정렬하고, 배열 `B`를 내림차순으로 정렬한다.(반대의 경우도 가능하다)
- 정렬된 `a`와 `b`를 같은 인덱스 번호로 곱해주면서 `result`와 더해주는 것을 반복한다.
- 최솟값 완성!
