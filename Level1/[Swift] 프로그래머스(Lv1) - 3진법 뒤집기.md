# 나의 풀이

```swift
func solution(_ n: Int) -> Int {
    return Int(String(String(n, radix: 3).reversed()), radix: 3)!
}
```

# 나의 풀이 2

```swift
func solution(_ n: Int) -> Int {
    let a = String(n, radix: 3).reversed()
    return Int(String(a), radix: 3)!
}
```

- 첫번째꺼랑 다를게 없다. 대동소이

### 10진수 → 2진수

```swift
String(10진수, radix: 2)
```

- 10진수: `Int`
- 2진수 변환 결과값: `String`

### 2진수 → 10진수

```swift
Int(2진수, radix: 2)
```

- 2진수: `String`
- 10진수 변환 결과값: `Int?`
- 숫자로 바꿀 수 없는 문자일때 `nil`로 처리되기 때문에 결과값이 옵셔널로 반환됨
- radix: 뒤에 다른 숫자(n)를 넣어주면 다른 n진수로 변환이 가능하다.
