# 풀이 회고

## 핵심 아이디어

배열을 한 번 순회하면서, 현재 원소와 더해서 `target`이 되는 "보완 값(complement)"이 이미 등장했는지를 해시 맵(Hash Map)으로 O(1)에 확인한다. 보완 값이 없으면 현재 원소와 인덱스를 맵에 기록해두고, 나중에 등장할 원소가 이를 찾아 쓸 수 있게 한다. 이처럼 "지금까지 본 값을 기억해두고 역방향으로 짝을 찾는" 구조 덕분에 배열을 단 한 번만 훑어도 정답을 구할 수 있다.

## 사용된 자료구조 / 알고리즘

- 해시 맵 (Hash Map): 값 → 인덱스 매핑으로 O(1) 조회
- 단일 패스 순회 (One-pass traversal): 배열을 한 번만 탐색

## 복잡도 분석

- **시간 복잡도**: O(n)
  - 배열을 한 번만 순회하며, 각 원소에서 해시 맵 조회·삽입이 평균 O(1)이므로 전체 O(n)
- **공간 복잡도**: O(n)
  - 최악의 경우 배열의 모든 원소를 해시 맵에 저장해야 하므로 O(n)

## 개선된 코드

```java
import java.util.HashMap;
import java.util.Map;

class Solution {

    public int[] twoSum(int[] nums, int target) {
        // (값 → 인덱스) 형태로 지금까지 등장한 원소를 기록하는 맵
        Map<Integer, Integer> indexByValue = new HashMap<>();

        for (int currentIndex = 0; currentIndex < nums.length; currentIndex++) {
            int currentValue = nums[currentIndex];

            // target에서 현재 값을 빼면, 짝이 되어야 할 보완 값을 구할 수 있다
            int complementValue = target - currentValue;

            // 보완 값이 이미 맵에 존재하면 → 두 인덱스가 정답
            if (indexByValue.containsKey(complementValue)) {
                int complementIndex = indexByValue.get(complementValue);
                return new int[]{complementIndex, currentIndex};
            }

            // 아직 짝을 찾지 못했으므로, 현재 값과 인덱스를 맵에 저장
            indexByValue.put(currentValue, currentIndex);
        }

        // 문제 조건상 반드시 정답이 존재하므로 이 지점에는 도달하지 않음
        throw new IllegalArgumentException("유효한 두 수의 합이 존재하지 않습니다.");
    }
}
```

## 다른 접근

**① 브루트 포스 (Brute Force) — O(n²) / O(1)**
이중 반복문으로 모든 쌍 `(i, j)`를 검사해 합이 `target`인 쌍을 찾는다. 구현이 가장 단순하지만, 배열 길이가 커질수록 시간이 급격히 늘어난다는 트레이드오프가 있다. 공간은 추가 자료구조 없이 O(1)로 절약된다.

**② 정렬 + 투 포인터 (Two Pointers) — O(n log n) / O(n)**
배열을 값 기준으로 정렬한 뒤(원래 인덱스는 별도 보관), 양 끝에서 포인터를 좁혀가며 합을 맞춘다. 해시 맵보다 공간 효율이 나쁘진 않지만, 정렬 비용(O(n log n))이 발생하고 원본 인덱스를 따로 저장해야 하는 번거로움이 있다.

## 비슷한 문제

- **LeetCode 167** · Two Sum II - Input Array Is Sorted
- **LeetCode 15** · 3Sum
- **LeetCode 560** · Subarray Sum Equals K