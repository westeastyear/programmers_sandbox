# [Swift] 프로그래머스(Lv2) - 최댓값과 최솟값

[문제출처 프로그래머스 - 최댓값과 최솟값](https://school.programmers.co.kr/learn/courses/30/lessons/12939)

# 나의 풀이

```swift
func solution(_ s: String) -> String {
    let arr = s.components(separatedBy: " ").map { Int($0)! }
    let min = String(arr.min()!)
    let max = String(arr.max()!)
    return "\(min) \(max)"
}
```

- 코딩테스트 문제를 공부하면서 이런저런 메서드를 많이 알게 되는거 같다.
- 굳이 상수를 만들지 않아도 되지만, 코드에 정답은 없으니깐

# 리펙토링

```swift
func solution(_ s: String) -> String {
    let arr = s.components(separatedBy: " ").map { Int($0)! }
    return "\(arr.min()!) \(arr.max()!)"
}
```

- 상수를 없애는것도 좋은 방법인거 같다!
