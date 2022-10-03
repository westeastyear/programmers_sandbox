# [Swift] 프로그래머스(Lv2) - [3차] 압축

[문제출처 프로그래머스 - [3차] 압축](https://school.programmers.co.kr/learn/courses/30/lessons/43165)

# 다른 풀이

```swift
func solution(_ msg: String) -> [Int] {
    let alphabet = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
    var dict = [String:Int]()
    for (i, v) in alphabet.enumerated() {
        dict[String(v)] = i+1
    }
    let msg = msg.map { String($0) }
    var result = [Int]()
    
    var index = 0
    var beforeCh = ""
    while index < msg.count {
        let presentCh = msg[index]
        let combinedCh = beforeCh + presentCh
        
        if dict[combinedCh] == nil {
            dict[combinedCh] = dict.count+1
            result.append(dict[beforeCh]!)
            beforeCh = ""
        } else {
            beforeCh += presentCh
            index += 1
        }
    }
    result.append(dict[beforeCh]!)
    return result
}
```

- 오늘은 컨디션이 이상해서 머리가 안돌아가는 날이였던거 같다.
- 남의 풀이 쓰윽쓰윽…ㅎ
- 원래는 직접 하나하나 알파벳으로 이루어진 딕셔너리를 만들어 줬었는데, 반복문으로 생성해서 사용하였다.
- `index`가 `msg`의 갯수보다 작은경우에만 `while` 반복문을 진행한다.
- `dict[combinedCh]`가 `nil`이라면 `dict`에 등록를 해준다.
- `dict`에 없기 때문에 `result`에는 조합되기 전의 문자인 `beforeCh`의 `value`값을 추가해주고, `beforeCh`를 빈 문자열로 초기화 해준다.
- 조합된 문자가 `dict`에 등록이 되어 있는경우 `beforeCh`에 `presentCh`를 더해주도록 한다.
- `index`에도 `+1` 해주고 다음 문자가 조합될수 있도록 해서 `dict`에 등록이 되어있는지 검증하도록 한다.
