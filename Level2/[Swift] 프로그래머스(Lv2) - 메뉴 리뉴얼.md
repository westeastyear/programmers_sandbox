# [Swift] 프로그래머스(Lv2) - 메뉴 리뉴얼

[문제출처 프로그래머스 - 메뉴 리뉴얼](https://school.programmers.co.kr/learn/courses/30/lessons/72411)

# 다른 풀이

```swift
var dict = [String:Int]()

func combination(_ arr: [Character], _ target: Int) {
    let limit = arr.count
    
    func combi(_ index: Int, _ depth: Int, _ str: String) {
        if target == depth {
            dict[str, default: 0] += 1
            return
        }
        for j in index..<limit {
            combi(j+1, depth+1, str+String(arr[j]))
        }
    }
    combi(0, 0, "")
}

func solution(_ orders: [String], _ course: [Int]) -> [String] {
    var result = [String]()
    
    for i in course {
        for order in orders where order.count >= i {
            combination(Array(order.sorted()), i)
        }
    }
    
    for k in course {
        let sortedDict = dict.filter { $0.key.count == k && $0.value >= 2 }.sorted { $0.value > $1.value }
        if sortedDict.isEmpty { continue }
        let maxValue = sortedDict.first!.value
        result.append(contentsOf: sortedDict.filter { $0.value == maxValue }.map { $0.key })
    }
    return result.sorted()
}
```

- 조합, 해쉬, 정렬이 짬뽕된 문제라고 할 수 있겠습니다.
- 손님들이 주문할 때 가장 많이 함께 주문한 단품메뉴들을 코스요리 메뉴로 구성하고자 합니다.
- 코스요리 메뉴는 최소 2가지 이상의 단품메뉴로 구성해야 하고, 2명 이상의 손님으로부터 주문된 단품메뉴 조합에 대해서만 코스요리 메뉴에 포함하여야 합니다.
- `orders`의 각 요소로 `course` 배열 요소의 갯수만큼 조합할 수 있는 메뉴들을 만들어 `dict`에 저장합니다.
    - `where`절을 사용하여 `order.count`가 `i`보다 이상인 경우를 조건으로 설정하였습니다.
    - `order`는 미리 정렬하여 파라미터로 넣어주었습니다.
- 재귀를 사용하여 `target` 길이만큼 만들수 있는 문자열을 조합하면서 `dict`에 저장하게 됩니다.
- `course`를 순회하여 각 갯수로 조건이 충족되는 값들을 걸러 `result`에 추가해줍니다.
    - 손님마다 중복되는 메뉴들이 있기 때문에 가장 많이 주문된 메뉴의 `value`와 같은 값들만 걸러 `result`에 추가하게 됩니다.
- 마지막으로 `result`를 오름차순으로 정렬하여 반환합니다.
