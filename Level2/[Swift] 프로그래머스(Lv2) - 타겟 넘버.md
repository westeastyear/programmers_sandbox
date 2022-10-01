# [Swift] 프로그래머스(Lv2) - 타겟 넘버

[문제출처 프로그래머스 - 타겟 넘버](https://school.programmers.co.kr/learn/courses/30/lessons/43165)

# 다른 풀이

```swift
func solution(_ numbers: [Int], _ target: Int) -> Int {
    var count = 0
    
    func dfs(index: Int, sum: Int) {
        if index == numbers.count {
            if sum == target {
                count += 1
            }
            return
        }
        dfs(index: index + 1, sum: sum + numbers[index])
        dfs(index: index + 1, sum: sum - numbers[index])
    }
    
    dfs(index: 0, sum: 0)
    
    return count
}
```

- `DFS`, `BFS`는 처음이라… 좀 힘들었다고 할 수 있겠다.
- 직접 그림을 그려보면서 이해하느라 오래걸렸었다.
- 배열의 숫자들이 `+`인 경우와 `-`인 경우를 자식 노드로 놓고 `DFS`방식으로 풀면 된다.
- 트리의 각 레벨을 `numbers` 배열의 `index`로 보면 된다.
- `dfs()` 메서드는 다음 `index`의 수를 더하거나 빼주며 재귀적으로 호출한다.
- 자식노드가 더 이상 없는 경우(= `index+1`이 `numbers` 배열의 `count`와 같은 경우) 지금까지 계산한 값과 `target`을 비교하여 같은지 판단한다.
