# [Swift] 프로그래머스(Lv1) - 신규 아이디 추천

[문제출처 프로그래머스 - 신규 아이디 추천](https://school.programmers.co.kr/learn/courses/30/lessons/72410)

# 나의 풀이

```swift
import Foundation

func stage1(_ str: String) -> String {
    return str.map { $0.lowercased() }.joined()
}

func stage2(_ str: String) -> String {
    var temp = ""
    
    for i in str {
        if i.isLetter || i.isNumber || i == "-" || i == "_" || i == "." {
            temp.append(i)
        }
    }
    return temp
}

func stage3(_ str: String) -> String {
    var str = str
    
    while str.contains("..") {
        str = str.replacingOccurrences(of: "..", with: ".")
    }
    return str
}

func stage4(_ str: String) -> String {
    var str = str
    
    if str.hasPrefix(".") {
        str = String(str.dropFirst())
    }
    if str.hasSuffix(".") {
        str = String(str.dropLast())
    }
    return str
}

func stage5(_ str: String) -> String {
    return str.isEmpty ? "a" : str
}

func stage6(_ str: String) -> String {
    let str = str.map { String($0) }
    var result = ""
    
    if str.count >= 16 {
        for k in 0..<15 {
            result.append(str[k])
        }
    } else {
        result = str.joined()
    }
    
    if result.hasSuffix(".") {
        result = String(result.dropLast())
    }
    return result
}

func stage7(_ str: String) -> String {
    var str = str
    
    while str.count < 3 {
        str += String(str.last!)
    }
    return str
}

func solution(_ new_id: String) -> String {
    var result = ""
     
    result = stage1(new_id)
    result = stage2(result)
    result = stage3(result)
    result = stage4(result)
    result = stage5(result)
    result = stage6(result)
    result = stage7(result)
    return result
}
```

- 일부러 함수를 분리하여 작성하였는데 나중에 보니 이럴 필요가 없었다고 느꼈다.
- 빨리 되기만 하면 장땡이기 때문이다.

# 다른 풀이

```swift
func solution(_ new_id: String) -> String {
    var result = ""
    
    // 1차
    let newID = new_id.lowercased()
    
    // 2차
    for i in newID {
        if i.isLetter || i.isNumber || i == "-" || i == "_" || i == "." {
            result.append(i)
        }
    }
    
    // 3차
    while result.contains("..") {
        result = result.replacingOccurrences(of: "..", with: ".")
    }
    
    // 4차
    if result.hasPrefix(".") {
        result.removeFirst()
    }
    if result.hasSuffix(".") {
        result.removeLast()
    }
    
    // 5차
    if result.isEmpty {
        result = "a"
    }
    
    // 6차
    if result.count >= 16 {
        let endIndex = result.index(result.startIndex, offsetBy: 15)
        result = String(result[result.startIndex..<endIndex])
        if result.hasSuffix(".") {
            result.removeLast()
        }
    }
    
    // 7차
    while result.count < 3 {
        result += String(result.last!)
    }

    return result
}
```

- 2단계에서 판별하는 `isLetter`, `isNumber`를 잘 기억하자!
- 3단계에서 `“..”`이 포함되어 있다면 계속 반복하면서 바꾸는 코드가 아주 좋았다.
- 6단계에서 문자열의 `endIndex`를 구하는 방법과 어떻게 활용하는지를 배울 수 있었다.
