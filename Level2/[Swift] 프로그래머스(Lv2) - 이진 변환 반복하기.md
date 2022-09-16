# [Swift] 프로그래머스(Lv2) - 이진 변환 반복하기

[문제출처 프로그래머스 - 이진 변환 반복하기](https://school.programmers.co.kr/learn/courses/30/lessons/70129)

# 나의 풀이

```swift
func solution(_ s: String) -> [Int] {
    var str = s
    var cycleCount = 0
    var zeroCount = 0
    
    while str != "1" {
        var temp = ""
        cycleCount += 1
        zeroCount += str.filter { $0 == "0" }.count
        temp = str.replacingOccurrences(of: "0", with: "")
        str = String(temp.count, radix: 2)
    }
    
    return [cycleCount, zeroCount]
}
```

- `str`이 `“1”`이 될때까지 반복하도록 `while`문을 사용하였다.
- `while`문이 한번 반복할 때마다 `cycleCount`가 `+=1` 된다.
- 계속 변화하는 `str`의 0의 갯수를 `filter`를 이용해서 `count`해서 계속 더해주었다.
- `temp.count`가 이진수로 변환되는 값을 `str`에 할당해주었다.
- `[cycleCount, zeroCount]`을 반환한다.

# 리펙토링

```swift
func solution(_ s: String) -> [Int] {
    var str = s
    var cycleCount = 0
    var zeroCount = 0
    
    while str != "1" {
        cycleCount += 1
        zeroCount += str.replacingOccurrences(of: "1", with: "").count
        str = String(str.replacingOccurrences(of: "0", with: "").count, radix: 2)
    }
    
    return [cycleCount, zeroCount]
}
```

- `replacingOccurrences`로도 `count`를 셀수 있다는 걸 알게 되었다.
- `“1”`을 `“”`로 변환하여 남아있는 0의 갯수를 세어주고 `zeroCount`에 더해준다.
- `temp`를 삭제하고 `count`를 사용하여 바로 이진수를 구할 수 있도록 해주었다.
