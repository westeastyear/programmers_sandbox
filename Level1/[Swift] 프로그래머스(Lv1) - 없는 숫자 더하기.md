# [Swift] 프로그래머스(Lv1) - 없는 숫자 더하기

[문제출처 프로그래머스 - 없는 숫자 더하기](https://school.programmers.co.kr/learn/courses/30/lessons/86051)

# 나의 풀이

```swift
func solution(_ numbers: [Int]) -> Int {
    return (1...9).filter { !(numbers.contains($0)) }.reduce(0, +)
}
```
- 깔끔하게 하려고 노력하다 보니 계속 발전하는거 같다.
- 이번에는 좋아요를 많이 받은 코드와 완전 일치하게 작성하였다!!

# 다른 풀이

```swift
func solution(_ numbers: [Int]) -> Int {
    return 45 - numbers.reduce(0, +)
}
```
- 재밌는 풀이다! 발상의 전환이랄까? 
