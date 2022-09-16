# [Swift] 프로그래머스(Lv2) - JadenCase 문자열 만들기

[문제출처 프로그래머스 - JadenCase 문자열 만들기](https://school.programmers.co.kr/learn/courses/30/lessons/12951)

# 나의 풀이

```swift
func solution(_ s: String) -> String {
    var arr = s.components(separatedBy: " ")

    for i in 0..<arr.count {
        if arr[i].isEmpty {
            continue
        }
        let upper = String(arr[i].first!).uppercased()
        let lower = String(arr[i].lowercased().dropFirst())
        arr[i] = upper + lower
    }
    return arr.joined(separator: " ")
}
```

- 공백기준으로 문자열을 나누어주고 배열로 반환한다.
- 반복문으로 `arr` 요소에 접근하는데, 비어있다면 `continue`해준다.
    - 공백문자가 연속해서 나올 수 있기 때문에 예외처리 해주었다.
- `arr` 요소의 첫번째 문자를 대문자로 변환한다.
- `arr` 요소를 소문자로 변환후 첫번째 문자를 지워준다.
- 위의 두개를 합쳐서 `arr[i]`에 업데이트 한다.
- 배열을 `joined(separator: “ “)` 해준다.
