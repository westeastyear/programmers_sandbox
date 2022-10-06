# [Swift] 프로그래머스(Lv2) - [1차] 프렌즈4블록

[문제출처 프로그래머스 - [1차] 프렌즈4블록](https://school.programmers.co.kr/learn/courses/30/lessons/17679)

# 나의 풀이

```swift
func findCoordinates(_ board: [[String]]) -> [[Int]] {
    var storage = [[Int]]()
    
    for i in 0..<board.count-1 {
        for j in 0..<board[i].count-1 {
            let upperLeft = board[i][j]
            let upperRight = board[i][j+1]
            
            if upperLeft != "*" && upperLeft == upperRight {
                let bottomLeft = board[i+1][j]
                let bottomRight = board[i+1][j+1]
                
                if upperLeft == bottomLeft && upperLeft == bottomRight {
                    let coordinates = [[i, j], [i, j+1], [i+1, j], [i+1, j+1]]
                    
                    coordinates.forEach {
                        if !storage.contains($0) {
                            storage.append($0)
                        }
                    }
                }
            }
        }
    }
    return storage
}

func moveText(_ board: [[String]], _ coordinates:[[Int]]) -> [[String]] {
    var board = board
    var signal = true
    
    coordinates.forEach {
        board[$0[0]][$0[1]] = "*"
    }
    
    while signal {
        let copyBoard = board
        
        for i in 0..<board.count {
            for j in 0..<board.count-1 {
                
                if board[j+1][i] == "*" {
                    board[j+1][i] = board[j][i]
                    board[j][i] = "*"
                }
            }
        }
        signal = copyBoard != board
    }
    return board
}

func solution(_ m: Int, _ n: Int, _ board: [String]) -> Int {
    var board = board.map { $0.map { String($0) } }
    var signal = true
    
    while signal {
        let copyBoard = board
        let coordinates = findCoordinates(board)
        board = moveText(board, coordinates)
        signal = copyBoard != board
    }
    
    return board.map { $0.filter { $0 == "*" }.count }.reduce(0, +)
}
```

- 처음에는 전혀 감을 잡을 수가 없어 열심히 찾아보고 직접 손코딩 해보며 코드의 흐름을 익혀나갔다.
- 시간이 해결을 해주었는지 계속 붙잡고 있으니까 흐름이 눈에 보이기 시작하면서 마무리 할 수 있었다…

<br>

```swift
func findCoordinates(_ board: [[String]]) -> [[Int]] {
    var storage = [[Int]]()
    
    for i in 0..<board.count-1 {
        for j in 0..<board[i].count-1 {
            let upperLeft = board[i][j]
            let upperRight = board[i][j+1]
            
            if upperLeft != "*" && upperLeft == upperRight {
                let bottomLeft = board[i+1][j]
                let bottomRight = board[i+1][j+1]
                
                if upperLeft == bottomLeft && upperLeft == bottomRight {
                    let coordinates = [[i, j], [i, j+1], [i+1, j], [i+1, j+1]]
                    
                    coordinates.forEach {
                        if !storage.contains($0) {
                            storage.append($0)
                        }
                    }
                }
            }
        }
    }
    return storage
}
```

- 입력으로 주어지는 이중배열 `board`에서 `2*2` 형태가 동일한 문자라면 그 좌표를 찾아내는 `findCoordinates()` 메서드를 정의하였습니다.
- 행(`i`)과 열(`j`)의 범위는 `upperLeft`를 기준으로 `2*2` 영역을 만들었을때 범위가 벗어나지 않도록 `-1`씩 해주었습니다.
- `2*2`영역 → `[i, j]`, `[i, j+1]`, `[i+1, j]`, `[i+1, j+1]`
- `storage`에 없는 좌표만 추가하도록하여 겹치는 좌표가 중복되지 않도록 예외처리해 주었습니다.

<br>

```swift
func moveText(_ board: [[String]], _ coordinates:[[Int]]) -> [[String]] {
    var board = board
    var signal = true
    
    coordinates.forEach {
        board[$0[0]][$0[1]] = "*"
    }
    
    while signal {
        let copyBoard = board
        
        for i in 0..<board.count {
            for j in 0..<board.count-1 {
                
                if board[j+1][i] == "*" {
                    board[j+1][i] = board[j][i]
                    board[j][i] = "*"
                }
            }
        }
        signal = copyBoard != board
    }
    return board
}
```

- `board`와 `coordinates`를 받아서 해당 좌표의 문자를 `“*”`로 변경하고 문자의 위치를 조정하는 `moveText()` 메서드를 정의하였습니다.
- `2*2` 영역에서 `bottomLeft`의 문자가 `“*”`이라면, 위의 `upperLeft`와 위치를 변경하는 작업을 하게 됩니다.
- `2*2` 영역이 행의 영역을 벗어나지 않도록 `-1`를 해주었습니다.
- 문자를 옮기기 전의 `copyBoard`와 문자를 옮긴 후의 `board`가 동일해서 옮길 문자가 없을 때까지 `while`문을 통해 반복하게 됩니다.

<br>

```swift
func solution(_ m: Int, _ n: Int, _ board: [String]) -> Int {
    var board = board.map { $0.map { String($0) } }
    var signal = true
    
    while signal {
        let copyBoard = board
        let coordinates = findCoordinates(board)
        board = moveText(board, coordinates)
        signal = copyBoard != board
    }
    
    return board.map { $0.filter { $0 == "*" }.count }.reduce(0, +)
}
```

- 작업전의 `copyBoard`와 작업후의 `board`가 동일해서 더 이상 작업할게 없을때까지 `while`문을 통해 반복하게 됩니다.
- 마지막으로 총 `“*”`의 갯수(지워지는 블록의 갯수)를 반환하여 마무리 합니다.
