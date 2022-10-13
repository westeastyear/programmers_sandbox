# [Swift] 프로그래머스(Lv2) - 2개 이하로 작은 비트

[문제출처 프로그래머스 - 2개 이하로 작은 비트](https://school.programmers.co.kr/learn/courses/30/lessons/77885)

# 나의 풀이

```swift
func solution(_ numbers: [Int64]) -> [Int64] {
    var result = [Int64]()
    
    for number in numbers {
        var targetBit = String(number, radix: 2)
        var count = number
        var signal = true

        while signal {
            count += 1
            let convert = String(count, radix: 2)
            
            if targetBit.count != convert.count {
                targetBit = String(repeating: "0", count: convert.count-targetBit.count) + targetBit
            }
            
            var defferentCount = 0
            for i in zip(targetBit, convert) {
                if i.0 != i.1 {
                    defferentCount += 1
                }
            }
            signal = defferentCount > 2
        }
        result.append(count)
    }
    return result
}
```

- 처음에는 1씩 더해가면서 가장 처음 등장하는 비트의 차이가 1이나 2인 숫자를 찾아내었습니다.
- 전부다 비교하면서 구하는 방법인데 당연하게도 시간초과가 났습니다.

# 다른 풀이

```swift
func solution(_ numbers: [Int64]) -> [Int64] {
    return numbers.map {
        if $0 % 2 == 0 {
            return $0 + 1
        } else {
            let temp = ~$0 & ($0+1)
            return ($0|temp) & ~(temp>>1)
        }
    }
}
```

[문제 조건] - `x`보다 크고 `x`와 **비트가 1~2개 다른** 수들 중에서 제일 작은 수

| 수 | 비트 |
| --- | --- |
| 1 | 000…0001 |
| 2 | 000…0010 |
| 3 | 000…0011 |
| 4 | 000…0100 |
| 5 | 000…0101 |
| 6 | 000…0110 |
| 7 | 000…0111 |
| 8 | 000…1000 |
| 9 | 000…1001 |
| 10 | 000…1010 |
| 11 | 000…1011 |
- 입력이 짝수라면 위 문제조건에 충족하는 수는 입력 값에 `+1`을 한 값입니다.
- 입력이 홀수라면 해당 이진수 가장 오른쪽 `0`을 기준으로 잡,고 그 기준값을 `1`로 바꾼후, 기준에서 한칸 오른쪽의 `1`을 `0`으로 바꾸면 문제 조건에 충족하는 값을 찾을 수 있습니다.

 

### 예시

```swift
11 -> 000...1011
```

<br>

- 11을 `~`(NOT)연산자로 뒤집은 이진수와 다음수의 이진수로 `&`(AND)연산하여 가장 오른쪽 `0`의 위치를 찾습니다.
- `temp`에 할당해줍니다.

```swift
~11 -> 111...0100
 12 -> 000...1111
------------------
     & 000...0100

let temp = ~$0 & ($0+1)
```

<br>

- `temp`를 한칸 오른쪽으로 `Shift`해줍니다.
- `~` (NOT)연산자로 `temp>>1`를 뒤집어 줍니다.

```swift
temp -> 000...0100
-------------------
>> 1 -> 000...0010

temp>>1 -> 000...0010
----------------------
         ~ 111...1101
```

<br>

- 11과 `temp`를 `|`(OR)연산한 값과 `~(temp>>1)`값을 `&`(AND)연산하여 조건에 충족하는 값을 반환합니다.

```swift
        11 -> 000...1011
      temp -> 000...0100
-------------------------
            | 000...1111
~(temp>>1) -> 111...1101
-------------------------
            & 000...1101
```
