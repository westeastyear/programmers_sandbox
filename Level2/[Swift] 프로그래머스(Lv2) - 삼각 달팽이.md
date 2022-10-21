# [Swift] 프로그래머스(Lv2) - 삼각 달팽이

[문제출처 프로그래머스 - 삼각 달팽이](https://school.programmers.co.kr/learn/courses/30/lessons/68645)

# 다른 풀이

```swift
enum Direction {
    case down
    case right
    case up
}

func solution(_ n: Int) -> [Int] {
    var direction: Direction = .down
    var result = [[Int]]()
    var sum = 0
    var value = 0
    var oneline = n-1 == 0 ? 1 : n-1
    var count = 1
    
    for i in 0..<n {
        result.append(Array(repeating: 0, count: i+1))
        sum += i+1
    }
    
    while count <= sum {
        switch direction {
        case .down:
            for i in 0..<oneline {
                result[i+(value*2)][value] = count
                count += 1
            }
            direction = .right
        case .right:
            for i in 0..<oneline {
                result[n-1-value][i+value] = count
                count += 1
            }
            direction = .up
        case .up:
            for i in value..<oneline+value {
                result[n-1-i][n-1-i-value] = count
                count += 1
            }
            value += 1
            oneline = oneline-3 == 0 ? 1 : oneline-3
            direction = .down
        }
    }
    return result.flatMap{ $0 }
}
```
- 달팽이 모양처럼 반시계방향으로 숫자를 차례대로 채우면 되는 문제입니다… 눈물이 많아진 요즘입니다…🥲
- `enum`으로 `Direction`을 정의하였습니다.
    - `down`, `right`, `up`
- 문제설명의 삼각형 모양과 동일하게 `0`으로 초기화 되어있는 이중배열을 만들어 둡니다. 반복하면서 `1`에서 `n`까지 `sum`에 더해줍니다. `sum`은 반복문의 종료조건으로 쓰이게 됩니다.
- 배열의 자리에 순서대로 채워질 `count`값이 `sum`보다 커질때까지 `while` 반복문이 돌게 됩니다.
- `direction` 변수를 만들어 방향을 지정해 주었고, `switch`문으로 분기하여 현재 방향에 맞는 반복문이 실행되도록 하였습니다. 각각의 반복문은 반복이 끝나면 다음의 진행방향을 지정해주어 달팽이 모양처럼 채워지도록 하였습니다.

<br>

- `down` 방향에서는 각 행의 같은 열에 값이 순서대로 저장되도록 하였습니다.

```swift
for i in 0..<oneline {
    result[i+(value*2)][value] = count
    count += 1
}
```

- 열에 해당되는 `[value]`는 고정되어야 하고, 행에 해당되는 `[i+(value*2)]`는 반복문 변화에 맞춰야 합니다.
- `n`이 `6`인경우 첫번째 바퀴에서 `oneline = 5`, `index`는 `[0,0] [1,0] [2,0] [3,0] [4,0]`이 되고, 두번째 바퀴에서는 `oneline = 2`, `index`는 `[2,1] [3,1]`이 된다.
- 만약 `n`이 `6`보다 커서 두바퀴 이상 돌 경우 세번째 바퀴의 `index`는 `[4,2]`가 되는데, 행은 2씩 더해지는 규칙을 찾을 수가 있습니다.
- `value`변수는 한바퀴 돌때마다 한 진행방향당 채워지는 칸 수가 줄어드는 만큼 변화되는 `index`를 조정하기 위한 값입니다.
- `oneline`은 각 방향마다 채울 개수를 저장하는 변수입니다. 한바퀴 돌때마다 `-3`씩 작아지는 규칙을 가지고 있습니다.

<br>

- `right` 방향에서는 같은 행이면서 다른 열에 값이 순서대로 저장되도록 하였습니다.

```swift
for i in 0..<oneline {
    result[n-1-value][i+value] = count
    count += 1
}
```

- 행에 해당되는 `[n-1-value]`는 고정되어야 하고, 열에 해당되는 `[i+value]`는 반복문 변화에 맞춰야 합니다.
- `n`이 `6`일 경우에 첫번째 바퀴에서 `oneline = 5`, `index`는 `[5,0] [5,1] [5,2] [5,3] [5,4]`가 되고 두번째 바퀴에서 `oneline = 2`, `index`는 `[4,1] [4,2]`이 됩니다.
- 만약 `n`이 `6`보다 커서 두바퀴 이상 돌게 될 경우 세번째 바퀴의 `index`는 `[3,2]`부터 시작하게 됩니다.
- 행은 마지막 행부터 한바퀴씩 돌때마다 `-1`씩 작아지고, 열은 `0`부터 시작해서 `1`씩 커지는데 한바퀴씩 돌때마다 `1`씩 커진 `index`로 시작하게 됩니다.

<br>

- `up` 방향에서는 다른 행이면서 다른 열에 역순으로 값이 저장되도록 하였습니다.

```swift
for i in value..<oneline+value {
    result[n-1-i][n-1-i-value] = count
    count += 1
}
value += 1
oneline = oneline-3 == 0 ? 1 : oneline-3
direction = .down
```

- `n`이 `6`일 경우에 첫번째 바퀴에서 `oneline = 5`, `index`는 `[5,5] [4,4] [3,3] [2,2] [1,1]`이 되고 두번째 바퀴에서는 `oneline = 2`, `index`는 `[4,3] [3,2]`가 됩니다.
- 행과 열이 모두 `1`씩 작아지고 `1`바퀴를 돌게 되면 이전보다 행은 `1`씩, 열은 `2`씩 작아진 상태에서 시작되는 것을 볼 수 있기 때문에 행과 열이 이러한 반복문의 변화에 맞춰야 합니다.
- 반복문의 범위를 `value..<oneline+value`로 설정하여 위의 로직을 만족하도록 하였습니다.
- `up`의 반복문이 끝나게 되면 진행방향은 `down`으로 바뀌게 되고, 한바퀴가 돌았으므로 `value`변수를 `+1`해주고, `oneline`도 이시점에 `-3`를 해줍니다.
