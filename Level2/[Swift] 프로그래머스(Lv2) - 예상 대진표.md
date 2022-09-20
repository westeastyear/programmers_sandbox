# [Swift] 프로그래머스(Lv2) - 예상 대진표

[문제출처 프로그래머스 - 예상 대진표](https://school.programmers.co.kr/learn/courses/30/lessons/12985)

# 나의 풀이

```swift
func solution(_ n: Int, _ a: Int, _ b: Int) -> Int {
    var A = a
    var B = b
    var round = 0
    
    while A != B {
        round += 1
        
        if A % 2 == 0 {
            A /= 2
        } else {
            A = A / 2 + 1
        }
        
        if B % 2 == 0 {
            B /= 2
        } else {
            B = B / 2 + 1
        }
    }
    return round
}
```

- 숫자가 짝수일 경우 2로 나눈 몫을 넣어주었다
- 숫자가 홀수일 경우 2로 나눈 몫에 1을 더해주었다.
- `A`와 `B`가 같아질때 까지 `round`를 `+1`해주며 반복해준다.
- `A`와 `B`가 같아지게 되면 `while`문을 빠져나오게 되고 마지막으로 `round`를 반환한다.

# 다른 코드

```swift
func solution(_ n: Int, _ a: Int, _ b: Int) -> Int {
    var A = a
    var B = b
    var round = 0
    
    while A != B {
        round += 1
        A = (A + 1) / 2
        B = (B + 1) / 2
    }
    return round
}
```

- 홀수, 짝수 구분할 필요 없이 `A` or `B`에 `+1`한 값에 나누기 2를 해주어도 동일한 값을 얻을 수가 있다?!?!
- 소수점은 이후의 값은 필요하지 않으니깐!
