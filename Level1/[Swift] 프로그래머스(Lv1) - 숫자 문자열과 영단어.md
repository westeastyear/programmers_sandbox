# [Swift] 프로그래머스(Lv1) - 숫자 문자열과 영단어

[문제출처 프로그래머스 - 숫자 문자열과 영단어](https://school.programmers.co.kr/learn/courses/30/lessons/81301)

# 나의 풀이

```swift
func solution(_ s: String) -> Int {
    
    let numbers = ["zero" : "0",
                   "one" : "1",
                   "two" : "2",
                   "three" : "3",
                   "four" : "4",
                   "five" : "5",
                   "six" : "6",
                   "seven" : "7",
                   "eight" : "8",
                   "nine" : "9"]
    
    var temp = ""
    var result = ""
    
    for i in s {
        if let j = Int(String(i)) {
            result.append(String(j))
            continue
        }
        
        temp.append(String(i))
        if numbers.keys.contains(temp) {
            result.append(numbers[temp]!)
            temp = ""
        }
    }
    return Int(result)!
}
```

- `key : value`를 지정해주어서 문제를 해결하였습니다.
- `s`가 숫자로 변환이 되는 경우엔 바로 `result`에 넣어주고 `continue`해줍니다.
- `temp`에 `s`를 넣어주면서 `temp`가 딕셔너리의 `key`값으로 포함될때까지 반복합니다.
- 포함된다면 그 `key`의 `value`값을 `result`에 추가해줍니다.

# 다른 풀이

```swift
func solution(_ s:String) -> Int {

    var s = s
        var answer = s.replacingOccurrences(of: "zero", with: "0")
            .replacingOccurrences(of: "one", with: "1")
            .replacingOccurrences(of: "two", with: "2")
            .replacingOccurrences(of: "three", with: "3")
            .replacingOccurrences(of: "four", with: "4")
            .replacingOccurrences(of: "five", with: "5")
            .replacingOccurrences(of: "six", with: "6")
            .replacingOccurrences(of: "seven", with: "7")
            .replacingOccurrences(of: "eight", with: "8")
            .replacingOccurrences(of: "nine", with: "9")

    return Int(answer)!
}
```

- !!!!!!
- 약간 머리가 편한 코드랄까?
- 하지만 코드는 중복이 없는게 내 스탈

# 다른 풀이 2

```swift
func solution(_ s:String) -> Int {
    let arr = ["zero","one","two","three","four","five","six","seven","eight","nine"]
    var str = s
    for i in 0..<arr.count {
        str = str.replacingOccurrences(of: arr[i], with: String(i))
    }
    return Int(str)!
}
```

- !!!!!!2
- 인덱스를 이렇게도 사용을 한다는 거에서 충격…
