# [Swift] 프로그래머스(Lv2) - 오픈채팅방

[문제출처 프로그래머스 - 오픈채팅방](https://school.programmers.co.kr/learn/courses/30/lessons/42888)

# 나의 풀이

```swift
func returnMessage(_ data: [String:String], _ arr: [[String]]) -> [String] {
    var result = [String]()
    
    arr.forEach {
        let state = $0[0]
        let name = data[$0[1]]!

        if state == "Enter" {
            result.append("\(name)님이 들어왔습니다.")
        }
        if state == "Leave" {
            result.append("\(name)님이 나갔습니다.")
        }
    }
    return result
}

func solution(_ record: [String]) -> [String] {
    var dict = [String:String]()
    let separated = record.map { $0.components(separatedBy: " ") }
    
    separated.forEach {
        let state = $0[0]
        
        if state == "Enter" || state == "Change" {
            let uid = $0[1]
            let name = $0[2]
            dict[uid] = name
        }
    }
    return returnMessage(dict, separated)
}
```

- 딕셔너리를 활용하여 채팅방에 오고 나간 명단을 먼저 전처리 해주었습니다.
- `“Enter”` 또는 `“Change”`인 경우 `dict`에 업데이트 해주었습니다.
- 전처리된 `dict`과 오고나간 순서 `separated`를 매개변수로 받는 `returnMessage()` 메서드를 정의하였습니다.
- `dict`에 `uid`로 접근하여 `value`값을 가져오고 상태에 맞는 문장을 `result`에 추가하여 리턴하도록 하였습니다.

# 리펙토링

```swift
func solution(_ record: [String]) -> [String] {
    var dict = [String:String]()
    var result = [String]()
    let separated = record.map { $0.components(separatedBy: " ") }
    
    separated.forEach {
        let state = $0[0]
        
        if state == "Enter" || state == "Change" {
            let uid = $0[1]
            let name = $0[2]
            dict[uid] = name
        }
    }
    
    separated.forEach {
        let state = $0[0]
        let name = dict[$0[1]]!
        
        if state == "Enter" {
            result.append("\(name)님이 들어왔습니다.")
        }
        if state == "Leave" {
            result.append("\(name)님이 나갔습니다.")
        }
    }
    return result
}
```

- 굳이 함수를 만들지 않는다면 이렇게 해볼 수 있겠습니다.
