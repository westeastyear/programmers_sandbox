# [Swift] 프로그래머스(Lv1) - 체육복

[문제출처 프로그래머스 - 체육복](https://school.programmers.co.kr/learn/courses/30/lessons/42862)

# 다른 풀이

```swift
func solution(_ n: Int, _ lost: [Int], _ reserve: [Int]) -> Int {
    let losted = lost.filter { !reserve.contains($0) }
    let reserved = reserve.filter { !lost.contains($0) }
    var lostedPeople = losted.count
    
    reserved.forEach {
        let isBool: Bool = losted.contains($0+1) || losted.contains($0-1)
        if isBool && lostedPeople > 0 {
            lostedPeople -= 1
        }
    }
    return n - lostedPeople
}
```

- 여벌 체육복을 가져온 학생도 도난 당할 수 있기 때문에 `lost`와 `reserve`배열에 중복되는 학생의 번호를 제거해서 새로 할당해 줍니다.
- 중복된 학생은 제외한 채로 카운트를 해줍니다.
- `reserved`의 요소를 `+1`, `-1`한 값이 `losted`에 OR 논리로 포함된다면 `true`를 반환하는 상수를 할당합니다.
- `isBool`이 `true`이고 `lostedPeople`이 `0`초과인 경우 `lostedPeople`을 `1`차감해줍니다.
- 마지막으로 전체 학생수`n`에서 차감된 `lostedPeople`을 빼주어 체육수업을 들을수 있는 학생의 최댓값을 반환해줍니다.
