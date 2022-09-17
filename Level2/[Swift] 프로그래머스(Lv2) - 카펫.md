# [Swift] 프로그래머스(Lv2) - 카펫

[문제출처 프로그래머스 - 카펫](https://school.programmers.co.kr/learn/courses/30/lessons/42842)

# 나의 풀이

```swift
func returnDivisor(_ n: Int) -> [Int] {
    return (1...n).filter { n % $0 == 0 }
}

func filterWidthLength(_ ascending: [Int], _ descending: [Int]) -> [(Int, Int)] {
    let arr = (0..<ascending.count).map { (ascending[$0], descending[$0]) }
    return arr.filter { $0.0 >= $0.1 }
}

func solution(_ brown: Int, _ yellow: Int) -> [Int] {
    let ascending = returnDivisor(yellow)
    let descending = returnDivisor(yellow).sorted(by: >)
    let widthLength = filterWidthLength(ascending, descending)
    var result = [Int]()
    
    widthLength.forEach {
        let fullSize = ($0.0 + 2) * ($0.1 + 2)
        let yellowSize = $0.0 * $0.1
        if fullSize - yellowSize == brown {
            result.append($0.0 + 2)
            result.append($0.1 + 2)
        }
    }
        
    return result
}
```

- 문제를 풀다가 기능분리 병이 도져서 이것저것 메서드를 만들어 풀어보았다.
- 다른 코드들를 보아하니 이렇게 까지 할 필요는 없을거 같다는 느낌이 들었다.
- `n`의 약수 배열을 만들어주는 메서드를 만들었다.
- 약수 배열의 내림차순 오름차순을 받아서 튜플형식으로 가로세로를 만들고, 가로가 세로와 같거나 세로보다 큰 경우를 걸러주는 메서드를 만들었다.
- 가로세로에 접근해서 `yellowSize`를 구해주고, 가로세로에 `+2`한 값으로 `fullSize`를 구해주었다.
- `fullSize`에서 `yellowSize`를 뺀 값이 `brown`과 같다면, 해당 가로세로에 `+2`한 값을 배열에 담에 리턴한다.

# 다른 풀이

```swift
func solution(_ brown: Int, _ yellow: Int) -> [Int] {
    var width = 0
    var height = 0
    let total = brown + yellow
    
    for i in 1...total {
        if total % i == 0 {
            width = total / i
            height = i
        }
        if (width - 2) * (height - 2) == yellow {
            break
        }
    }
    
    return [width, height]
}
```

- 이 얼마나 보기 좋은 코드인가!
- 나의 풀이로 문제를 풀면서도 이렇게 까지 했는데 나중에 다른 사람이 한거 보면서 충격먹을거 같다란 생각을 했었는데 역시는 역시군…ㅋ
- `brown`과 `yellow`를 합치면 전체 카펫의 사이즈를 알 수 있다?!?!
- `i`가 `total`의 약수라면 `if`문을 실행하게 된다.
- 보통이라면 오름차순의 순서로 `i`를 받게 되는데 다시 `total`을 `i`로 나누어서 내림차순으로 나오는 값을 `width`에 넣어주고 `height`에 `i`를 넣어준다.
- `brown`은 카펫의 가장자리에만 있기때문에 가로세로에 `-2`를 해주고, 뺀 값의 곱이 `yellow`와 같다면 `[width, height]` 반환한다.

# 다른 풀이 2

```swift
func solution(_ brown: Int, _ yellow: Int) -> [Int] {
    for i in (1...Int(sqrt(Double(yellow)))).reversed() {
        if (yellow / i + 2) * (i + 2) == brown + yellow {
            return [(yellow / i + 2), (i + 2)]
        }
    }
    return [0, 0]
}
```

- 똑똑한 사람들…👍
- 모든 약수를 구할 필요 없이 `yellow`의 제곱근까지 범위의 약수만 있으면 된다!
- `reversed()`까지 해주어서 큰 수부터 반복!
- 핵심적인 아이디어는 다른 코드들과 비슷하다!!
