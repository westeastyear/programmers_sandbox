# [Swift] 프로그래머스(Lv2) - 스킬트리

[문제출처 프로그래머스 - 스킬트리](https://school.programmers.co.kr/learn/courses/30/lessons/49993#fnref1)

# 나의 풀이

```swift
func skillCount(_ skills: [String], _ skill_trees: [String]) -> Int {
    var count = 0
    
    for skillTree in skill_trees {
        for i in skills {
            if skillTree.contains(i) {
                count += 1
            }
        }
    }
    return count
}

func isSorted(_ arr: [Int]) -> Bool {
    
    if arr.contains(99) {
        return false
    }
    let before = arr
    let after = arr.sorted(by: <)
    
    if before != after {
        return false
    }
    return true
}

func solution(_ skill: String, _ skill_trees: [String]) -> Int {
    var count = 0
    let skillTrees = skill_trees.map { $0.map { String($0) } }
    let skills = skill.map { String($0) }
    
    for skillTree in skillTrees {
        var temp = [Int]()
        
        for i in 0..<skillCount(skills, skillTree) {
            let index = skillTree.firstIndex(of: skills[i]) ?? 99
            temp.append(index)
        }
        if isSorted(temp) {
            count += 1
        }
    }
    return count
}
```

- `skill_trees`의 각 요소가 입력으로 들어오는 `skill`에서 스킬을 몇개 가지고 있는지 반환하는 `skillCount()` 메서드를 정의하였습니다.
    - 반복문의 범위로 사용되게 됩니다.
- `arr`에 `99`가 있으면 `false`를 반환하고, 정렬 전후가 다른경우에도 `false`를 반환하는 `isSorted()` 메서드를 정의하였습니다.
- `skill_trees`의 각 요소에 입력으로 들어오는 `skill`의 문자들이 위치해 있는 인덱스 번호를 찾아 `temp`에 추가해줍니다.
- `isSorted(temp)`가 `true`라면 `count`를 `+1`해줍니다.

# 다른 풀이

```swift
func solution(_ skill: String, _ skill_trees: [String]) -> Int {
    
    func available(_ s: String, _ t: String) -> Bool {
        let temp = t.filter { s.contains($0) }
        return s.starts(with: temp)
    }
    
    return skill_trees.filter { available(skill, $0) }.count
}
```

- `starts(with:_)` 새로운 메서드를 알게 되었다!
- `s`에 `temp`가 존재 하는지 안하는지 `Bool`값을 반환한다.
- `String`으로도 판별이 가능해서 신기하다.
