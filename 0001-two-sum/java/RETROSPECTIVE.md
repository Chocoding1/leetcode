# 풀이 회고

## 핵심 아이디어

배열을 한 번 순회하면서, 현재 원소와 더했을 때 `target`이 되는 "보완값(complement)"이 이미 앞에서 등장한 적 있는지를 해시 맵으로 O(1)에 확인합니다. 핵심은 "지금까지 본 숫자와 그 인덱스"를 해시 맵에 축적하면서, 아직 보지 못한 오른쪽 원소가 왼쪽을 조회하는 방향으로 탐색한다는 점입니다. 덕분에 이중 반복문 없이 단일 패스로 정답을 찾을 수 있습니다.

## 사용된 자료구조 / 알고리즘

- 해시 맵 (Hash Map): 숫자 → 인덱스 매핑으로 O(1) 조회
- 단일 순회 (One-pass): 배열을 한 번만 탐색

## 복잡도 분석

- **시간 복잡도**: O(n)
  - 배열을 한 번만 순회하며, 해시 맵의 조회/삽입이 평균 O(1)이므로 전체 O(n)
- **공간 복잡도**: O(n)
  - 최악의 경우 배열의 모든 원소를 해시 맵에 저장해야 하므로 O(n)

## 개선된 코드

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int[] twoSum(int[] nums, int target) {
        // 숫자(값) → 인덱스를 저장하는 해시 맵 (이미 방문한 원소 기록용)
        Map<Integer, Integer> visitedNumToIndex = new HashMap<>();

        for (int currentIndex = 0; currentIndex < nums.length; currentIndex++) {
            int currentNum = nums[currentIndex];

            // target에서 현재 값을 뺀 보완값: 이 값이 앞에 있었다면 정답
            int complement = target - currentNum;

            // 보완값이 이미 해시 맵에 존재하면 두 인덱스를 반환
            if (visitedNumToIndex.containsKey(complement)) {
                int complementIndex = visitedNumToIndex.get(complement);
                return new int[] { complementIndex, currentIndex };
            }

            // 현재 숫자와 인덱스를 해시 맵에 기록 (이후 원소가 조회할 수 있도록)
            visitedNumToIndex.put(currentNum, currentIndex);
        }

        // 문제 조건상 항상 정답이 존재하므로 이 줄에 도달하면 안 됨
        throw new IllegalArgumentException("유효한 두 수의 합이 존재하지 않습니다.");
    }
}
```

## 다른 접근

**1. 브루트 포스 (Brute Force) — O(n²) / O(1)**
모든 두 원소 쌍 `(i, j)`를 이중 반복문으로 확인하는 방식입니다. 추가 공간이 전혀 필요 없다는 장점이 있지만, 배열 크기가 커질수록 시간이 급격히 증가해 `n = 10^4`에서는 약 5천만 번의 연산이 필요합니다.

**2. 정렬 + 투 포인터 (Two Pointers) — O(n log n) / O(n)**
배열을 `(값, 원래 인덱스)` 쌍으로 정렬한 뒤, 양 끝에서 포인터를 좁혀가며 합을 찾는 방식입니다. 원래 인덱스를 보존하기 위해 별도의 쌍 배열이 필요하고, 정렬 비용 때문에 해시 맵 방식보다 느립니다. 다만 해시 충돌이 전혀 없어 최악의 경우에도 안정적인 성능을 보장한다는 트레이드오프가 있습니다.

## 비슷한 문제

- LeetCode 167 · Two Sum II - Input Array Is Sorted
- LeetCode 15 · 3Sum
- LeetCode 560 · Subarray Sum Equals K