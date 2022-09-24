# [Swift] 프로그래머스(Lv2) - 튜플

[문제출처 프로그래머스 - 튜플](https://school.programmers.co.kr/learn/courses/30/lessons/64065)

# 나의 풀이

```swift
func solution(_ s: String) -> [Int] {
    var dict = [String: Int]()
    let arr = s.split(whereSeparator: { $0 == "{" || $0 == "}" || $0 == "," })
    
    for i in arr {
        let key = String(i)
        if !dict.keys.contains(key) {
            dict.updateValue(0, forKey: key)
        } else {
            dict[key]! += 1
        }
    }
    return dict.sorted(by: { $0.value > $1.value }).map { Int($0.key)! }
}
```

- `s`의 `“{”`, `“}”`, `“,”`을 어떻게 제거를 할지 고민하였다. `split(whereSeparator:_)`를 활용하여 배열을 얻을 수 있었다. → `!$0.isNumber`로도 가능하다(리펙토링ㄱㄱ)
- 문자별 카운팅을 하기 위해 `dict`를 만들어 주었다.
- `arr`의 요소에 접근하여 `dict`에 `key`값으로 존재하지 않는다면 새롭게 추가해준다.
- `dict`에 존재한다면 `count`를 `+1` 해준다.
- `dict`의 `value` 크기로 내림차순 정렬하고 `key`를 `Int`로 변환하여 반환한다.

# 리펙토링

```swift
func solution(_ s: String) -> [Int] {
    var dict = [Int: Int]()
    
    s.split(whereSeparator: { !$0.isNumber }).map { Int($0)! }.forEach {
        dict[$0, default: 0] += 1
    }

    return dict.sorted(by: { $0.value > $1.value }).map { $0.key }
}
```

- `Dictionary`에 `default:_` 값을 줄수 있다는걸 처음 알게 되었따!!
