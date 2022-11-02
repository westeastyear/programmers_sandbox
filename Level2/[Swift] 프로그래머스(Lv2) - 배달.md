# [Swift] 프로그래머스(Lv2) - 배달

[문제출처 프로그래머스 - 배달](https://school.programmers.co.kr/learn/courses/30/lessons/12978)

# 다익스트라(Dijkstra) 알고리즘: 모든 정점까지의 최단 경로 구하기

- 다익스트라 알고리즘은 그래프 내의 특정 정점에서 갈 수 있는 모든 정점들까지의 최단 경로를 구하는 알고리즘입니다.
- 방식이 효율적이기 때문에 그래프가 큰 경우에도 사용가능한 장점이 있습니다.
- 단, 그래프 내 간선의 가중치가 음수인 것이 하나라도 있다면 사용할 수 없습니다.

## 다른 풀이

```swift
func solution(_ N:Int, _ road:[[Int]], _ k:Int) -> Int {
    var allDistances = Array(repeating: Int.max, count: N+1)
    dijkstra(allDistances: &allDistances, road: road, start: 1)
    return allDistances.filter { $0 <= k }.count
}

func dijkstra(allDistances: inout [Int], road: [[Int]], start: Int) {
    allDistances[start] = 0
    var queue = [start]
    
    while !queue.isEmpty {
        let first = queue.removeFirst()
        let filter = road.filter { $0[0] == first || $0[1] == first }
        
        for f in filter {
            let other = f[0] == first ? f[1] : f[0]
            if allDistances[first]+f[2] < allDistances[other] {
                allDistances[other] = allDistances[first]+f[2]
                queue.append(other)
            }
        }
    }
}
```

- `Int.max`값을 `N+1`개 가지는 배열을 생성합니다.
    - 각 마을의 번호를 인덱스번호와 맞추어 헷갈리지 않기 위해 갯수를 `N+1`을 해주어 배열을 생성하였습니다.
- 시작마을인 `1`번부터 다익스트라 알고리즘을 시작하게 됩니다.
    - 자기자신과의 거리는 `0`이기에 `allDistances[first]`에 `0`을 할당합니다.
    - 시작인덱스 `1`을 담은 `queue`를 만들어줍니다.
- `queue`에 값이 없을때까지 반복하게 됩니다.
- `dequeue`하여 `first`에 할당하고, `road`의 요소중에 `$0[0]`, `$0[1]`값이 `first`와 같은 경우를 걸러내줍니다.
    - 그래프가 그려진 모습을 보았을때, `first`마을과 연결되어있는 모든 마을을 추출하는 과정입니다.
- 추출한 마을의 도로정보를 순회합니다.
    - 첫번째 값인 `f[0]`가 `first`와 같다면 두번째 값인 `f[1]`값을 `other`에 할당합니다.
    - 다르다면 첫번째 값인 `f[0]`을 `other`에 할당합니다.
        - `other`는 연결되어있는 마을입니다.
- `allDistances[first]`에 걸리는 시간 `f[2]`를 더한값이 `allDistances[other]` 값보다 작은경우, `allDistances[other]`에 앞서 더한값을 업데이트해줍니다.
- `queue`에 다음 탐색대상인 `other`를 추가해줍니다.
- 반복이 끝나면 `allDistances`에서 `k`이하인 값들의 갯수를 반환합니다!
