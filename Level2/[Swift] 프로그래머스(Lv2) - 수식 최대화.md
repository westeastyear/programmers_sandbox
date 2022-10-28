# [Swift] 프로그래머스(Lv2) - 수식 최대화

[문제출처 프로그래머스 - 수식 최대화](https://school.programmers.co.kr/learn/courses/30/lessons/67257#qna)

# 다른 풀이

```swift
func solution(_ expression: String) -> Int64 {
    let numbers = expression.split(whereSeparator: { !$0.isNumber }).map { Int64($0)! }
    let operators = expression.split(whereSeparator: { $0.isNumber }).map { String($0) }
    let set = Set(operators)
    var priorities = [[String]]()
    var result: Int64 = 0
    
    func permuteWirth(_ arr: [String], _ n: Int) {
        if n == 0 {
            priorities.append(arr)
        } else {
            var arr = arr
            permuteWirth(arr, n-1)
            for i in 0..<n {
                arr.swapAt(i, n)
                permuteWirth(arr, n-1)
                arr.swapAt(i, n)
            }
        }
    }
    permuteWirth(Array(set), set.count-1)
    
    for priority in priorities {
        var numbers = numbers
        var operators = operators
        
        for element in priority {
            while operators.contains(element) {
                if let index = operators.firstIndex(of: element) {
                    switch element {
                    case "+":
                        numbers[index] += numbers[index+1]
                    case "-":
                        numbers[index] -= numbers[index+1]
                    case "*":
                        numbers[index] *= numbers[index+1]
                    default:
                        break
                    }
                    numbers.remove(at: index+1)
                    operators.remove(at: index)
                }
            }
        }
        if result < abs(numbers.first!) {
            result = abs(numbers.first!)
        }
    }
    return result
}
```

- 입력으로 주어지는 수식에서 연산자를 추려내어 중복제거후, 순열을 이용하여 연산자로 구할 수 있는 모든 경우의 수를 구합니다.
- 모든 경우의 수로 계산한 결과값을 절대값으로 기존에 결과값과 비교후 최대값을 반환합니다.

## Swift) Factorial, Combination, Permutation 예시

하비 블로그(raywenderlich - 펙토리얼, 조합, 순열) - [https://velog.io/@hansangjin96/Swift-Factorial-Combination-Permutation](https://velog.io/@hansangjin96/Swift-Factorial-Combination-Permutation)
