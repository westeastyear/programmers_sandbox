# [Swift] 프로그래머스(Lv2) - [3차] 방금그곡

[문제출처 프로그래머스 - [3차] 방금그곡](https://school.programmers.co.kr/learn/courses/30/lessons/77485)

# 나의 풀이

```swift
func calculateTime(_ before: String, _ after: String) -> Int {
    let b = before.components(separatedBy: ":").map { Int($0)! }
    let a = after.components(separatedBy: ":").map { Int($0)! }
    return ((a[0] * 60) + a[1]) - ((b[0] * 60) + b[1])
}

func replacingSharp(_ str: String) -> String {
    let sharpDict = ["C#":"c", "D#":"d", "F#":"f", "G#":"g", "A#":"a"]
    var str = str
    sharpDict.forEach {
        str = str.replacingOccurrences(of: $0.key, with: $0.value)
    }
    return str
}

func solution(_ m: String, _ musicinfos: [String]) -> String {
    let melody = replacingSharp(m)
    var result: [(title: String, time:Int)] = []
    
    for musicinfo in musicinfos {
        let infos = musicinfo.components(separatedBy: ",")
        let totalPlayTime = calculateTime(infos[0], infos[1])
        let musicTitle = infos[2]
        let musicSheet = replacingSharp(infos[3]).map { String($0) }
        
        var newSheet = ""
        for i in 0..<totalPlayTime {
            let index = i % musicSheet.count
            newSheet += musicSheet[index]
        }
        
        if newSheet.contains(melody) {
            result.append((musicTitle, totalPlayTime))
        }
    }
    return result.max { $0.time < $1.time }?.title ?? "(None)"
}
```

- `“HH:MM”`형식으로 입력되는 시간문자열을 변환하여 음악의 총 재생시간을 반환하는 함수 `calculateTime()`을 정의하였습니다.
- 음악정보에 `#`이 포함된 문자를 구분하기 위해 함수 `replacingSharp()`을 정의하여 소문자로 변환하는 과정을 거치도록 하였습니다.
- 음악이 총 재생된 시간만큼의 음악정보가 필요하기 때문에 `totalPlayTime`만큼 반복하면서 `newSheet`에 악보정보를 추가해주게 됩니다.
    - 인덱스 `i`를 `musicSheet.count`로 나눈 나머지값으로 `musicSheet`에 첨자접근하여 값을 더해줄 수 있었습니다.
- `newSheet`에 네오가 찾으려는 `melody`가 있다면 `result`에 추가해줍니다.
- 배열의 `max()`와는 살짝다른 `Dictionary`에서의 `max()`를 사용하였습니다. 삼항연산자를 통해 간략하게 작성할 수 있었습니다.
    - 조건이 일치하는 음악이 여러개일 경우 재생된 시간이 제일 긴 음악제목을 반환합니다.
    - 재생된 시간도 같을 경우 먼저 입력된 음악 제목을 반환합니다.
    - 또, 조건이 일치하는 음악이 없을 때에는 `“(None)”`을 반환합니다.
