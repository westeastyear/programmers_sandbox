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

```

- 제가 짠 코드는 시간이 아쉽게 느껴져서 다른 코드를 찾아보았습니다.
