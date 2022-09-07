# [Swift] 프로그래머스(Lv1) - [1차] 비밀지도

[문제출처 프로그래머스 - [1차] 비밀지도](https://school.programmers.co.kr/learn/courses/30/lessons/17681)

- 언제나 나 자신의 힘으로 문제를 풀어낸다는 것은 큰 성취감을 느끼게 합니다.
- 이진법으로 바꾸는 부분만 어떻게 하는지 몰라서 찾아본 후 풀어냈습니다.

# 나의 풀이

```swift
func convertBinary(_ n: Int, _ arr: [Int]) -> [String] {
    var binaryNumbers = arr.map { String($0, radix: 2) }
    binaryNumbers.forEach {
        if $0.count < n {
            let index = binaryNumbers.firstIndex(of: $0)!
            let zeros = String(repeating: "0", count: n - $0.count)
            let merged = zeros + binaryNumbers[index]
            binaryNumbers.remove(at: index)
            binaryNumbers.insert(merged, at: index)
        }
    }
    return binaryNumbers
}

func solution(_ n:Int, _ arr1:[Int], _ arr2:[Int]) -> [String] {
    var answer: [String] = []
    
    let arr1 = convertBinary(n, arr1).map { Array($0) }
    let arr2 = convertBinary(n, arr2).map { Array($0) }
    
    for i in 0..<n {
        var temp = ""
        for j in 0..<n {
            if arr1[i][j] == "0" && arr2[i][j] == "0" {
                temp += " "
            } else {
                temp += "#"
            }
        }
        answer.append(temp)
    }
    return answer
}
```

- 인자로 들어오는 배열의 요소를 이진법으로 변환
    - (변환시 앞부분의 0이 생략되는 현상이 있어서 부족한 만큼 넣어줘야 한다고 판단)
- 만약, 이진수의 count가 n보다 작은경우 부족한 만큼 "0"을 넣어주고 바뀐 값으로 교체하는 `convertBinary`함수 생성
- 마지막으로 각 배열에 함수를 접목시켜 이진수를 얻어낸 다음 반복문으로 접근하여 공백과 "#"을 삽입

# 다른 풀이
```swift
func solution(_ n: Int, _ arr1: [Int], _ arr2: [Int]) -> [String] {
    var answer: [String] = []
    
    for i in 0..<n {
        var bitwise = String(arr1[i] | arr2[i], radix: 2)
        bitwise = String(repeating: "0", count: n - bitwise.count) + bitwise
        answer += [bitwise.reduce("", { $0 + ($1 == "1" ? "#" : " ") })]
    }
    return answer
}
```

- 다른 분의 코드를 보고 비트연산자에 대해 알게 되었고 역시 아는게 힘이구나를 느꼈습니다...👍
- 비트 연산자 OR (Bitwise OR Operator)이기 때문에 각 배열의[i] 값에 접근하여 한곳이라도 1이 있으면 결과값으로 1을 얻게 되고 바로 이진수로 변환이 됩니다.
- 만약, 이진수의 `count`값이 `n`보다 작은경우라면 부족한 갯수만큼 "0"을 삽입해 줍니다.
- 마지막으로 `reduce`를 활용해서 (`$0는 "", $1은 bitwise의 요소하나하나`) 1이면 #, 0이면 공백을 넣어주고 `answer`에 합친후 결과를 반홥합니다.


# 비트연산자가 무엇인지..?

### Bit? (드랍더비트아님..ㅎ)

Binary Digit의 줄임말로 0과 1로만 표현할 수 있는 이진 숫자를 의미한다.

### Bitwise Operator(비트연산자)

비트 연산자를 사용하여 개발 원시 데이터 비트를 조작할 수 있다.

그래픽 프로그래밍이나 장치 드라이버 생성과 같은 저수준(Low-level) 프로그래밍에 자주 사용되고, 데이터 통신을 위한 decoding, encoding 데이터와 같은 외부 소스의 원시 데이터로 작업할 때도 사용될 수 있다.

[고급 연산자 (Advanced Operators)](https://jusung.gitbook.io/the-swift-language-guide/language-guide/26-advanced-operators)

