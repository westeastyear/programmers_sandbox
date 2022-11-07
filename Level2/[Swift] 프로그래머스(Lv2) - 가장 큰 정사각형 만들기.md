# [Swift] 프로그래머스(Lv2) - 가장 큰 정사각형 만들기

[문제출처 프로그래머스 - 가장 큰 정사각형 만들기](https://school.programmers.co.kr/learn/courses/30/lessons/12905)

# 다른 풀이

```swift
func solution(_ board: [[Int]]) -> Int {
    var board = board
    let rowCount = board.count
    let columnCount = board[0].count
    var result = 0
    
    if rowCount == 1 || columnCount == 1 {
        for row in 0..<rowCount {
            for col in 0..<columnCount {
                if board[row][col] == 1 {
                    return 1
                }
            }
        }
        return 0
    }
    
    for row in 1..<rowCount {
        for col in 1..<columnCount {
            if board[row][col] == 1 {
                board[row][col] += min(board[row-1][col-1], board[row][col-1], board[row-1][col])
            }
            result = max(result, board[row][col])
        }
    }
    return result * result
}
```

- 위 문제는 기가막힌 풀이법이 전해저 내려오고 있는 문제인거 같습니다.
- 저 또한 그 풀이법을 쓰윽 보고 유레카!를 외치며 풀수 있었습니다.
    - 이것이 `DP`기본유형(?)이라는것도 알게 되었습니다.
- `rowCount`또는 `columnCount`이 `1`인경우와 반환값이 `0`인경우을 예외처리 해주었습니다.
- 예전에 풀었던 프렌즈4블록이 떠오르는 문제였습니다. 이중배열에서 `2*2`형태로 사각형을 만들어 값을 탐색합니다.
    - `[row-1][col-1]`, `[row][col-1]`, `[row-1][col]`, `[row][col]`
- 위 인덱스 위치의 값들이 모두 `1`이상인 경우 정사각형을 만들수 있다는 뜻입니다!
- `board[row][col]`의 값이 `1`일때 조건문을 실행하게 됩니다.
    - 이중배열을 돌면서 `board[row][col]` 위치에 나머지 세개중 최솟값을 찾아 더해주는 것을 반복하게 되면 가장 큰 정사각형의 길이를 구할 수가 있습니다.
- 넓이를 구해야 하기 때문에 `result`를 제곱하여 반환합니다.
