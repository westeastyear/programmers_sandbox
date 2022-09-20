# [Swift] 프로그래머스(Lv2) - N개의 최소공배수

[문제출처 프로그래머스 - N개의 최소공배수](https://school.programmers.co.kr/learn/courses/30/lessons/12953)

# 다른 풀이

```swift
func gcd(_ a: Int, _ b: Int) -> Int {
    return b == 0 ? a : gcd(b, a % b)
}

func lcm(_ a: Int, _ b: Int) -> Int {
    return a * b / gcd(a, b)
}

func solution(_ arr: [Int]) -> Int {
    return arr.reduce(1) { lcm($0, $1) }
}
```

- 최대공약수`(GCD, Great Common Divisor)`는 가장 큰 공약수를 말합니다.
    - 두 정수 `a`, `b`의 공약수는 `a`의 약수이자 `b`의 약수인 정수입니다.
- 최대공약수 구하기 - 유클리드호제법
    - `a`를 `b`로 나눈 나머지를 `r`이라고 했을 때, `GCD(a, b) = GCD(b, r)`임을 이용하면 빠르게 구할 수 있습니다. 나머지가 0이 될때까지 진행하면 최대 공약수를 구할 수 있습니다.
    - 위의 코드에서는 재귀함수를 사용하였습니다. 재귀는 호출시 메모리를 더 사용한다고 합니다.

---

- 최소공배수`(LCM, Least Common Multiple)`는 가장 작은 공배수를 말합니다.
    - 두 정수 `a`, `b`의 공배수는 `a`과 `b` 모두의 배수가 되는 정수입니다.
- 최소공배수는 위에서 구한 최대공약수를 활용해 구할 수 있습니다.
    - `LCM(a, b) = a * b / GCD(a, b)`
    - 위의 공식을 그대로 옮긴 공식인데 `a * b`부분에서 오버플로우가 발생하기 쉽습니다.
    - 미리 한쪽을 `gcd`로 나눠주면 오버플로우가 발생하지 않습니다.

```swift
func lcm(_ a: Int, _ b: Int) -> Int {
    return a * (b / gcd(a, b))
}
```
