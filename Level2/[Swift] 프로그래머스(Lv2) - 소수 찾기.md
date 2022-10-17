# [Swift] 프로그래머스(Lv2) - 소수 찾기

[문제출처 프로그래머스 - 소수 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/42839)

# 다른 풀이

```swift
func isPrime(_ n: Int) -> Bool {
    if n == 0 || n == 1 {
        return false
    }
    
    for i in 2..<Int(sqrt(Double(n)))+1 {
        if n % i == 0 {
            return false
        }
    }
    return true
}

func solution(_ numbers: String) -> Int {
    let arr = numbers.map { String($0) }
    var result = [Int]()
    var checkList = Array(repeating: 0, count: arr.count)
    var number = ""
        
    func dfs(_ depth: Int, _ str: String, _ count: Int) {
        if depth == count {
            if let number = Int(str) {
                if isPrime(number) && !result.contains(number) {
                    result.append(number)
                }
            }
        } else {
            for i in 0..<arr.count {
                if checkList[i] == 0 {
                    number += arr[i]
                    checkList[i] = 1
                    dfs(depth+1, number, count)
                    checkList[i] = 0
                    number = str
                }
            }
        }
    }
    for i in 1...arr.count {
        dfs(0, "", i)
    }
    return result.count
}
```

- 공부중… 업뎃예정
