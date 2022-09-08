# [Swift] 프로그래머스(Lv1) - 최소직사각형

[문제출처 프로그래머스 - 최소직사각형](https://school.programmers.co.kr/learn/courses/30/lessons/86491)

# 다른 풀이

```swift
func solution(_ sizes: [[Int]]) -> Int {
    var width = [Int]()
    var height = [Int]()
    
    sizes.forEach { size in
        let size = size.sorted()
        width.append(size[0])
        height.append(size[1])
    }
    return width.max()! * height.max()!
}
```
- 처음에는 감이 안잡혀서 포인트를 잘 못 잡아서 두시간 동안 헤매다가 결국 찾아보게 되었는데 너무나 간단해서 어이가 없었다... 등잔 밑이 어둡다🕯(내 미래도..ㅠ)
- `width`와 `height`가 담긴 배열을 오름차순으로 정렬한다. → 명함을 회전시켜 세로로 길게 놓는것과 마찬가지 이다.
- `width`와 `height`의 가장 큰 값을 곱한다. → 가로의 최댓값, 세로의 최댓값을 곱하면 모든 명함이 들어갈 수 있는 지갑이 만들어진다!!
