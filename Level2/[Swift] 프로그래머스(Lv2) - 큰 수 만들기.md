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

- `arr`를 순회하면서 값을 `stack`에 쌓습니다.
- `K`가 `0`보다 크고, `stack`에 값이 있으며, `stack`의 마지막 값이 `arr[i]`보다 작은경우 `stack`의 마지막 값을 제거해 줍니다. 제거되었기 때문에 `K`를 `-1`해줍니다.
    - 조건에 만족한다면 반복합니다.
- `K`가 `0`인경우 더 이상 제거하면 안되기 때문에 나머지 `arr`를 `stack`에 추가해줍니다.
    - 특정 범위의 배열을 뒤에 추가하고자 할 때는 `append(contentsOf:_)`를 사용할 수 있습니다.
- `K`가 `0`이 아니라면 `stack`에 `arr[i]`를 추가해줍니다.
- 마지막으로 `stack`에 `K`개를 제거하였을때 가장 큰 수가 만들어집니다.
- `stack`은 `[Character]` 타입이기 때문에 `String` 문자열로 합칠수 있습니다.
- `stack`의 `0..<nCount-k`까지의 문자열을 반환합니다.
