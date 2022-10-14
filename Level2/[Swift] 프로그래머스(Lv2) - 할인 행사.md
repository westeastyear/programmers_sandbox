# [Swift] 프로그래머스(Lv2) - 할인 행사

[문제출처 프로그래머스 - 할인 행사](https://school.programmers.co.kr/learn/courses/30/lessons/131127)

# 나의 풀이

```swift
func solution(_ want: [String], _ number: [Int], _ discount: [String]) -> Int {
    var dict = [String:Int]()
    var count = 0
    
    for (want, number) in zip(want, number) {
        dict[want] = number
    }
    
    for i in 0..<discount.count-9 {
        var dict2 = [String:Int]()
        
        for j in i..<i+10 {
            dict2[discount[j], default: 0] += 1
        }
        if dict == dict2 {
            count += 1
        }
    }
    return count
}
```

- 입력으로 들어오는 `want`와 `number`의 요소들을 순서에 맞게 `dict`에 할당해 주었습니다.
- 회원기간은 10일이기 때문에, 마트에서 15일간 할인행사를 하는 경우, 1일에서 5일까지만 검사하도록 `0..<discount.count-9`로 범위를 설정해주었습니다.
- `i`날부터 `i+10`날까지의 품목을 검사하여 `dict2`에 할당합니다.
- `dict`와 `dict2`가 동일하다면 알뜰한 정현이가 회원가입을 하여 모든 품목을 구매할 수 있는 날이기 때문에 `count`를 `+1`해줍니다.
