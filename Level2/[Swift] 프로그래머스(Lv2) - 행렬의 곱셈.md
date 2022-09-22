# [Swift] 프로그래머스(Lv2) - 행렬의 곱셈

[문제출처 프로그래머스 - 행렬의 곱셈](https://school.programmers.co.kr/learn/courses/30/lessons/12949)

# 나의 풀이

```swift
func solution(_ arr1: [[Int]], _ arr2: [[Int]]) -> [[Int]] {
    var result = Array(repeating: [Int](), count: arr1.count)
    var temp = 0
    
    for index in 0..<arr1.count {
        for i in 0..<arr2[0].count {
            temp = 0
            for j in 0..<arr2.count {
                temp += arr1[index][j] * arr2[j][i]
            }
            result[index].append(temp)
        }
    }
    return result
}
```

- 문제 이름부터 행렬의 곱셈이라 하여 유튜브로 행렬강의를 들었다. 이해하기 어렵지 않았지만 코드로 작성하는 부분에서 감이 잡히지가 않았다.
- 쓰윽 찾아보고 문제를 풀었다!!
    - 예시로 행렬 `A`는 `m * k` 이고, 행렬 `B`는 `k * n`일때 행렬 `AB`는 `m * n`입니다.
        - (행렬 A의 열의 개수 `k`) = (행렬 B의 행의 개수 `k`)이기 때문에 두 행렬을 곱할 수 있습니다.
        - `*` 기호의 앞에 있는 행렬의 **행의 개수**와 `*` 기호 뒤에 있는 행렬의 **열의 개수**를 따르게 됩니다.
- 비어있는 이중배열을 `arr1`의 행의 개수만큼 선언해 둔다.
- `arr1`의 각 행에 접근하도록 `for-loop`의 범위를 설정한다. → `for index in 0..<arr1.count`
- `i`는 `arr2`의 각 행이 가진 값의 개수로서, 열로 사용한다. → `for i in 0..<arr2[0].count`
- `j`는 `arr2`의 행의 개수로서 사용한다. → `for j in 0..<arr2.count`
- `i`와 `j`를 보통 사용하는 것과는 달리 뒤집어서 사용한다는 것을 캐치하여야 했다. → `arr2[j][i]`
- 행렬 곱셈식을 작성하여 `temp`에 누적해준다.
- 최하단의 반복문이 끝나면 누적된 `temp`를 `result[index]`에 `append`해준다.
