# [Swift] 프로그래머스(Lv2) - 올바른 괄호

[문제출처 프로그래머스 - 올바른 괄호](https://school.programmers.co.kr/learn/courses/30/lessons/12909#)

# 나의 풀이

```swift
func solution(_ s: String) -> Bool {
    var str = s
    
    while str.contains("()") {
        str = str.replacingOccurrences(of: "()", with: "")
    }

    return str.isEmpty
}
```

- 잔머리가 핑핑돌아서 개쉽네 하면서 기분좋게 제출했는데?!?! 효율성 테스트에서 실패가 떳다…? 뭘까?
- 초장부터 알될 녀석들은 빨리 `return` 하라는 의미인거 같다.

# 나의 풀이 2

```swift
func solution(_ s: String) -> Bool {
    var count = 0
    
    for i in s {
        if i == "(" {
            count += 1
        } else {
            count -= 1
        }
        if count < 0 {
            return false
        }
    }
    return count == 0
}
```

- `count`가 마이너스 되는 순간 `false`를 반환하도록 하였다.
- 어차피 안될녀석이니깐!!
