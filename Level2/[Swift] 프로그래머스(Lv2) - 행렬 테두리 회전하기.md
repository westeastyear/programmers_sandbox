# [Swift] 프로그래머스(Lv2) - 행렬 테두리 회전하기

[문제출처 프로그래머스 - 행렬 테두리 회전하기](https://school.programmers.co.kr/learn/courses/30/lessons/77485)

# 나의 풀이

```swift
func solution(_ rows: Int, _ columns: Int, _ queries: [[Int]]) -> [Int] {
    var board = Array(repeating: Array(repeating: 0, count: columns), count: rows)
    var count = 1
    var minimums = [Int]()
    
    for i in 0..<rows {
        for j in 0..<columns {
            board[i][j] = count
            count += 1
        }
    }
    
    for query in queries {
        let row1 = query[0]-1
        let column1 = query[1]-1
        let row2 = query[2]-1
        let column2 = query[3]-1
        var coordinates = [(Int, Int)]()
        
        for column in column1..<column2 {
            coordinates.append((row1, column))
        }
        for row in row1..<row2 {
            coordinates.append((row, column2))
        }
        for column in (column1+1...column2).reversed() {
            coordinates.append((row2, column))
        }
        for row in (row1+1...row2).reversed() {
            coordinates.append((row, column1))
        }
        
        var numbers = [Int]()
        for coordi in coordinates {
            numbers.append(board[coordi.0][coordi.1])
        }
        minimums.append(numbers.min()!)
        numbers.insert(numbers.removeLast(), at: 0)
        
        for (number, coordi) in zip(numbers, coordinates) {
            board[coordi.0][coordi.1] = number
        }
    }
    return minimums
}
```

- 오랜만에 혼자힘으로 푼 문제여서 감회가 새롭다고 할 수 있겠습니다.
- 먼저 입력으로 들어오는 `rows`와 `columns`에 맞게 이중배열 `board`를 생성해 주었습니다.
- `queries`의 요소에 접근하여 시계방향으로의 좌표값 배열을 구해주었습니다.
- 좌표값을 순회하여 좌표값 위치의 `board`의 값을 `numbers`배열에 추가해줍니다.
- 그중 최소값을 `minimums`에 추가해줍니다.
- `numbers`의 마지막 값을 맨 앞으로 옮겨줍니다.
- 옮겨진 `numbers` 값들을 좌표값 순서에 맞게 `board`에 재할당해줍니다.
- 마지막으로 `minimums`을 반환합니다.

# 다른 풀이

```swift
func makeBoard(_ rows: Int, _ columns: Int) -> [[Int]] {
    var board = [[Int]](repeating: [], count: rows)
    
    for i in 0..<board.count {
        let start = i * columns + 1
        board[i] = [Int](start..<start+columns)
    }
    return board
}

func solution(_ rows: Int, _ columns: Int, _ queries: [[Int]]) -> [Int] {
    var board = makeBoard(rows, columns)
    
    func rotateAndFindMinimum(_ p1: (Int, Int), _ p2: (Int, Int)) -> Int {
        var minimum = Int.max
        var pos = p1
        var before = board[pos.0][pos.1]
        var phase = 0
        
        let row = [0, 1, 0, -1]
        let column = [1, 0, -1, 0]
        
        while phase < 4 {
            pos = (pos.0+row[phase], pos.1+column[phase])
            
            if before < minimum {
                minimum = before
            }
            
            let temp = board[pos.0][pos.1]
            board[pos.0][pos.1] = before
            before = temp
            
            let nextRow = pos.0 + row[phase]
            let nextCol = pos.1 + column[phase]
            if case (p1.0...p2.0) = nextRow, case (p1.1...p2.1) = nextCol {
                continue
            } else {
                phase += 1
            }
        }
        return minimum
    }
    
    var answer = [Int]()
    for q in queries {
        answer.append(rotateAndFindMinimum((q[0]-1, q[1]-1), (q[2]-1, q[3]-1)))
    }
    return answer
}
```

- 전에 짠 코드는 시간이 아쉽게 느껴져서 다른 코드를 찾아보았습니다.
- 하나씩 위치를 옮겨가면서 값을 옮겨주는 코드를 공부하였는데, 시간이 훨씬 더 빨리 동작하는걸 알수 있었습니다.
- `[Int](start..<start+column)`으로도 배열을 만들수 있는게 신기방기하였습니다.
- `Int.max`로 `Int`타입의 제일 큰값을 가져올수 있다는 걸 알게 되었습니다.(`min`도 가능)
- 미리 `row = [0, 1, 0, -1]`, `column = [1, 0, -1, 0]`으로 값을 할당해두어 이동이 필요할시 필요한 값에 첨자접근하여 사용합니다!
- 처음보는 문법이 있었는데 `if case`문이 아주 신박하여 기록하게 되었습니다.
    - `(p1.0...p2.0)`의 범위에 `nextRow`가 있고, `(p1.1...p2.1)`의 범위에 `nextCol`이 있는 경우에만 `continue`합니다.
    - 범위에 없는 경우에는 `phase`에 `+1`을 해주어 다른방향 이동시작!
- 확실히 전에 짠 코드에는 무지성 `for`문이 가득한데, 이런식 하는 것이 더 개발자스럽고 좋아보입니다!
