# [Swift] 프로그래머스(Lv1) - 내적

[문제출처 프로그래머스 - 내적](https://school.programmers.co.kr/learn/courses/30/lessons/70128)

# 나의 풀이

```swift
func solution(_ a: [Int], _ b: [Int]) -> Int {
    return (0..<a.count).reduce(0, { $0 + (a[$1] * b[$1]) })
}
```
- 다른 풀이들 중에 `zip()` 사용하더라
- `zip()`을 사용해보자

# 다른 풀이

```swift
func solution(_ a: [Int], _ b: [Int]) -> Int {
    return zip(a, b).map { $0 * $1 }.reduce(0, +)
}
```
- `zip()`을 사용하면 두개의 배열을 받아서 튜플로 처리할수가 있다?!
- 이거 아주 좋군!
