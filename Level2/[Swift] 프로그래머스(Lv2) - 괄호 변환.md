# [Swift] 프로그래머스(Lv2) - 괄호 변환

[문제출처 프로그래머스 - 괄호 변환](https://school.programmers.co.kr/learn/courses/30/lessons/60058)

# 다른 풀이

```swift
func solution(_ p: String) -> String {
    if p.count < 1 { return "" }
    
    var count = 0
    var index = p.startIndex
    
    repeat {
        count += String(p[index]) == "(" ? 1 : -1
        index = p.index(after: index)
    } while count != 0
    
    var u = String(p[..<index])
    let v = String(p[index...])
    
    if u.first! == "(" {
        return u + solution(v)
    } else {
        u.removeFirst()
        u.removeLast()
        return "(" + "\(solution(v))" + ")" + "\(u.map { $0 == ")" ? "(" : ")" }.joined())"
    }
}
```

- 재귀를 잘하는 방법이 있다면 정답을 알려줘! -도니-
- `solution()` 함수자체를 재귀적으로 사용하는 코드를 발견하였습니다!
- 입력값이 빈문자열인 경우에는 미리 `“”`를 반환합니다.
- 입력값(`p`)을 순회하여 `“균형잡힌 괄호 문자열”`을 찾아줍니다.
    - `“(”`와 `“)”`가 짝이 맞지 않더라도 각각의 갯수가 같으면 `“균형잡인 괄호 문자열”`입니다.
    - `index = p.index(after: index)`
        - 주어진 인덱스 바로 뒤의 `index`를 반환하는 코드로 `p`를 순회합니다.
    - `count`값이 `0`이 되면 `repeat ~ while`문을 빠져나오게 됩니다.
- 위에서 할당한 `index`를 기준으로 `u`, `v`의 값을 분류합니다.
- `u`의 `first`가 `“(”`이라면 `“올바른 괄호 문자열"`이므로 `v`를 `solution()`의 입력값으로 재귀호출하여 위의 과정을 반복합니다.
    - 수행한 결과 문자열을 `u`에 이어 붙인 후 반환합니다.
- `u`의 `first`가 `“(”`이 아니라면 아래의 과정을 거치게 됩니다.
    - 빈 문자열에 첫 번째 문자로 `“(”`를 붙입니다.
    - 문자열 `v`에 대해 1단계부터 재귀적으로 수행한 결과 문자열을 이어 붙입니다.
    - `“)”`를 다시 붙입니다.
    - `u`의 첫 번째와 마지막 문자를 제거하고, 나머지 문자열의 괄호 방향을 뒤집어서 뒤에 붙입니다.
    - 생성된 문자열을 반환합니다.
