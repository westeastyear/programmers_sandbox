# [Swift] 프로그래머스(Lv2) - 거리두기 확인하기

[문제출처 프로그래머스 - 거리두기 확인하기](https://school.programmers.co.kr/learn/courses/30/lessons/81302)

# 다른 풀이

```swift
func isP(_ arr: [[String]], _ row: Int, _ col: Int) -> Bool {
    return arr[row][col] == "P"
}

func isO(_ arr: [[String]], _ rhs: (Int, Int), _ lhs: (Int, Int)) -> Bool {
    if rhs.0 == lhs.0 {
        for i in rhs.1+1..<lhs.1 {
            if arr[rhs.0][i] == "O" {
                return true
            }
        }
    } else if rhs.1 == lhs.1 {
        for i in rhs.0+1..<lhs.0 {
            if arr[i][lhs.1] == "O" {
                return true
            }
        }
    } else {
        if arr[rhs.0][lhs.1] == "O" || arr[lhs.0][rhs.1] == "O" {
            return true
        }
    }
    return false
}

func distance(_ rhs: (Int, Int), _ lhs: (Int, Int)) -> Int {
    return abs(rhs.0 - lhs.0) + abs(rhs.1 - lhs.1)
}

func solution(_ places: [[String]]) -> [Int] {
    var result = [Int]()
    
    for place in places {
        var value = 1
        var coord = [(Int, Int)]()
        let p = place.map { $0.map { String($0) } }
        
        for i in 0..<5 {
            for j in 0..<5 {
                if isP(p, i, j) {
                    coord.append((i, j))
                }
            }
        }
        
        if !coord.isEmpty {
            for k in 0..<coord.count-1 {
                for t in k+1..<coord.count {
                    if distance(coord[k], coord[t]) == 2 {
                        if isO(p, coord[k], coord[t]) {
                            value = 0
                        }
                    } else if distance(coord[k], coord[t]) == 1 {
                        value = 0
                    }
                }
            }
        }
        result.append(value)
    }
    return result
}
```
