# [Swift] 프로그래머스(Lv2) - n^2 배열 자르기 

[문제출처 프로그래머스 - n^2 배열 자르기](https://school.programmers.co.kr/learn/courses/30/lessons/87390#qna)

# 나의 풀이

```swift
func solution(_ n: Int, _ left: Int64, _ right: Int64) -> [Int] {
    var result = Array(repeating: [Int](), count: n)
    
    for i in 1...n {
        result[i-1] = Array(repeating: i, count: i)
        for j in 0..<i-1 {
            result[j].append(i)
        }
    }
    
    return Array(result.flatMap { $0 }[Int(left)...Int(right)])
}
```

- 코드상으로는 완벽한데 시간초과가 뜬다…ㅠ
- 제한사항때문인거 같다!! 무려 $10^7$!!

# 다른 풀이

```swift
func solution(_ n: Int, _ left: Int64, _ right: Int64) -> [Int] {
    return (Int(left)...Int(right)).map { max($0 / n, $0 % n) + 1 }
}
```

- 문제설명, gif에서 보이듯이 좌표(`r` ,`c`)에 들어갈 숫자는 `max(r, c) + 1`의 공식을 거쳐 구할 수 있다.
- `left ~ right` 범위의 숫자를 좌표로 변환만 해주면 된다!
- `left ~ right` 범위의 임의의 숫자 `num`에 대해서 좌표(`r`, `c`)를 구하는 공식은 아래와 같다.
    - `r = num / n`
    - `c = num % n`
- 직접 숫자의 좌표를 그려보니 단번에 이해할수 있던 문제였다.
- 몫이랑 나머지를 이렇게 이용할 수 있다는게 너무나 신기하다!!
