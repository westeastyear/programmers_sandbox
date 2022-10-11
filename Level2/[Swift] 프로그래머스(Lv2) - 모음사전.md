# [Swift] 프로그래머스(Lv2) - 모음사전

[문제출처 프로그래머스 - 모음사전](https://school.programmers.co.kr/learn/courses/30/lessons/84512)

# 나의 풀이

```swift
func solution(_ word: String) -> Int {
    var arr = [String]()
    let aeiou = ["A", "E", "I", "O", "U"]
    
    for i in aeiou {
        for j in aeiou {
            for k in aeiou {
                for h in aeiou {
                    for u in aeiou {
                        arr.append(i+j+k+h+u)
                    }
                    arr.append(i+j+k+h)
                }
                arr.append(i+j+k)
            }
            arr.append(i+j)
        }
        arr.append(i)
    }
    return arr.sorted().firstIndex(of: word)!+1
}
```

- 때론 무식한 방법이 정답일 때가 있다..!
