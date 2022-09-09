# [Swift] 프로그래머스(Lv1) - 두 개 뽑아서 더하기

[문제출처 프로그래머스 - 두 개 뽑아서 더하기](https://school.programmers.co.kr/learn/courses/30/lessons/68644)

# 나의 풀이

```swift
func solution(_ numbers:[Int]) -> [Int] {
    var result = [Int]()
    
    for i in 0..<numbers.count {
        let peek = numbers[i]
        
        for j in 0..<i {
            let sum = peek + numbers[j]
            if !result.contains(sum) {
                result.append(sum)
            }
        }
    }
    return result.sorted()
}
```

- 거의 정답이라고 볼 수 있겠다💯

# 다른 풀이

```swift
func solution(_ numbers:[Int]) -> [Int] {
    var result = [Int]()
    
    for i in 0..<numbers.count {
        for j in 0..<i {
            result.append(numbers[i] + numbers[j])
        }
    }
    return Set(result).sorted()
}
```

- 다른 코드를 보고 줄이고 줄여보았다
- 여차저차한 상수들을 없에고 `if`문도 없앴다
- `Set`으로 중복을 제거할 수 있기 때문이다!(그런데 시간은 조금더 걸림..ㅋ)
