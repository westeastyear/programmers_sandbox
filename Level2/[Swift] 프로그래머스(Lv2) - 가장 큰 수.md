# [Swift] 프로그래머스(Lv2) - 가장 큰 수

[문제출처 프로그래머스 - 가장 큰 수](https://school.programmers.co.kr/learn/courses/30/lessons/42746)

# 나의 풀이

```swift
func solution(_ numbers: [Int]) -> String {
    let strings = numbers.map { String($0) }.sorted() { $0+$1 > $1+$0 }
    guard let answer = Int(strings.joined()) else { return strings.joined() }
    return String(answer)
}
```

- 숫자인 문자열도 비교가 되어서 어안이 벙벙하였습니다.
- `$0+$1` 문자열과 `$1+$0` 문자열을 비교하여 내림차순으로 정렬합니다.
- 입력으로 `[0,0,0,0]`과 같은 경우을 대비하여 `Int` 변환으로 예외처리 해주었습니다.
