# [Swift] 프로그래머스(Lv2) - 위장

[문제출처 프로그래머스 - 위장](https://school.programmers.co.kr/learn/courses/30/lessons/42578)

# 나의 풀이

```swift
func solution(_ clothes: [[String]]) -> Int {
    var dict = [String: Int]()
    
    clothes.map { $0[1] }.forEach {
        dict[$0, default: 0] += 1
    }
        
    return dict.reduce(1, { $0 * ($1.value + 1) }) - 1
}
```

- 옷의 종류별로 몇개의 옷이 있는지를 알아내기 위해 `Dictionary`로 저장하였다.
- `clothes`에 접근하여 옷의 종류로 `dict`을 구성한다. 
- 옷의 종류가 `key`값으로 존재한다면 `+1`을 해주고, 없다면 `default`값인 0으로 구성한다.
- 의상의 개수가 A, B, C 라면 → 전체 조합수의 개수는 `(A + 1) * (B + 1) * (C + 1) - 1` 입니다.
    - 하나씩 입는 경우도 포함되기 때문에 `n + 1`의 값을 곱해줍니다.
    - 1을 빼주는 이유는 옷을 안입는 경우(?)를 제외하기 위함입니다.
