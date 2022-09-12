# [Swift] 프로그래머스(Lv1) - 모의고사

[문제출처 프로그래머스 - 모의고사](https://school.programmers.co.kr/learn/courses/30/lessons/42840)

# 나의 풀이

```swift
func solution(_ answers: [Int]) -> [Int] {
    let a = [1,2,3,4,5]
    let b = [2,1,2,3,2,4,2,5]
    let c = [3,3,1,1,2,2,4,4,5,5]
    
    var temp = [0,0,0]
    var result = [Int]()
    
    for i in 0..<answers.count {
        if a[i % a.count] == answers[i] {
            temp[0] += 1
        }
        if b[i % b.count] == answers[i] {
            temp[1] += 1
        }
        if c[i % c.count] == answers[i] {
            temp[2] += 1
        }
    }
    let max = temp.max()
    for i in 0..<temp.count {
        if max == temp[i] {
            result.append(i + 1)
        }
    }
    return result
}
```

- 제한조건이 최대 10,000문제이기 때문에 엣지케이스를 고려하여 작성하였습니다.
- 각각의 수포자의 찍는 방식의 규칙을 고려하여 배열`(a, b, c)`을 만들어주었습니다.
- `answers`의 갯수만큼 반복되는 `i`를 각 수포자 배열`(a, b, c)`의 갯수로 각각의 나머지를 구하고, 그 나머지로 인덱싱하여 `answer[i]`와 값을 비교합니다.
- 답이 일치한다면 카운트를 올려줍니다.
- `temp.max()`로 최대값을 구해줍니다.
- 반복문을 통해 가장 많은 문제를 맞힌 사람을 찾아줍니다.
- 가장 높은 점수를 받은 사람이 여럿일 경우, 오름차순으로 정렬합니다.

# 다른 풀이

```swift
func solution(_ answers: [Int]) -> [Int] {
    
    let answer = (
        a: [1,2,3,4,5],
        b: [2,1,2,3,2,4,2,5],
        c: [3,3,1,1,2,2,4,4,5,5]
    )
    var point = [1:0, 2:0, 3:0]
    
    for (i, v) in answers.enumerated() {
        if v == answer.a[i % 5] {
            point[1]! += 1
        }
        if v == answer.b[i % 8] {
            point[2]! += 1
        }
        if v == answer.c[i % 10] {
            point[3]! += 1
        }
    }
    return point.sorted { $0.key < $1.key }.filter { $0.value == point.values.max() }.map { $0.key }
}
```

- 딕셔너리를 사용한 다른풀이, 딕셔너리도 좋은 방법이다!
- 방식은 나의 풀이와 비슷하다👍
- `point`를 `key`기준으로 오름차순 정렬
- `filter`를 사용하여 각각의 `value`와 `value`의 최댓값과 비교
- 일치한다면 그 `key`값을 배열로 리턴
