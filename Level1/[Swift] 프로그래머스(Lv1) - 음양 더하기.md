# [Swift] 프로그래머스(Lv1) - 음양 더하기

[문제출처 프로그래머스 - 음양 더하기](https://school.programmers.co.kr/learn/courses/30/lessons/76501)

# 나의 풀이

```swift
func solution(_ absolutes: [Int], _ signs: [Bool]) -> Int {
    return (0..<absolutes.count).reduce(0, { $0 + (signs[$1] == true ? absolutes[$1] : -(absolutes[$1])) })
}
```
- 사람들이 좋아요를 많이 눌러준 코드와 비슷해서 기분이 좋다!!
- 조금더 보완이 필요한 부분을 발견했다!!
- `signs[$1] == true`는 필요가 없다...ㅎ 어짜피 `signs[$1]`이 `true`, `false`이니까


# 리펙토링

```swift
func solution(_ absolutes: [Int], _ signs: [Bool]) -> Int {
    return (0..<absolutes.count).reduce(0, { $0 + (signs[$1] ? absolutes[$1] : -absolutes[$1]) })
}
```
