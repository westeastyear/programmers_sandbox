# [Swift] 프로그래머스(Lv2) - 괄호 회전하기

[문제출처 프로그래머스 - 괄호 회전하기](https://school.programmers.co.kr/learn/courses/30/lessons/76502)

# 나의 풀이

```swift
func isBracket(_ s: String) -> Bool {
    var str = s
    let brackets = ["()", "[]", "{}"]
    
    while !str.isEmpty {
        let before = str
        
        brackets.forEach {
            str = str.replacingOccurrences(of: $0, with: "")
        }
        
        let after = str
        
        if before == after {
            return false
        }
    }

    return true
}

func solution(_ s: String) -> Int {
    var str = s
    var count = 0
    
    for _ in 0..<str.count {
        if isBracket(str) {
            count += 1
        }
        let temp = str.removeFirst()
        str = str + String(temp)
    }
    
    return count
}
```

- 어찌되었든 통과한 풀이… 시간을 어떻게 손 볼 수 있을까..?
- `isBracket` 메서드를 만들어 괄호를 지울수 없을때까지 반복하여 `Bool`값을 판별하도록 하였다.
- `solution`에서는 `s`의 `count`만큼 반복하도록 하였다.
- `isBracket`이 `true`이면 `count`를 `+1`해준다.
- `str`의 맨 앞 문자를 뒤로 옮겨가면서 `isBrecket`이 가능한지 검증한다.
- 마지막으로 `count`를 반환한다.

# 다른 풀이

```swift
func isBracket(_ s: String) -> Bool {
    let brackets: [Character: Character] = ["(": ")", "{": "}", "[": "]"]
    var stack = [Character]()
    
    for i in s {
        if !stack.isEmpty && brackets[stack.last!] == i {
            stack.removeLast()
        } else {
            stack.append(i)
        }
    }
    return stack.isEmpty
}

func solution(_ s: String) -> Int {
    var str = s
    var count = 0
    
    for _ in 0..<str.count {
        if isBracket(str) {
            count += 1
        }
        str.append(str.removeFirst())
    }
    return count
}
```

- 괄호별로 짝에 맞게 `Dictionary`형태로 저장해 놓습니다.
- `stack`자료구조를 활용하였습니다.
- `stack`에 값이 있고, `stack`의 마지막 값을 `key`값으로 `brackets`에 접근하였을때 `value`가 `i`와 같다면 같은 올바른 괄호의 짝이므로 `stack`에서 삭제해줍니다.
- 조건에 충족하지 않는 경우에는 `stack`에 추가해줍니다.
- 반복하여 `stack`의 `Bool`값을 반환하도록 합니다.
- `isBracket` 메서드가 `true`인 경우 `count`를 `+1`합니다.
- `str`의 첫번째 문자를 제거후 맨 뒤로 옮겨줍니다.
- `str`의 문자 갯수만큼 반복하고 검증하여 마지막에 `count` 값을 반환합니다.
