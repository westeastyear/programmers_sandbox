# [Swift] 프로그래머스(Lv1) - 신고 결과 받기

[문제출처 프로그래머스 - 신고 결과 받기](https://school.programmers.co.kr/learn/courses/30/lessons/92334)

# 나의 풀이

```swift
func solution(_ id_list: [String], _ report: [String], _ k: Int) -> [Int] {
    var reportDict = [String: [String]]()
    var countDict = [String: Int]()
    var result = [Int]()
    
    for i in id_list {
        reportDict.updateValue([], forKey: i)
        countDict.updateValue(0, forKey: i)
    }
    
    for j in report {
        let k = j.components(separatedBy: " ")
        if !reportDict[k[1]]!.contains(k[0]) {
            reportDict[k[1]]!.append(k[0])
        }
    }
    
    for h in id_list {
        if reportDict[h]!.count >= k {
            for u in reportDict[h]! {
                countDict[u]! += 1
            }
        }
    }
    
    for t in id_list {
        result.append(countDict[t]!)
    }
        
    return result
}
```

- 딕셔너리를 많이 활용한 코드인데 런타임 에러가 나타난다고 한다…ㅠㅠ → 딕셔너리랑 상관이 없고, 중복으로 들어오는 값이랑, 조건문때문!!
- 힌트 하나 쓸쩍해서 `Set`을 활용해 보라는 걸 알게 되었다! 중복제거할때는 `Set`이 좋다는걸 알고는 있는데 기억이 나지 않는 나의 머리…🤯

# 나의 풀이 2

```swift
func solution(_ id_list: [String], _ report: [String], _ k: Int) -> [Int] {
    var reportedDict = [String: [String]]()
    var countDict = [String: Int]()
    
    for i in id_list {
        reportedDict.updateValue([], forKey: i)
        countDict.updateValue(0, forKey: i)
    }
    
    for j in Set(report) {
        let k = j.components(separatedBy: " ")
        reportedDict[k[1]]!.append(k[0])
    }
    
    for h in id_list {
        if reportedDict[h]!.count >= k {
            for u in reportedDict[h]! {
                countDict[u]! += 1
            }
        }
    }

    return id_list.map {
        countDict[$0]!
    }
}
```

- `id_list`의 요소에 맞게 `reportedDict`, `countDict`를 업데이트 해준다.
- `Set(report)`로 미리 중복제거를 해주고 신고당한 사람을 `key`로 신고한 사람들을 `value`로 `reportedDict`에 추가해 주었다.
- `k`명 이상에게 신고된 유저는 이용이 정지 되며 해당 유저를 신고한 사람들에게 정지 사실을 메일로 보내주어야 한다.
- `k`명 이상에게 신고된 유저의 `value`에 접근하여 신고한 사람(메일을 받을 사람)들의 `value`에 `+= 1` 해줍니다.
- 신고한 사람들이 받을 메일의 갯수를 반환해준다.
- 아예 처음부터 `report`의 중복된 값들을 `Set`으로 미리 걸러주면 내부에서 `if`문을 사용할 일이 없어지게 되는 매직!! 🪄
- `result` 배열 변수도 삭제하고 마지막에 `map`으로 반환하도록 리펙토링 하였다.
