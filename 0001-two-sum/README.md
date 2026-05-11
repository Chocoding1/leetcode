# 1. Two Sum

> **Easy** · Array, Hash Table · [원문](https://leetcode.com/problems/two-sum/)

## 문제

정수 배열 `nums`와 정수 `target`이 주어질 때, 두 수를 더하면 `target`이 되는 두 원소의 인덱스를 반환하세요.

각 입력에는 **정확히 하나의 정답**이 존재하며, 동일한 원소를 두 번 사용할 수 없습니다.

정답은 어떤 순서로 반환해도 괜찮습니다.

## 입출력 예시

### Example 1
```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
```
**설명**: `nums[0] + nums[1] == 9`이므로 `[0, 1]`을 반환합니다.

### Example 2
```
Input: nums = [3,2,4], target = 6
Output: [1,2]
```

### Example 3
```
Input: nums = [3,3], target = 6
Output: [0,1]
```

## 제약 조건

- `2 <= nums.length <= 10^4`
- `-10^9 <= nums[i] <= 10^9`
- `-10^9 <= target <= 10^9`
- **유효한 정답은 정확히 하나만 존재합니다.**

---

**심화 문제**: 시간 복잡도 `O(n^2)` 미만의 알고리즘을 설계할 수 있나요?