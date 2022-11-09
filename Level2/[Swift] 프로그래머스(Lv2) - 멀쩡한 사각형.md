# [Swift] 프로그래머스(Lv2) - 멀쩡한 사각형

[문제출처 프로그래머스 - 멀쩡한 사각형](https://school.programmers.co.kr/learn/courses/30/lessons/62048)

# 나의 풀이

```swift
func solution(_ w: Int, _ h: Int) -> Int64 {
    var sum: Int64 = 0
    
    for i in 1..<w {
        let i = Double(i)
        let inclination = Double(h) / Double(w)
        sum += Int64(inclination * i)
    }
    return sum * 2
}
```

- 좌표평면(2사분면)에서 대각선를 그려준다고 생각하여 갯수를 구해주는 방식입니다. 이는 곧 일차함수를 활용한 것이라고 할 수 있습니다.
- 대각선 $y = (h/w)x$ 입니다. 반복문의 범위는 `1..<w`(`== w-1`)까지 설정합니다.
- `x`가 1일때 `y`는 1.5가 나옵니다. 하지만 우리에게 필요한 것은 높이가 1인 정사각형입니다. 즉, `y`를 정수로 바꾼값이 정사각형의 최대 높이이자 최대 개수인 것입니다.
- 그런데 이렇게 작성하게 되면 테케 6번을 통과하지 못합니다. 그 이유는…
    - “소수점을 가진 값의 계산이 조금 부정확합니다. 예를 들면 0.1 + 0.2 = 0.300000000000000000004 처럼 나오는데요. 이 말은 소수점을 가진 값을 계산을 할수록 더 부정확해진다는 것입니다.
    - `h/w`를 나눠서 `i`를 곱하는 것은 `h/w`라는 소수점을 가진 값에 어떤 값을 곱해서 또 소수점을 가진 값을 만드는 것입니다. 차라리 `h`에 `i`를 먼저 곱한뒤 `w`로 나눠보세요. 그럼 계산이 더 정확해여서 통과합니다.”

# 리펙토링

```swift
// Double
func solution(_ w: Int, _ h: Int) -> Int64 {
    var sum: Int64 = 0
    
    if w == 1 || h == 1 { return 0 }
    
    for i in 1..<w {
        let h = Double(h), w = Double(w), i = Double(i)
        sum += Int64((h * i) / w)
    }
    return sum * 2
}

// Int
func solution(_ w: Int, _ h: Int) -> Int64 {
    var sum: Int64 = 0
    
    if w == 1 || h == 1 { return 0 }
    
    for i in 1..<w {
        sum += Int64((h * i) / w)
    }
    return sum * 2
}
```

- `Double`타입으로 바꾸지 않아도 되는데, `Int`타입으로 하니 속도차이가 많이 나는걸 알게 되었습니다.

### Double 타입으로 한 경우 / Int 타입으로 한 경우

<img src="https://user-images.githubusercontent.com/74251593/200758499-f01ba494-1b7a-4e4a-b4b5-7a8cd0b98248.png" width="49%"> <img src="https://user-images.githubusercontent.com/74251593/200758508-9e9c457f-3830-42df-b6a4-310a6adc28b2.png" width="49%">

# 다른 풀이

```swift
func gcd(_ a: Int, _ b: Int) -> Int {
    return b == 0 ? a : gcd(b, a % b)
}

func solution(_ w: Int, _ h: Int) -> Int64 {    
    return Int64((w * h) - ((w + h) - gcd(w, h)))
}
```

- 최대공약수를 활용해서 하니 깜쪽같이 끝나버렸습니다...!
- 멀쩡한 사각형의 갯수 구하는 방법은 전체 사각형에서 빗선이 지나가는 사각형의 갯수를 빼주면 됩니다.
- 빗선이 지나는 사각형의 객수를 구하는 방법은…
    - `가로 + 세로 - (가로세로의 최대공약수)`
- `전체갯수(가로 * 세로) - (가로 + 세로 - (가로세로의 최대공약수))`
