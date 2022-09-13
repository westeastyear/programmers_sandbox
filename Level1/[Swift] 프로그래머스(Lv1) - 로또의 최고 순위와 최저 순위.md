# [Swift] 프로그래머스(Lv1) - 로또의 최고 순위와 최저 순위

[문제출처 프로그래머스 - 로또의 최고 순위와 최저 순위](https://school.programmers.co.kr/learn/courses/30/lessons/77484)

# 나의 풀이

```swift
func returnRank(_ n: Int) -> Int {
    switch n {
    case 6:
        return 1
    case 5:
        return 2
    case 4:
        return 3
    case 3:
        return 4
    case 2:
        return 5
    default:
        return 6
    }
}

func solution(_ lottos: [Int], _ win_nums: [Int]) -> [Int] {
    let answers = lottos.filter { win_nums.contains($0) }.count
    let zeros = lottos.filter { $0 == 0 }.count

    let topRank = returnRank(answers + zeros)
    let lowRank = returnRank(answers)
    
    return [topRank, lowRank]
}
```

- 로또번호가 일치하는 갯수만큼 순위를 반환하는 함수를 만들어서 사용하였다.
- 경우의 수는 `0`이 다 맞는 경우와 다 틀릴 경우만 생각하면 된다.
- 다 맞는 경우의 순위는 로또번호가 일치하는 갯수에 0의 갯수만큰 더해준 순위이다.
- 다 틀린 경우의 순위는 로또번호가 일치하는 수의 갯수만큼의 순위이다.

# 다른 풀이

```swift
func solution(_ lottos: [Int], _ win_nums: [Int]) -> [Int] {
    let answers = lottos.filter { win_nums.contains($0) }.count
    let zeros = lottos.filter { $0 == 0 }.count
    
    return [min(7 - answers - zeros, 6), min(7 - answers, 6)]
}
```

- 나의 풀이처럼 함수를 작성하지 않고 `min()` 메서드를 활용한 코드이다.
- 7에 다 맞는 경우의 수(`answers + zeros`)를 빼주면 1이 나오게 되는데 이걸 순위로 활용할 수가 있다?!?!
- 아주 신박한 생각이라고 할 수 있겠다👍
