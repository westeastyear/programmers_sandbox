# [Swift] 프로그래머스(Lv1) - K번째수

[문제출처 프로그래머스 - K번째수](https://school.programmers.co.kr/learn/courses/30/lessons/42748)

# 나의 풀이

```swift
func solution(_ array: [Int], _ commands: [[Int]]) -> [Int] {
    
    var arr = [Int]()
    var result = [Int]()
    var peek = 0
    
    for i in 0..<commands.count {
        let left = array[commands[i][0]-1]
        let right = array[commands[i][1]-1]
        peek = commands[i][2]-1
        
        arr = []
        for j in array.firstIndex(of: left)!...array.firstIndex(of: right)! {
            arr.append(array[j])
        }
        result.append(arr.sorted()[peek])
    }
    return result
}
```

- `signal: illegal instruction (core dumped))` 라는 컴파일 에러가 뜨는 경우
- 이 경우에는 **논리 에러**일 때가 많으므로 Array의 Index out of range 문제, 옵셔널 문제, 잘못된 수식 문제 이므로 잘 확인해보자
- 뭔가 엣지케이스 때문에 에러가 나는거 같다…또는 강제 언래핑? 그래서 지워보는 방향으로 바꿔보았다

# 나의 풀이 2

```swift
func solution(_ array: [Int], _ commands: [[Int]]) -> [Int] {
    var result = [Int]()
    
    for i in 0..<commands.count {
        var arr = [Int]()
        for j in array[commands[i][0]-1...commands[i][1]-1] {
            arr.append(j)
        }
        result.append(arr.sorted()[commands[i][2]-1])
    }
    return result
}
```

- `array[commands[i][0]-1...commands[i][1]-1]` 배열의 범위로도 반복을 돌릴수가 있다?!👍
- 나머지는 간단하다

# 다른 풀이

```swift
func solution(_ array: [Int], _ commands: [[Int]]) -> [Int] {
    return commands.map { element in
        return array[(element[0]-1)...(element[1]-1)].sorted()[element[2]-1]
    }
}
```

- `map()`을 사용한 풀이이다.
- 깔끔하지만 가독성까지 챙긴다면??

# 다른 풀이 2

```swift
func solution(_ array: [Int], _ commands: [[Int]]) -> [Int] {
    return commands.map {
        let i = $0[0]-1
        let j = $0[1]-1
        let k = $0[2]-1
        return array[i...j].sorted()[k]
    }
}
```

- 아주 그냥 한 눈에 잘 들어오는 코드인거 같다👍👍👍
- 내 스타일이다 😘
