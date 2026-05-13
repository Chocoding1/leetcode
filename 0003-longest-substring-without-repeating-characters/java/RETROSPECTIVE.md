# 풀이 회고

## 핵심 아이디어

원본 코드가 실제로 이 문제의 풀이를 담고 있지 않아, **슬라이딩 윈도우(Sliding Window)** 방식으로 올바른 풀이를 작성한다. 왼쪽 포인터(`leftIndex`)와 오른쪽 포인터(`rightIndex`)로 윈도우를 유지하면서, 각 문자의 마지막 등장 위치를 HashMap에 기록한다. 오른쪽 포인터가 이동할 때 현재 문자가 윈도우 안에 이미 존재하면 왼쪽 포인터를 그 문자의 다음 위치로 점프시켜 중복을 제거한다. 이 방식 덕분에 각 문자를 한 번씩만 방문하여 O(n) 시간에 해결할 수 있다.

## 사용된 자료구조 / 알고리즘

- 슬라이딩 윈도우 (Sliding Window)
- 해시 맵 (HashMap) — 문자 → 마지막 등장 인덱스 매핑

## 복잡도 분석

- **시간 복잡도**: O(n)
  - 오른쪽 포인터가 문자열을 한 번 순회하며, HashMap 조회/갱신이 O(1)이므로 전체 O(n)
- **공간 복잡도**: O(min(n, a))
  - HashMap에 저장되는 항목 수는 현재 윈도우 내 고유 문자 수이며, 문자 집합 크기 `a`(알파벳, 숫자, 기호 등)에 의해 상한이 결정됨

## 개선된 코드

```java
import java.util.HashMap;
import java.util.Map;

class Solution {

    public int lengthOfLongestSubstring(String s) {
        // 문자 → 해당 문자의 마지막 등장 인덱스를 저장하는 맵
        Map<Character, Integer> lastIndexMap = new HashMap<>();

        int maxLength = 0;       // 중복 없는 가장 긴 부분 문자열의 길이
        int leftIndex = 0;       // 슬라이딩 윈도우의 왼쪽 경계 (포함)

        for (int rightIndex = 0; rightIndex < s.length(); rightIndex++) {
            char currentChar = s.charAt(rightIndex);

            // 현재 문자가 윈도우 안에 이미 존재하는지 확인
            if (lastIndexMap.containsKey(currentChar)) {
                int lastSeenIndex = lastIndexMap.get(currentChar);

                // 중복 문자가 현재 윈도우 범위 안에 있을 때만 왼쪽 경계를 이동
                // (이전 윈도우 밖의 오래된 인덱스는 무시)
                if (lastSeenIndex >= leftIndex) {
                    leftIndex = lastSeenIndex + 1;
                }
            }

            // 현재 문자의 마지막 등장 위치를 갱신
            lastIndexMap.put(currentChar, rightIndex);

            // 현재 윈도우 크기와 최댓값을 비교하여 갱신
            int currentWindowSize = rightIndex - leftIndex + 1;
            maxLength = Math.max(maxLength, currentWindowSize);
        }

        return maxLength;
    }
}
```

## 다른 접근

**1. 배열 기반 슬라이딩 윈도우 (Array instead of HashMap)**
문자 집합이 ASCII(128자) 또는 확장 ASCII(256자)로 한정된다면 HashMap 대신 크기 128(또는 256)짜리 정수 배열로 인덱스를 관리할 수 있다. HashMap의 해싱 오버헤드가 없어 실제 실행 속도가 빠르지만, 유니코드 문자가 포함된 경우에는 적용할 수 없다는 제약이 있다.

**2. HashSet 기반 슬라이딩 윈도우 (HashSet + 두 포인터)**
HashSet으로 현재 윈도우 안의 문자 존재 여부만 관리하고, 중복이 발생하면 왼쪽 포인터를 한 칸씩 이동하며 Set에서 제거하는 방식이다. 코드가 직관적이어서 이해하기 쉽지만, 왼쪽 포인터가 한 번에 여러 칸 점프하는 HashMap 방식과 달리 한 칸씩 이동하므로 최악의 경우 포인터 이동 횟수가 늘어나 상수 배만큼 느릴 수 있다 (시간 복잡도는 동일하게 O(n)).

## 비슷한 문제

- LeetCode 159 · Longest Substring with At Most Two Distinct Characters
- LeetCode 340 · Longest Substring with At Most K Distinct Characters
- LeetCode 76 · Minimum Window Substring