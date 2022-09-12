# [Swift] 프로그래머스(Lv1) - 소수 만들기

[문제출처 프로그래머스 - 소수 만들기](https://school.programmers.co.kr/learn/courses/30/lessons/12977)

# 다른 풀이

```swift
func isPrime(_ n: Int) -> Bool {
    for i in 2..<n {
        if n % i == 0 {
            return false
        }
    }
    return true
}

func solution(_ nums: [Int]) -> Int {
    var sum = [Int]()
    
    for i in 0..<nums.count {
        for j in 0..<i {
            for k in 0..<j {
                sum.append(nums[i]+nums[j]+nums[k])
            }
        }
    }
    return sum.filter { isPrime($0) }.count
}
```

- 소수임을 판별하는 `isPrime`함수를 생성하였다.
- 소수란 1과 자기자신(`n`)만을 약수로 가지는 수를 말한다.
- 1과 자기자신(`n`)을 제외한 범위에서 나머지가 0인 수가 있다면 `n`은 소수가 아니기 때문에 `false`를 반환한다.
- 서로다른 숫자 3개를 뽑아야 하기 때문에 반복문을 이전의 인덱스(`i`, `j`) 만큼으로 범위를 설정해주었다.
- 세번 반복하였고 저절로 중복이 제거 되었다?!?! 우연하게 발견한 코드였다. 신기했다.
- `sum`에 요소들을 합하여 넣어주었다.
- `filter`를 사용하여 소수를 판별하였고 `count`로 갯수를 반환한다.
