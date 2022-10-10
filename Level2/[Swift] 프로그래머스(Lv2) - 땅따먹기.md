# [Swift] 프로그래머스(Lv2) - 땅따먹기

[문제출처 프로그래머스 - 땅따먹기](https://school.programmers.co.kr/learn/courses/30/lessons/12913?language=swift)

# 나의 풀이

```swift
func solution(_ land: [[Int]]) -> Int{
    var land = land
    
    for i in 1..<land.count {
        land[i][0] += max(land[i-1][1], land[i-1][2], land[i-1][3])
        land[i][1] += max(land[i-1][0], land[i-1][2], land[i-1][3])
        land[i][2] += max(land[i-1][0], land[i-1][1], land[i-1][3])
        land[i][3] += max(land[i-1][0], land[i-1][1], land[i-1][2])
    }
    
    guard let last = land.last else { return 0 }
    return max(last[0], last[1], last[2], last[3])
}
```

- 처음에는 당연히 `DFS`를 활용해야 하는 줄 알아서 그림도 그려가면서 작성했었는데, 더 쉬운 방법이 있었습니다!!
- 문제설명에 N행 4열로 제한되어있기 때문에 반복문을 한번만 사용하고 하드코딩 하였습니다.
- 1행에서 밟은 열(인덱스)을 다음행에서 밟을 수 없다는 규칙때문에, 다음 행에서 1행에서 밟은 인덱스를 제외한 값중에 최대값을 더해주며 반복합니다.
- 반복하며 더해진 마지막 열의 배열에서 최대값을 반환합니다.
