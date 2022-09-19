# [Swift] 프로그래머스(Lv2) - 영어 끝말잇기

[문제출처 프로그래머스 - 영어 끝말잇기](https://school.programmers.co.kr/learn/courses/30/lessons/12981)

# 나의 풀이

```swift
func solution(_ n: Int, _ words: [String]) -> [Int] {
    var result = [String]()
    
    for i in 0..<words.count {
        let numberAndSequence = [(i % n + 1), (i / n + 1)]
        
        if result.contains(words[i]) {
            return numberAndSequence
        }
        
        if result.count != 0 {
            if result.last?.last! != words[i].first! {
                return numberAndSequence
            }
        }
        result.append(words[i])
    }
    return [0, 0]
}
```

- 입력되는 `n`명과 인덱스 숫자를 활용하여 끝말있기를 하는 사람의 번호와 차례를 구하는 공식을 구해줍니다.
    - 번호는 `i`에서 `n`명을 나눈 나머지에 `+1`를 해줍니다.
    - 차례는 `i`에서 `n`명을 나눈 몫에 `+1`를 해줍니다.
- `result`에 포함되어 있는 단어를 말한 경우 틀린 사람의 번호와 차례를 반환하게 됩니다.
- `result`의 갯수가 0이 아닌 경우에 배열의 마지막 요소의 마지막 문자가 `word[i]`의 첫번째 문자와 다른경우, 틀렸기 때문에 번호와 차례를 반환하게 됩니다.
- 모든 조건을 통과하는 경우에는 `result`에 추가되게 됩니다.
- 탈락자가 생기지 않는다면, `[0, 0]`를 반환하게 됩니다.
