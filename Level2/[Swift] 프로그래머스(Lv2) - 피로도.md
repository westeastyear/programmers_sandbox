# [Swift] 프로그래머스(Lv2) - 피로도

[문제출처 프로그래머스 - 피로도](https://school.programmers.co.kr/learn/courses/30/lessons/87946)

# 나의 풀이

```swift
func solution(_ k: Int, _ dungeons: [[Int]]) -> Int {
    var answer = 0
    
    func dfs(dungeons: [[Int]], k: Int, count: Int) {
        answer = max(count, answer)
        
        for (i, dungeon) in dungeons.enumerated() {
            var tempDungeons = dungeons
            
            if k >= dungeon[0] {
                let newK = k - dungeon[1]
                tempDungeons.remove(at: i)
                dfs(dungeons: tempDungeons, k: newK, count: count+1)
            }
        }
    }
    dfs(dungeons: dungeons, k: k, count: 0)
    return answer
}
```

- 남의 코드 안보고는 너무 어려워요!
- 전에 타겟넘버 풀었을때와 비슷한 스타일로 작성해보았습니다.
- 문제분류가 완전탐색으로 되어있어서 “`DFS`로 탐색을 하면 되겠구나!”까지 생각을 할순 있었지만, 구현하는데에는 손가락이 떨어지지가 않았습니다…
- 언젠간 눈에 익어 문제가 술술풀리는 날이 오긴 오겠죠…?(엉엉)
- `dungeons`의 요소에 하나씩 접근합니다.
- `dungeons`의 복사본인 `tempDungeons`를 생성합니다.
- 현재 피로도(`k`)가 최소 필요 피로도(`dungeon[0]`) 이상인 경우 해당 던전을 탐험할 수 있게 됩니다.
- 현재 피로도(`k`)에서 소모 피로도(`dungeon[1]`)를 빼주어 `newK`에 할당합니다.
- 던전은 한 번씩만 탐험할 수 있기 때문에 `tempDungeons`에서 탐험한 던전을 삭제해줍니다.
- `tempDungeons`, `newK`, `count+1`를 매개변수로 `dfs()` 함수를 재귀호출하여 모든 던전을 조건에 맞게 탐험할때까지 반복하게 됩니다.
- 유저가 탐험할 수 있는 최대 던전 수를 반환합니다.
