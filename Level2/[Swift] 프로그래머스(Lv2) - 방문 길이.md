# [Swift] 프로그래머스(Lv2) - 방문 길이

[문제출처 프로그래머스 - 방문 길이](https://school.programmers.co.kr/learn/courses/30/lessons/49994)

# 나의 풀이

```swift
func solution(_ dirs: String) -> Int {
    var now = [0, 0]
    var storage = Set<[Int]>()
    
    for dir in dirs {
        var (dx, dy) = (0, 0)
        
        switch dir {
        case "U": (dx, dy) = (0, 1)
        case "D": (dx, dy) = (0, -1)
        case "L": (dx, dy) = (-1, 0)
        case "R": (dx, dy) = (1, 0)
        default: break
        }

        let next = [now[0]+dx, now[1]+dy]
        if abs(next[0]) > 5 || abs(next[1]) > 5 { continue }
        
        if !storage.contains(now+next) && !storage.contains(next+now) {
            storage.insert(now+next)
        }
        now = next
    }
    return storage.count
}
```

- 현재의 좌표 `now`를 `[0, 0]`으로 할당하였습니다.
- `now`좌표와 `next`좌표를 저장할 `storage`를 `Set<[Int]>`타입으로 정의하였습니다.
    - `[n, n, n, n]` 위와 같은 형식으로 `now`와 `next`를 합쳐서 지나간 길을 저장하게 됩니다.
    - `Set`을 활용한 이유는 중복을 제거함과 동시에 배열과 달리 `contains()`의 시간 복잡도가 상수 시간복잡도 이기 때문입니다.
    - `Set`은 해쉬 테이블 자료구조입니다.
- `now`에 `dx`, `dy`값을 더해 다음 좌표(`next`)를 구해줍니다.
- 좌표평면의 경계가 있기 때문에 `next[0]` 또는 `next[1]`의 절대값이 5를 초과하게 되면 `continue`합니다.
- `storage`에 `now+next` 또는 `next+now`  좌표값이 없다면 처음 걸어본 길이므로 `storage`에 추가해줍니다.
- 매 반복마다 `now`에 `next`를 할당하여 현재의 좌표를 업데이트 해줍니다.
- 마지막으로 `stroage`의 갯수를 반환합니다.
