# [Swift] 프로그래머스(Lv2) - 거리두기 확인하기

[문제출처 프로그래머스 - 거리두기 확인하기](https://school.programmers.co.kr/learn/courses/30/lessons/81302)

# 다른 풀이

```swift
func isP(_ arr: [[String]], _ row: Int, _ col: Int) -> Bool {
    return arr[row][col] == "P"
}

func isO(_ arr: [[String]],_ r: (Int, Int), _ l: (Int, Int)) -> Bool {
    if r.0 == l.0 {
        for i in r.1+1..<l.1 {
            if arr[r.0][i] == "O" {
                return true
            }
        }
    } else if r.1 == l.1 {
        for i in r.0+1..<l.0 {
            if arr[i][l.1] == "O" {
                return true
            }
        }
    } else {
        if arr[r.0][l.1] == "O" || arr[l.0][r.1] == "O" {
            return true
        }
    }
    return false
}

func distance(_ r: (Int, Int), _ l: (Int, Int)) -> Int {
    return abs(r.0 - l.0) + abs(r.1 - l.1)
}

func solution(_ places: [[String]]) -> [Int] {
    var result = [Int]()
    
    for place in places {
        var answer = 1
        var coordi = [(Int, Int)]()
        let p = place.map { $0.map { String($0) } }
        
        for i in 0..<5 {
            for j in 0..<5 {
                if isP(p, i, j) {
                    coordi.append((i, j))
                }
            }
        }
        
        if !coordi.isEmpty {
            for i in 0..<coordi.count-1 {
                for j in i+1..<coordi.count {
                    if distance(coordi[i], coordi[j]) == 2 {
                        if isO(p, coordi[i], coordi[j]) {
                            answer = 0
                            break
                        }
                    } else if distance(coordi[i], coordi[j]) == 1 {
                        answer = 0
                        break
                    }
                }
            }
        }
        result.append(answer)
    }
    return result
}
```

- `isP()` 메서드를 정의하여 이중배열에서의 `P`좌표를 추출합니다.
- `isO()` 메서드를 정의하여 두개의 좌표사이에 `“O"`가 있는지 검증합니다.
    - 행이 동일한 경우, 열이 동일한 경우, 대각선 위치에 있는 경우를 검증합니다.
    - `arr[r.0][l.1] == "O" || arr[l.0][r.1] == "O"` → 양쪽 조건문 둘다 `(r,l)`, `(r,l)`의 순서로 접근했어서 테케를 통과하지 못했었습니다. 직접 손으로 인덱스를 작성해보니, 두번째 조건에서는 `(l, r)`의 순서로 접근해야 한다는걸 알게 되었습니다.
- 맨해튼 거리가 `1`인 경우 - 거리두기가 지켜지지 않고 있기 때문에 `0`을 할당하고 `break`합니다.
- 맨해튼 거리가 `2`인 경우 - 사이에 빈 테이블인 `“O”`이 있다면 `0`을 할당하고 `break`합니다. 사이에 파티션인 `“X”`가 있다면 거리두기가 지켜지고 있기 때문에 검증을 계속합니다.

<br>

- 모든 좌표들간의 거리를 알기 위해 탐색한 방법입니다. 편도만 검증하면 됩니다…!

```swift
for i in 0..<coordi.count-1 {
    for j in i+1..<coordi.count {
        print("\(i), \(j)")
    }
}

// 0, 1
// 0, 2
// 0, 3
// 0, 4
// 0, 5
// 1, 2
// 1, 3
// 1, 4
// 1, 5
// 2, 3
// 2, 4
// 2, 5
// 3, 4
// 3, 5
// 4, 5
```
