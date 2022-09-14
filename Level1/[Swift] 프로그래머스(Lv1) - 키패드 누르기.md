# [Swift] 프로그래머스(Lv1) - 키패드 누르기

[문제출처 프로그래머스 - 키패드 누르기](https://school.programmers.co.kr/learn/courses/30/lessons/67256)

# 나의 풀이

```swift
func solution(_ numbers: [Int], _ hand: String) -> String {
    var result = ""
    let keyPad = [1:(0,0), 2:(0,1), 3:(0,2),
                  4:(1,0), 5:(1,1), 6:(1,2),
                  7:(2,0), 8:(2,1), 9:(2,2),
                  10:(3,0), 0:(3,1), 11:(3,2)]
    var leftThumb = keyPad[10]
    var rightThumb = keyPad[11]
    
    for n in numbers {
        switch n {
        case 1, 4, 7:
            result += "L"
            leftThumb = keyPad[n]
        case 3, 6, 9:
            result += "R"
            rightThumb = keyPad[n]
        default:
            let leftDistance = abs(keyPad[n]!.0 - leftThumb!.0) + abs(keyPad[n]!.1 - leftThumb!.1)
            let rightDistance = abs(keyPad[n]!.0 - rightThumb!.0) + abs(keyPad[n]!.1 - rightThumb!.1)
                        
            if leftDistance < rightDistance {
                result += "L"
                leftThumb = keyPad[n]
            } else if leftDistance > rightDistance {
                result += "R"
                rightThumb = keyPad[n]
            } else {
                switch hand {
                case "right":
                    result += "R"
                    rightThumb = keyPad[n]
                case "left":
                    result += "L"
                    leftThumb = keyPad[n]
                default:
                    print("error")
                }
            }
        }
    }
    return result
}
```

- 키패드에 좌표를 설정하는 힌트를 알게되니 술술 풀수 있었던 문제였다.(그동안 고민한 나의 시간…😭)
- `1,4,7`은 왼손 엄지, `3,6,9`는 오른손 엄지를 사용한다. 나머지번호는 `n`번의 좌표와 가까운 거리의 엄지를 사용한다.
- 반복문을 돌면서 왼쪽, 오른쪽 엄지의 좌표를 누른 번호의 좌표로 업데이트 해준다.
- `leftThumb`좌표와 `rightThumb`좌표를 빼주게 되면 값이 마이너스가 되는 경우가 있는데, `abs()`메서드를 활용하였다.
- 절대값으로 변환하여 튜플타입 거리값`(x, y)`을 추출해낸다.
- 엄지는 상하좌우 4가지 방향으로만 이동할 수 있고, 키패드 이동 한 칸은 거리로 `1`에 해당하기에 `x`와 `y`를 더해주었다.
- 더 작은 값이 가까운 거리를 의미하므로 알맞은 `L, R`를 입력해주고, 값이 동일하다면 `hand`에 맞는 쪽으로 `L, R`를 입력해준다.
