# [Swift] 프로그래머스(Lv1) - 2016년

[문제출처 프로그래머스 - 2016년](https://school.programmers.co.kr/learn/courses/30/lessons/12901)

# 나의 풀이

```swift
func solution(_ a: Int, _ b: Int) -> String {
    let weeks = ["THU","FRI","SAT","SUN","MON","TUE","WED"]
    var daysCount = 0
    
    for i in 1..<a {
        switch i {
        case 1, 3, 5, 7, 8, 10, 12:
            daysCount += 31
        case 2:
            daysCount += 29
        default:
            daysCount += 30
        }
    }
    daysCount += b
    
    return weeks[daysCount % 7]
}
```

- 시작일(1월1일)이 금요일(= Index 1)이기 때문에 목요일(= Index 0)부터 `weeks`를 작성하였다.
- 윤년이기 때문에 각 월별로 일수를 더해주었다.
- 총 일수(= daysCount)를 7로 나누어주고 나머지를 통해 요일을 구할 수 있었다.
- 다른 풀이들은 별로 맘에 안든다..ㅎ
