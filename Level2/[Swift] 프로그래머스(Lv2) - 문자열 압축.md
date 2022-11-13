# [Swift] 프로그래머스(Lv2) - 문자열 압축

[문제출처 프로그래머스 - 문자열 압축](https://school.programmers.co.kr/learn/courses/30/lessons/60057)

# 다른 풀이

```swift
func slice(_ str: String, by length: Int) -> [String] {
    var result = [String]()
    var temp = ""
    
    for s in str {
        temp += String(s)
        if temp.count == length {
            result.append(temp)
            temp = ""
        }
    }
    if temp != "" {
        result.append(temp)
    }
    return result
}

func compress(_ arr: [String]) -> String {
    var result = ""
    var temp = ""
    var count = 0
    
    for str in arr {
        if temp == str {
            count += 1
        } else {
            if temp != "" {
                result += (count > 1) ? "\(count)\(temp)" : "\(temp)"
            }
            temp = str
            count = 1
        }
    }
    if temp != "" {
        result += (count > 1) ? "\(count)\(temp)" : "\(temp)"
    }
    return result
}

func solution(_ s: String) -> Int {
    if s.count < 3 { return s.count }
    
    let length = s.count
    var result = Int.max
    
    for i in 1...length/2 {
        let sliced = slice(s, by: i)
        let compressed = compress(sliced).count
        result = min(result, compressed)
    }
    return result
}
```

- `1...(length/2)`까지의 단위로 문자를 끊도록 `slice()` 메서드를 정의하였습니다.
    - 반복문이 끝나면 `temp`에 남아있는 값들도 모두 추가해주어야 합니다.
- 같은 문자인경우 `count`를 `+1`해주어 압축된 문자를 반환하는 `compress()` 메서드를 정의하였습니다.
    - 반복문이 끝나면 `temp`에 남아있는 값들도 모두 추가해주어야 합니다.

# extension String

```swift
extension String {
    func slice(by length: Int) -> [String] {
        var startIndex = self.startIndex
        var results = [Substring]()
        
        while startIndex < self.endIndex {
            let endIndex = self.index(startIndex, offsetBy: length, limitedBy: self.endIndex) ?? self.endIndex
            results.append(self[startIndex..<endIndex])
            startIndex = endIndex
        }
        return results.map { String($0) }
    }
}
```

- 원하는 길이만큼 문자열을 끊어서 배열로 반환하도록 확장하는 코드를 알게 되었습니다.
- 외워버리자~
