# [Swift] 프로그래머스(Lv2) - [1차] 캐시

[문제출처 프로그래머스 - [1차] 캐시](https://school.programmers.co.kr/learn/courses/30/lessons/17680)

# 나의 풀이(Array)

```swift
func solution(_ cacheSize: Int, _ cities: [String]) -> Int {
    guard cacheSize != 0 else { return cities.count * 5 }
    
    var cache = [String]()
    var time = 0
    
    for i in 0..<cities.count {
        let city = cities[i].lowercased()
        
        if cache.contains(city) {
            time += 1
            cache.remove(at: cache.firstIndex(of: city)!)
        } else {
            time += 5
            if cache.count >= cacheSize {
                cache.removeFirst()
            }
        }
        cache.append(city)
    }
    return time
}
```

- 조건중에 캐시 교체 알고리즘은 `LRU(Least Recently Used)`를 사용한다고 해서 이건 또 뭐징…🤔 하며 찾아보았다.
- `cache`에 `city`가 포함되어 있다면 `hit`이므로 `+1`해주고 해당 `city`를 마지막으로 옮겨준다.
- 포함되어 있지 않다면 `miss`이므로 `+5`해준다.
- `LRU`특성상 `cacheSize`보다 갯수가 커지게 되면 오래된 데이터를 지워주어야 하므로 `cache`의 제일 오래된 요소인 첫번째 요소를 삭제해준다.
- 새로운 `city`를 추가해주며 실행시간을 반환하도록 한다.

# 다른 풀이(Dictionary)

```swift
func solution(_ cacheSize: Int, _ cities: [String]) -> Int {
    guard cacheSize != 0 else { return cities.count * 5 }
    
    var cache = [String: Int]()
    var time = 0
    
    for (index, city) in cities.enumerated() {
        let city = city.lowercased()
        
        if cache[city] != nil {
            time += 1
        } else {
            time += 5
            if cache.count >= cacheSize {
                let target = cache.sorted { $0.value < $1.value }.first!.key
                cache[target] = nil
            }
        }
        cache[city] = index
    }
    return time
}
```

# LRU(Least Recently Used)

[코드없는 프로그래밍 YouTube](https://www.youtube.com/watch?v=HpuIrGiHwTo) 

- 가장 최근에 사용되지 않은걸 제거하는 알고리즘
- `LRU`의 기본 가설은 가장 오랫동안 사용하지 않았던 데이터는 앞으로도 사용할 확률이 적다는 것
