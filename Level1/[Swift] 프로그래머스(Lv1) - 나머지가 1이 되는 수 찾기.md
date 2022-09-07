# [Swift] 프로그래머스(Lv1) - 나머지가 1이 되는 수 찾기

[문제출처 프로그래머스 - 나머지가 1이 되는 수 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/87389)

# 나의 풀이

```swift
func solution(_ n: Int) -> Int {
    var result = [Int]()
    for i in 1...n {
        if n % i == 1 {
            result += [i]
        }
    }
    return result[0]
}
```


# 나의 풀이 2

```swift
func solution(_ n: Int) -> Int {
    return (1...n).filter { n % $0 == 1 }[0]
}
```
- `filter`를 기억해낸 내 자신 칭찬해~
- 그치만 시간 더럽게 오래 걸림...ㅠ
- 시간이 아쉽게 나오는 부분들이 있어 좀더 생각해보았는데, 맨 처음에 나오는 값만 가져오면 된다는걸 캐치하였다.
- 제일 처음으로 나머지가 1인 값만 가져오게 코드를 짜면 된다.

# 다른 풀이

```swift
func solution(_ n: Int) -> Int {
    for i in 1..<n {
        if n % i == 1 {
            return i
        }
    }
    return 0
}
```
- 👍👍👍

# 다른 풀이

```swift
func solution(_ n: Int) -> Int {
    var answer = 1
    
    while n % answer != 1 {
        answer += 1
    }
    return answer
}
```
- 👍👍👍
