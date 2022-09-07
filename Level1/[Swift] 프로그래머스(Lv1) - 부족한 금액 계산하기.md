# [Swift] 프로그래머스(Lv1) - 부족한 금액 계산하기

[문제출처 프로그래머스 - 부족한 금액 계산하기](https://school.programmers.co.kr/learn/courses/30/lessons/82612)

# 나의 풀이
```swift
func solution(_ price: Int, _ money: Int, _ count: Int) -> Int64 {
    var sum = 0
    
    for i in 1...count {
        sum += price * i
    }
    if money > sum {
        return 0
    } else {
        return Int64(sum - money)
    }
}
```

# 나의 풀이 2
```swift
func solution(_ price: Int, _ money: Int, _ count: Int) -> Int64 {
    let sum = (1...count).reduce(0, { $0 + ($1 * price) })
    return sum < money ? 0 : Int64(sum - money)
}
```
- 코드를 줄여보았는데 줄이기 전보다 시간이 더 걸렸었다...!

# 다른 풀이
```swift
func solution(_ price: Int, _ money: Int, _ count: Int) -> Int64 {
    return Int64(max((count + 1) * count / 2 * price - money, 0))
}
```
- 수학적 사고가 가미된 코드이다..👍
- 부등호를 사용하지 않고 max()를 사용하여 큰 값을 반환하도록 한다.

