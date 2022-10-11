# [Swift] 프로그래머스(Lv2) - [3차] 파일명 정렬

[문제출처 프로그래머스 - [3차] 파일명 정렬](https://school.programmers.co.kr/learn/courses/30/lessons/17686)

# 나의 풀이

```swift
func solution(_ files: [String]) -> [String] {
    var storage = [String: [String]]()
    
    for (index, file) in files.enumerated() {
        let number = String(file.split(whereSeparator: { !$0.isNumber }).first!)
        let head = file.components(separatedBy: number).first!.lowercased()
        storage[file] = [head, number, String(index)]
    }
   
    return storage.sorted {
        let lhsHead = $0.value[0]
        let lhsNumber = Int($0.value[1])!
        let lhsIndex = Int($0.value[2])!
        
        let rhsHead = $1.value[0]
        let rhsNumber = Int($1.value[1])!
        let rhsIndex = Int($1.value[2])!
        
        if lhsHead == rhsHead && lhsNumber == rhsNumber {
            return lhsIndex < rhsIndex
        } else if lhsHead == rhsHead {
            return lhsNumber < rhsNumber
        } else {
            return lhsHead < rhsHead
        }
    }.map { $0.key }
}
```

- `storage`에 `[String:[String]]` 형식으로 값들을 저장하였습니다.
    - `[String:[Any]]` 형식으로 저장하려고 했었는데 **“타입으로서의 프로토콜 `Any`는 `RawRepresentable`을 따를수 없다”**하여 바꾸게 되었습니다.
    - `Protocol 'Any' as a type cannot conform to 'RawRepresentable’`
- `key`에는 `file`명, `value`에는 `[head, number, index]` 순으로 저장하였습니다.
    - `tail`은 사용되지 않습니다.

<br>

- 파일명에 숫자부분이 여러개 나올 수 있으므로 아래와 같이 작성하여 배열의 첫번째의 값을 `number`에 할당합니다.

```swift
// ex) foo010bar020.zip
// ["010", "020"] -> ["010"]

let number = String(file.split(whereSeparator: { !$0.isNumber }).first!)
```

- 위에서 구한 `number`를 기준으로 `file` 문자열 값을 나누어 배열의 첫번째 값을 소문자화후 `head`에 할당합니다.

```swift
// ex) foo010bar020.zip
// ["foo", "bar020.zip"] -> ["foo"]

let head = file.components(separatedBy: number).first!.lowercased()
```

<br>

- `storage`에 저장된 값들을 조건에 맞게 정렬후 `key`값 배열로 반환합니다.

```swift
return storage.sorted {
    let lhsHead = $0.value[0]
    let lhsNumber = Int($0.value[1])!
    let lhsIndex = Int($0.value[2])!
        
    let rhsHead = $1.value[0]
    let rhsNumber = Int($1.value[1])!
    let rhsIndex = Int($1.value[2])!
        
    if lhsHead == rhsHead && lhsNumber == rhsNumber {
        return lhsIndex < rhsIndex
    } else if lhsHead == rhsHead {
        return lhsNumber < rhsNumber
    } else {
        return lhsHead < rhsHead
    }
}.map { $0.key }
```
