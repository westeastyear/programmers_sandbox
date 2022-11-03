# [Swift] 프로그래머스(Lv2) - 배달

[문제출처 프로그래머스 - 배달](https://school.programmers.co.kr/learn/courses/30/lessons/12978)

# 다익스트라(Dijkstra) 알고리즘

- 다익스트라 알고리즘은 그래프 내의 하나의 정점에서 갈 수 있는 모든 정점들까지의 최단 경로를 구하는 알고리즘입니다.
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

## 리펙토링

```swift
func solution(_ N:Int, _ road:[[Int]], _ k:Int) -> Int {
    var distances = Array(repeating: Int.max, count: N+1)
    var queue = [1]
    
    distances[1] = 0
    while !queue.isEmpty {
        let first = queue.removeFirst()
        let filter = road.filter { $0[0] == first || $0[1] == first }
        
        for f in filter {
            let other = f[0] == first ? f[1] : f[0]
            if distances[first]+f[2] < distances[other] {
                distances[other] = distances[first]+f[2]
                queue.append(other)
            }
        }
    }
    return distances.filter { $0 <= k }.count
}
```

# 플로이드 와샬(Floyd-Warshall) 알고리즘

- 플로이드 와샬 알고리즘은 한번 실행하여 “모든 정점”에서 “모든 정점”으로의 최단 경로를 구하는 알고리즘입니다.
- 핵심 아이디어는 “거쳐가는 정점”을 기준으로 최단 거리를 구하는 것입니다.
- 다익스트라 알고리즘과 달리 간선의 가중치가 음수인 간선도 사용할 수 있습니다.

## 다른 풀이

```swift
func solution(_ N:Int, _ road:[[Int]], _ k:Int) -> Int {
    var distances = [[Int]](repeating: [Int](repeating: 99999999, count: N+1), count: N+1)
    
    for i in 1...N {
        distances[i][i] = 0
    }
    
    for r in road {
        distances[r[0]][r[1]] = min(r[2], distances[r[0]][r[1]])
        distances[r[1]][r[0]] = min(r[2], distances[r[1]][r[0]])
    }
    
    for k in 1...N {
        for i in 1...N {
            for j in 1...N {
                distances[i][j] = min(distances[i][j], distances[i][k] + distances[k][j])
            }
        }
    }
    return distances[1].filter { $0 <= k }.count
}
```

- 플로이드 와샬 알고리즘은 이중배열로 이루어져 있습니다.
    - 무한을 의미하는 `99999999`를 `N+1`개 가지고 있는 배열을, `N+1`개 가진 이중배열을 생성합니다.
    - 인덱스번호와 맞추어 생각하기 위해 `N+1`을 해주었습니다.
- 자기 자신에서 자신으로 가는 방법(`[i] → [i]`)의 가중치는 없기때문에 `0`으로 초기화 해줍니다.
- `road`의 요소를 순회하여 입력으로 주어지는 도로정보의 가중치를 입력해줍니다.
    - 연결된 도로가 2개 이상일수 있기 때문에 `min()`을 활용하여 더 작은 값을 넣어주게 됩니다.
- `k = 거쳐가는 노드,` `i = 출발 노드`, `j = 도착 노드`로 지정하여 플로이드 와샬을 수행합니다.
    - `i → j`로 가는 방법과 `i → k`, `k → j`처럼 `k`를 거쳐가는 방법을 비교하여 더 짧은 거리를 업데이트 해주게 됩니다.
- 이중배열에는 각 노드들이 다른 노드로 가는 최단거리를 저장하게 됩니다.
- `1`번 마을에 있는 음식점이 `K`이하의 시간에 배달이 가능한 마을의 갯수를 반환하는 것이 목적이기 때문에, `distances[1]`에서 조건에 맞는 갯수를 반환합니다.
