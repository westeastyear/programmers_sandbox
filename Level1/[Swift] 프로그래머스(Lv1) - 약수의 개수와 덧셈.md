# [Swift] 프로그래머스(Lv1) - 약수의 개수와 덧셈

[문제출처 프로그래머스 - 약수의 개수와 덧셈](https://school.programmers.co.kr/learn/courses/30/lessons/77884)

# 나의 풀이

```swift
func returnCounts(_ n: Int) -> Int {
    var count = 0
    
    for i in 1...n {
        count = 0
        for j in 1...i {
            if i % j == 0 {
                count += 1
            }
        }
    }
    return count
}

func solution(_ left: Int, _ right: Int) -> Int {
    var sum = 0
    
    for i in left...right {
        if returnCounts(i) % 2 == 0 {
            sum += i
        } else {
            sum += -i
        }
    }
    return sum
}
```

- 나는 왜 처음부터 깔끔하게 짜고 싶어서 시간을 버렸나... 너무 시간허비를 했다.
- 기능을 분리하여 일단먼저 부랴부랴 작성해보았다!!

# 리펙토링

```swift
func solution(_ left: Int, _ right: Int) -> Int {
    return (left...right).map { i in (1...i).filter { j in i % j == 0 }.count % 2 == 0 ? i : -i }.reduce(0, +)
}
```

- 위에서 시간을 허비한 이유 -> 고차함수도 파라미터를 받을 수 있다는 걸 까먹고 있어서 해맸었다…

# 다른 풀이

```swift
func solution(_ left:Int, _ right:Int) -> Int {
    var answer = 0

    for number in left...right{
        if floor(sqrt(Double(number))) == sqrt(Double(number)) {
            answer -= number
        } else {
            answer += number
        }
    }
    return answer
}
```

- 다른 풀이중에 속도가 빠른 코드가 있어 슬쩍해보았다.
- 입력값 중에서 정수인 제곱근을 가질 수 있는 숫자의 약수의 갯수는 홀수인것을 캐치할 수 있다.
- `number`의 제곱근을 구하고 소수점 이하를 버린값과, 정수인 제곱근을 가지는 `number`의 제곱근이 같은 경우에만 약수의 갯수가 홀수인 숫자이기 때문에 가감해준다.
- `floor()` → 소수점 이하 버림

```swift
floor(Double(19.223)) -> 19.0
floor(sqrt(Double(13))) -> 3.0
```

- `sqrt()` → 제곱근

```swift
sqrt(Double(16)) -> 4.0
sqrt(Double(13)) -> 3.605551275463989
```
