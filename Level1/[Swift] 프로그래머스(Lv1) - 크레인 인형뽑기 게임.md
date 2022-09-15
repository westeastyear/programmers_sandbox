# [Swift] 프로그래머스(Lv1) - [크레인 인형뽑기 게임]

[문제출처 프로그래머스 - 크레인 인형뽑기 게임](https://school.programmers.co.kr/learn/courses/30/lessons/64061)

# 나의 풀이

```swift
func solution(_ board: [[Int]], _ moves: [Int]) -> Int {
    var board = board
    var stack = [Int]()
    var count = 0
    
    for move in moves {
        let arrayByMovesNumber = board.map { $0[move-1] }
        let pickedNumber = arrayByMovesNumber.filter { $0 != 0 }.first ?? 0
        let index = arrayByMovesNumber.firstIndex(of: pickedNumber)!
        
        board[index][move-1] = 0
        stack = stack.filter { $0 != 0 }
        
        if stack.last == pickedNumber {
            stack.removeLast()
            count += 2
        } else {
            stack.append(pickedNumber)
        }
    }
    return count
}
```

- 나의 힘으로 풀어서 기분이 좋다!!
- `board`의 요소에서 각 인덱스별로 배열을 만들어 주었다. → `arrayByMovesNumber`
- `arrayByMovesNumber`에서 `0`이 아닌 숫자들중 첫번째 요소를 가져온다.
    - 모든 요소가 `0`일 경우에는 값이 없으므로 `0`을 반환하게 하였다.
- `arrayByMovesNumber`에서 `pickedNumber`의 `index`를 찾아주었다. → `board`에서 활용
- `board`에서 앞에서 구해준 `index`를 활용해 `pickedNumber`에 접근하여 값을 `0`으로 업데이트 해주었다.
- `stack`에 있을 수도 있는 `0`을 걸러주고 업데이트 한다.
- `stack`의 마지막 요소와 `pickedNumber`가 같다면, `stack`에서 지워주고 `count`를 `+2`해준다.
- 다르거나 없다면 `stack`에 `pickedNumber`를 추가해준다.
