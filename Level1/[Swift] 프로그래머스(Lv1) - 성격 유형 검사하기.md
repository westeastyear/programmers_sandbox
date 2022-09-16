# [Swift] 프로그래머스(Lv1) - 성격 유형 검사하기

[문제출처 프로그래머스 - 성격 유형 검사하기](https://school.programmers.co.kr/learn/courses/30/lessons/118666)

# 다른 풀이

```swift
func solution(_ survey: [String], _ choices: [Int]) -> String {
    var dict: [Character: Int] = ["R": 0, "T": 0, "C": 0, "F": 0, "J": 0, "M": 0, "A": 0, "N": 0]
    
    for i in 0..<choices.count {
        let survey: [Character] = Array(survey[i])
        
        if choices[i] < 4 {
            dict[survey[0]]! += abs(4 - choices[i])
        } else {
            dict[survey[1]]! += abs(4 - choices[i])
        }
    }
    
    let RT = dict["R"]! >= dict["T"]! ? "R" : "T"
    let CF = dict["C"]! >= dict["F"]! ? "C" : "F"
    let JM = dict["J"]! >= dict["M"]! ? "J" : "M"
    let AN = dict["A"]! >= dict["N"]! ? "A" : "N"
    
    return RT + CF + JM + AN
}
```

- 딕셔너리로 점수를 카운트 할 수 있도록 해주었다.
- 타입을 `Character`으로 미리 지정을 해두니 요소들을 중간중간 편히 사용할 수가 있었다.
- 4를 기준으로 `choices`의 요소들을 빼주고 그 절대값을 이용하는 것에 적잖은 충격을 받았다. 똑똑한 사람들…
- 딕셔너리는 순서가 없는 `Collection Type`이기 때문에 `key`를 이용해 `value`를 비교할 수 가 있다.
    - 전에는 중복한번 없애보려고 삼천포로 빠지는 고민을 했었지만, 좋은 방도가 떠오르지 않았다.
    - 때론 단순한게 제일 좋은법이라 적재적소에 판단을 잘 해야 겠다는걸 느꼈다.
