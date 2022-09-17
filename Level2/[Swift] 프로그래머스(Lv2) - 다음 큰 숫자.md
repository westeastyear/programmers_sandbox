# [Swift] 프로그래머스(Lv2) - 다음 큰 숫자

[문제출처 프로그래머스 - 다음 큰 숫자](https://school.programmers.co.kr/learn/courses/30/lessons/12911)

# 나의 풀이

```swift
func returnOneCount(_ n: Int) -> Int {
    return String(n, radix: 2).filter { $0 == "1" }.count
}

func solution(_ n: Int) -> Int {
    var nextNumber = n
    var nextNumberOneCount = 0
    let inputNumberOneCount = returnOneCount(n)
    
    while inputNumberOneCount != nextNumberOneCount {
        nextNumber += 1
        nextNumberOneCount = returnOneCount(nextNumber)
    }

    return nextNumber
}
```

- `n`을 이진수로 변환하고 `“1”`의 갯수를 반환하는 `returnOneCount()` 메서드를 만들어 사용하였다.
- 변수 `nextNumber`에 `n`을 할당해둔다.
- `nextNumber` 이진수의 `“1”`의 갯수를 담아둘 변수와, 입력된 숫자 이진수의 `“1”`의 갯수를 상수에 담아두었다.
- 입력된 숫자 이진수의 `“1”`의 갯수와 다음 큰 숫자 이진수의 `“1”`의 갯수가 같을 때까지 `while` 반복문을 돌려준다.
- `nextNumber`에는 `n`이 있고, 한번 반복될때마다 1를 더해주기 때문에 조건에 충족하는 가장 작은 수를 찾게 된다.
- 이진수의 `“1”`의 갯수가 동일한 `nextNumber`를 찾게 되면 `while`문을 빠져나오게 되고 `nextNumber`를 반환한다!

# 다른 풀이

```swift
func solution(_ n: Int) -> Int {
    var answer = n + 1
    
    while n.nonzeroBitCount != answer.nonzeroBitCount {
        answer += 1
    }

    return answer
}
```

- 이진수의 `“1”`의 갯수를 반환하는 `nonzeroBitCount()` 메서드를 알게되었다!
- 효과는 굉장했다!
