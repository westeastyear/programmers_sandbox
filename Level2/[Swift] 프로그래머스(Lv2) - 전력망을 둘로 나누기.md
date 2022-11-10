# [Swift] 프로그래머스(Lv2) - 전력망을 둘로 나누기

[문제출처 프로그래머스 - 전력망을 둘로 나누기](https://school.programmers.co.kr/learn/courses/30/lessons/86971)

# 다른 풀이

```swift
func solution(_ n: Int, _ wires: [[Int]]) -> Int {
    var states = Array(repeating: Array(repeating: false, count: n+1), count: n+1)
    var result = Int.max
    
    wires.forEach {
        states[$0[0]][$0[1]] = true
        states[$0[1]][$0[0]] = true
    }
    
    func numberOfConnections(startIndex: Int) -> Int {
        var visited = Array(repeating: false, count: n+1)
        var queue = [startIndex]
        var count = 1
        
        while !queue.isEmpty {
            let now = queue.removeFirst()
            visited[now] = true
            
            for (index, state) in states[now].enumerated() {
                if state && !visited[index] {
                    queue.append(index)
                    count += 1
                }
            }
        }
        return count
    }
    
    wires.forEach {
        states[$0[0]][$0[1]] = false
        states[$0[1]][$0[0]] = false
        let count1 = numberOfConnections(startIndex: $0[0])
        let count2 = numberOfConnections(startIndex: $0[1])
        states[$0[0]][$0[1]] = true
        states[$0[1]][$0[0]] = true
        result = min(result, abs(count1 - count2))
    }
    return result
}
```

- 먼저 각 송전탑끼리의 연결상태를 이중배열로 정의합니다.
    - `n+1`개의 `false`를 가진 배열을 `n+1`개 만들어줍니다. 송전탑번호와 인덱스번호를 동일하게 생각하기 위해 `n+1`를 해주는 거라고 이해하고 있습니다.
    - 전선정보 `wires`를 순회하여 송전탑들이 서로 연결되어 있음을 나타내기 위해 `true`를 할당합니다. 서로 연결되어있는 것이기 때문에 왕복으로 생각해주어야 합니다.
    - 그러면 주어지는 모든 전선정보로, 모든 송전탑이 연결되어있는 상태를 나타내는 `states` 이중배열이 만들어집니다.
- 문제 설명대로 전선을 하나씩 끊어보면서 송전탑 개수가 가능한 비슷하도록 두개로 나누었을때를 찾아보겠습니다.
    - `wires`를 다시 순회하여 `states`에 `false`를 할당함으로 송전탑간의 연결을 끊어줍니다.
    - `numberOfConnections`에 순회하고 있는 `$0[0]`, `$0[1]`를 인자로 넣어주어 각각 `count1`, `count2`에 할당합니다.
    - 다시 `states`에 `true`를 할당하여 송전탑들을 연결해줍니다.
    - `count1`과 `count2`간의 차이를 절대값으로 변환후, `result`와 비교하여 그중 더 작은값을 `result`에 할당합니다.
- 마지막으로 `result`를 반환합니다!

### numberOfConnections(startIndex: Int) -> Int

```swift
func numberOfConnections(startIndex: Int) -> Int {
    var visited = Array(repeating: false, count: n+1)
    var queue = [startIndex]
    var count = 1
        
    while !queue.isEmpty {
        let now = queue.removeFirst()
        visited[now] = true
            
        for (index, state) in states[now].enumerated() {
            if state && !visited[index] {
                queue.append(index)
                count += 1
            }
        }
    }
    return count
}
```

- 각 송전탑에 방문했는지를 알기위해 `n+1`개의 `false`를 가진 `visited`배열을 정의합니다.
- `queue`에는 인자로 들어오는 `startIndex`로 초기화합니다.
- `queue`에 값이 있다면 `while` 반복문을 반복하게 됩니다.
    - `dequeue`하여 `now`에 할당하고, 송전탑에 방문했음을 표시하기 위해 `visited[now]`에 `true`를 할당합니다.
    - 그러면 현재 탐색하고 있는 송전탑과 연결되어 있는 다른 송전탑을 알아야겠죠?
    - 연결을 끊어놓은 `states`에 탐색하고 있는 송전탑번호(`[now]`)로 접근하여 다른 송전탑과의 연결상태를 순회합니다.
    - 만약 연결되어있는 송전탑이 있고(`state == true`) 방문한적이 없는 송전탑이 있다면(`!visited[index] == false`) `queue`에 인덱스번호(== 송전탑번호)를 추가해줍니다.
    - `count`도 `+1`해줍니다.
- `queue`에 값이 없다면 반복문을 종료하고, 연결되어있는 갯수인 `count`를 반환합니다!
