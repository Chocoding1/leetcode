# 풀이 회고

## 핵심 아이디어

배열을 순회하면서 각 원소에 대해 "지금 이 값과 더해서 target이 되는 값(보수, complement)을 이전에 본 적 있는가?"를 해시 맵에서 O(1)로 조회한다. 보수가 존재하면 그 보수의 인덱스와 현재 인덱스가 정답이 되고, 존재하지 않으면 현재 값과 인덱스를 해시 맵에 저장하고 다음으로 넘어간다. 이 방식 덕분에 배열을 단 한 번만 순회해도 정답을 찾을 수 있다.

## 사용된 자료구조 / 알고리즘

- 해시 맵 (Hash Map): 값 → 인덱스 매핑을 저장하여 보수 조회를 O(1)로 수행
- 단일 순회 (Single Pass): 배열을 딱 한 번만 훑어 O(n) 시간 달성

## 복잡도 분석

- **시간 복잡도**: O(n)
  - 배열을 한 번만 순회하며, 각 원소에서 해시 맵 조회/삽입이 평균 O(1)이므로 전체 O(n)
- **공간 복잡도**: O(n)
  - 최악의 경우 배열의 모든 원소를 해시 맵에 저장해야 하므로 O(n)

## 개선된 코드

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int[] twoSum(int[] nums, int target) {
        // 지금까지 순회한 원소를 "값 → 인덱스" 형태로 저장하는 해시 맵
        Map<Integer, Integer> valueToIndex = new HashMap<>();

        for (int currentIndex = 0; currentIndex < nums.length; currentIndex++) {
            int currentValue = nums[currentIndex];

            // target에서 현재 값을 빼면, 함께 더해져야 할 보수(complement)를 구할 수 있다
            int complement = target - currentValue;

            // 보수가 해시 맵에 존재하면 → 두 인덱스가 정답
            if (valueToIndex.containsKey(complement)) {
                int complementIndex = valueToIndex.get(complement);
                return new int[]{complementIndex, currentIndex};
            }

            // 보수가 없으면 현재 값을 해시 맵에 저장하고 다음 원소로 이동
            valueToIndex.put(currentValue, currentIndex);
        }

        // 문제 조건상 항상 정답이 존재하므로 이 지점에 도달하지 않음
        throw new IllegalArgumentException("유효한 두 수의 합이 존재하지 않습니다.");
    }
}
```

## 다른 접근

**브루트 포스 (Brute Force) — O(n²) / O(1)**
두 개의 중첩 반복문으로 모든 쌍을 확인하는 방법이다. 공간을 추가로 사용하지 않아 공간 복잡도가 O(1)이라는 장점이 있지만, 시간 복잡도가 O(n²)으로 입력이 커질수록 급격히 느려진다는 치명적인 트레이드오프가 있다.

**정렬 + 투 포인터 (Two Pointers) — O(n log n) / O(n)**
배열을 정렬한 뒤 양쪽 끝에서 포인터를 좁혀 가며 합이 target인 쌍을 찾는 방법이다. 시간 복잡도는 정렬 비용인 O(n log n)이며 해시 맵보다 직관적이다. 단, 정렬하면 원래 인덱스 정보가 사라지므로 `(값, 원래 인덱스)` 쌍을 별도로 보관해야 하는 구현 복잡도가 생기고, 결국 공간도 O(n)이 필요해 해시 맵 방식 대비 뚜렷한 이점이 없다.

## 비슷한 문제

- LeetCode 167 · Two Sum II - Input Array Is Sorted
- LeetCode 15 · 3Sum
- LeetCode 560 · Subarray Sum Equals K