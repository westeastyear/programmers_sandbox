# [Swift] 프로그래머스(Lv2) - 큰 수 만들기

[문제출처 프로그래머스 - 큰 수 만들기](https://school.programmers.co.kr/learn/courses/30/lessons/42883)

# 다른 풀이

```swift
func solution(_ number: String, _ k: Int) -> String {
    let nCount = number.count
    var arr = Array(number)
    var stack: [Character] = []
    var K = k

    for i in arr.indices {
        while K > 0 && !stack.isEmpty && stack.last! < arr[i] {
            stack.removeLast()
            K -= 1
        }
        if K == 0 {
            stack.append(contentsOf: Array(arr[i...]))
            break
        } else {
            stack.append(arr[i])
        }
    }
    return String(stack[0..<nCount-k])
}
```
