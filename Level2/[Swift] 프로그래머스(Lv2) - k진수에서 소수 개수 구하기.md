# [Swift] 프로그래머스(Lv2) - k진수에서 소수 개수 구하기

[문제출처 프로그래머스 - k진수에서 소수 개수 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/92335)

# 나의 풀이

```swift
func isPrime(_ n: Int) -> Bool {
    for i in 2..<n {
        if n % i == 0 {
           return false
        }
    }
    return true
}

func solution(_ n: Int, _ k: Int) -> Int {
    let arr = String(n, radix: k).split(separator: "0").filter { $0 != "1"}
    var count = 0

    for i in arr {
        if isPrime(Int(i)!) {
            count += 1
        }
    }
    
    return count
}
```

- 완벽한 풀이인데 시간초과가 나는 테케가 있어서 살펴보니 소수를 판별할때 제곱근까지만 세주어야 한다는걸 발견하였다…!

# 리펙토링

```swift
func isPrime(_ n: Int) -> Bool {
    guard n != 1 else { return false }

    let root = Int(sqrt(Double(n)))+1    
    for i in 2..<root {
        if n % i == 0 {
           return false
        }
    }
    return true
}

func solution(_ n: Int, _ k: Int) -> Int {
    return String(n, radix: k).split(separator: "0").filter { isPrime(Int($0)!) }.count
}
```

- 소수임을 판별하는 `isPrime()` 메서드를 생성하였습니다.
- `guard`문으로 `1`이 들어오는 경우에 `false`를 반환하도록 해서, `solution`에 있던 2개의 `filter`중 `1`를 판별하던 `filter` 코드를 줄일 수 있었습니다.
- 시간초과가 나는 문제 때문에 반복문의 범위를 `2...<제곱근 + 1` 까지 설정하였습니다. → 아주 꿀팁🍯🐝
- `k`진수로 변환된 문자열을 `“0”`기준으로 `split`해주고 `isPrime()` 판별후 갯수를 세주면 끗!
