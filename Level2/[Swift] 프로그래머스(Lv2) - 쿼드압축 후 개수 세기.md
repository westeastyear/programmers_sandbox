# [Swift] 프로그래머스(Lv2) - 쿼드압축 후 개수 세기

[문제출처 프로그래머스 - 쿼드압축 후 개수 세기](https://school.programmers.co.kr/learn/courses/30/lessons/68936)

# 다른 풀이

```swift
func solution(_ arr: [[Int]]) -> [Int] {
    var zeroCount = 0
    var oneCount = 0
    let elementCount = arr.first!.count
    
    func recursive(_ arr: [[Int]], _ row: Int, _ col: Int, _ n: Int) {
        let standard = arr[row][col]
        
        for i in row..<row+n {
            for j in col..<col+n {
                if standard != arr[i][j] {
                    recursive(arr, row, col, n/2)
                    recursive(arr, row+n/2, col, n/2)
                    recursive(arr, row, col+n/2, n/2)
                    recursive(arr, row+n/2, col+n/2, n/2)
                    return
                }
            }
        }
        if standard == 1 {
            oneCount += 1
        } else {
            zeroCount += 1
        }
    }
    recursive(arr, 0, 0, elementCount)
    return [zeroCount, oneCount]
}
```

- 디버거를 보면서 설명을 해야 이해가 잘되는데 재귀는 글로 설명하기 힘들어요…😅
- 코드 이해하려고 하루종일 디버거를 보며 흐름을 따라갔습니다.
- `standard`와 `arr[i][j]`가 다르다면 큰사분면에서 중간사분면으로 재귀가 들어가게 되고, 또 `standard`와 `arr[i][j]`가 다르다면 중간사분면에서 작은사분면의 재귀로 들어가며 탐색을 하는 방식입니다. (쿼드트리 알고리즘)
- `standard`와 `arr[i][j]`가 같다면 더 이상 재귀를 할 필요가 없으므로  1 또는 0의 `count`를 올려주게 됩니다.
