# [Swift] 프로그래머스(Lv2) - 소수 찾기

[문제출처 프로그래머스 - 소수 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/42839)

# 다른 풀이

```swift
func isPrime(_ n: Int) -> Bool {
    if n == 0 || n == 1 {
        return false
    }
    
    for i in 2..<Int(sqrt(Double(n)))+1 {
        if n % i == 0 {
            return false
        }
    }
    return true
}

func solution(_ numbers: String) -> Int {
    let arr = numbers.map { String($0) }
    var result = [Int]()
    var checkList = Array(repeating: 0, count: arr.count)
    var number = ""
        
    func dfs(_ depth: Int, _ str: String, _ count: Int) {
        if depth == count {
            if let number = Int(str) {
                if isPrime(number) && !result.contains(number) {
                    result.append(number)
                }
            }
        } else {
            for i in 0..<arr.count {
                if checkList[i] == 0 {
                    number += arr[i]
                    checkList[i] = 1
                    dfs(depth+1, number, count)
                    checkList[i] = 0
                    number = str
                }
            }
        }
    }
    for i in 1...arr.count {
        dfs(0, "", i)
    }
    return result.count
}
```

- 남의 풀이를 안보고는 너무 어려워요!!!222
- 소수를 판별하는 `isPrime()` 함수를 정의하였습니다.
- `dfs`의 파라미터로는 `depth`, `str`, `count`를 받습니다.
- `depth`의 범위는 1부터 `arr.count`까지 입니다.
    - 반복문이기 때문에 `count`가 1까지인 `dfs()`, `count`가 2까지인 `dfs()`, `count`가 `arr.count`까지인 `dfs()`를 실행하게 됩니다.
- `dfs`의 `depth`가 `count`와 같아지게 된다면, `temp`에 추가된 문자열을 `Int`변환, 소수판별, 중복체크하여 `result`에 추가합니다.
- 아직 같아지지 않았다면, `checkList`배열에서 값이 0인 인덱스를 `arr`에서 활용하여 접근하게 됩니다.
- `temp`에 `arr[k]` 값을 추가해주고 `checkList[k]`에는 1을 넣어주어 다음 반복에서는 넘어가도록 합니다.
- `dfs(depth+1, temp, count)` 재귀호출하여 위의 조건을 반복합니다.
    - `depth`는 `+1`를 해주고 `str`에는 탐색하면서 만들어진 `temp`를 넘겨주고 `count`는 그대로 넘겨줍니다.
- 탐색이 끝나 재귀에서 빠져나오게 되면 체크해주었던 1를 0으로 변환하는 과정과, `temp`를 이전 재귀호출의 `str`값으로 변환하는 과정을 거치게 됩니다.
- 마지막으로 `result`의 `count`를 반환합니다.
