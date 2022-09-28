# [Swift] 프로그래머스(Lv2) - [1차] 뉴스 클러스터링

[문제출처 프로그래머스 - [1차] 뉴스 클러스터링](https://school.programmers.co.kr/learn/courses/30/lessons/17677)

# 나의 풀이

```swift
func customFiltering(_ str: String) -> [String] {
    let arr = str.map { String($0.uppercased()) }
    var result = [String]()
    
    for i in 0..<arr.count-1 {
        if Character(arr[i]).isLetter && Character(arr[i+1]).isLetter {
            result.append(arr[i]+arr[i+1])
        }
    }
    return result
}

func intersectionUnion(_ arr1: [String], _ arr2: [String]) -> [Int] {
    var dict1 = [String:Int]()
    var dict2 = [String:Int]()
    var minCount = 0
    var maxCount = 0
    
    arr1.forEach {
        dict1[$0, default: 0] += 1
    }
    arr2.forEach {
        dict2[$0, default: 0] += 1
    }
    
    for (key, value) in dict1 {
        if dict2[key] != nil {
            minCount += min(value, dict2[key]!)
            maxCount += max(value, dict2[key]!)
        } else {
            maxCount += value
        }
    }
    
    for (key, value) in dict2 {
        if dict1[key] == nil {
            maxCount += value
        }
    }
    
    return [minCount, maxCount]
}

func solution(_ str1: String, _ str2: String) -> Int {
    let str1 = customFiltering(str1)
    let str2 = customFiltering(str2)
    let intersectionCount = intersectionUnion(str1, str2)[0]
    let unionCount = intersectionUnion(str1, str2)[1]
    
    if intersectionCount == 0 && unionCount == 0 {
        return 65536
    }
    
    return Int(Double(intersectionCount) / Double(unionCount) * 65536)
}
```

- 입력으로 들어오는 문자열을 대문자화 및 배열로 변화후 두글자씩 끊어주는 함수(`customFiltering()`)를 만들어 주었다.
- 조건중에 영문자로만 된 쌍만 추가해야 하기 때문에, 공백, 숫자, 특수문자를 제외한 집합의 원소를 만들어 낸다.
    
    ```swift
    func customFiltering(_ str: String) -> [String] {
        let arr = str.map { String($0.uppercased()) }
        var result = [String]()
        
        for i in 0..<arr.count-1 {
            if Character(arr[i]).isLetter && Character(arr[i+1]).isLetter {
                result.append(arr[i]+arr[i+1])
            }
        }
        return result
    }
    ```
    

- 원래 `Set`을 활용해서 합집합(`union()`), 교집합(`intersection()`)을 구해줬었는데 다중집합에 대한 중복요소들을 하나로 보는 현상이 있었다.
- 그래서 기존 배열에 기반해서 구하는 방식을 고안해야 한다고 한다.
    
    ```swift
    // 예외 케이스
    A = {”AA”, “AA”} 
    B = {”AA”, “AA”, “AA”}
    
    // Set 사용시
    A = {”AA”} ?!
    B = {”AA”} ?!
    ```
    
    ```swift
    // 다중집합에 대한 설명예시
    다중집합 A = {1, 1, 2, 2, 3} 
    다중집합 B = {1, 2, 2, 4, 5}라고 한다면?!
    
    교집합 A ∩ B = {1, 2, 2}
    합집합 A ∪ B = {1, 1, 2, 2, 3, 4, 5}
    ```
    

- `Dictionary`로 교집합, 합집합 수를 카운트 하도록 하였다.
    
    ```swift
    // P.S 합집합의 갯수 구하는 공식
    
    (합집합 A ∪ B).count = A.count + B.count - (교집합 A ∩ B).count
    ```
    

- 먼저 `arr1`, `arr2`의 요소를 돌며 카운트 해준다. (중복되는 요소가 있을수도 있기 때문!!)
- `dict1`을 기준으로 시작하여 `dict1`에 있는 요소가 `dict2`에도 있다면 교집합이므로 `minCount(교집합)`, `maxCount(합집합)`의 카운트를 올려준다. 
- 한쌍(`2개`)의 교집합은 합집합에서 `1`개로 치기 때문에 합집합의 카운트도 올려준다.    
- 아래와 같은 케이스의 예시를 알아보자
    
    ```swift
    // 다음과 같은 다중집합이 있다
    
    A = {”AA”, “AA”}
    B = {”AA”, “AA”, “AA”}
    
    // 딕셔너리에 저장된 상태를 알아보자
    
    dict1 = ["AA":2]
    dict2 = ["AA":3]
    
    // 위에서 교집합의 갯수는 2개, 합집합의 갯수는 3개이다.
    // 따라서 중복되는 요소의 경우에 min()으로 최소값을 찾아 교집합에 카운트해주고
    // max()로 최대값을 찾아 합집합에 카운트 해주도록 하였다.
    
    for (key, value) in dict1 {
        if dict2[key] != nil {
            minCount += min(value, dict2[key]!)
            maxCount += max(value, dict2[key]!)
        } else {
            maxCount += value
        }
    }
    ```
    

- `dict1`의 요소가 `dict2`에 없으면 `maxCount(합집합)`의 카운트만 올려준다.
- 이전에 교집합은 다 찾아주었기 때문에 `dict2`를 기준으로 할때는 `dict2`에만 있는 요소만 찾아주어 `maxCount(합집합)`의 카운트를 올려준다.
- 사용할때는 `[0]`, `[1]` 인덱스로 값에 접근하여 사용하였다.
- 교집합, 합집합 둘 다 공집합인 경우 나눗셈이 정의 되지 않기때문에 조건에 따라 `65536`를 반환한다.
- `0`이 아닌 경우 자카드 유사도를 구하여 `65536`을 곱한 값을 반환한다!!
