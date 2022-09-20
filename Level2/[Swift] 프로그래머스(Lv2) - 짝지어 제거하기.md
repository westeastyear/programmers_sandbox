# [Swift] 프로그래머스(Lv2) - 짝지어 제거하기

[문제출처 프로그래머스 - 짝지어 제거하기](https://school.programmers.co.kr/learn/courses/30/lessons/12973)

# 나의 풀이

```swift
func solution(_ s: String) -> Int{
    
    var str = s
    var temp = ""
    
    while !str.isEmpty {
        let before = str
        for i in str {
            if temp.isEmpty || temp.last! != i {
                temp = String(i)
                continue
            }
            if temp.last! == i {
                temp += String(i)
                str = str.replacingOccurrences(of: temp, with: "")
                temp = ""
                break
            }
        }
        let after = str
        if before == after {
            return 0
        }
    }
    return 1
}
```

- 아이디어가 떠오르지 않아 어떻게든 작동되는 코드를 작성했는데 역시나 시간초과가 뜬다.
- 효율성도 똥이다💩

# 다른 풀이

```swift
func solution(_ s: String) -> Int{
    var stack: [Character] = []
    
    if s.count % 2 != 0 {
        return 0
    }
    
    for i in s {
        if !stack.isEmpty && stack.last! == i {
            stack.removeLast()
        } else {
            stack.append(i)
        }
    }
    return stack.isEmpty ? 1 : 0
}
```

- `s.count`가 홀수인 경우 무조건 실패임으로 미리 0을 반환하여 끝내도록 하였다.
- `stack`으로 사용할 배열을 선언해 주었다.
- `stack`에 요소가 있고 `i`와 같다면 `stack`의 마지막 요소를 삭제해준다.
- `stack`이 비어있거나 `i`와 다르다면 `stack`에 추가해준다.
- `for-loop`로 `s`를 반복해준다.
- 마지막으로 `stack`이 비어있다면 1, 비어있지 않다면 0을 반환한다.
