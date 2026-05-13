# 3. Longest Substring Without Repeating Characters

> **Medium** · Hash Table, String, Sliding Window · [원문](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

## 문제

문자열 `s`가 주어질 때, 중복 문자가 없는 가장 긴 **substring**의 길이를 구하라.

## 입출력 예시

### Example 1
```
Input: s = "abcabcbb"
Output: 3
```
**설명**: 정답은 `"abc"`이며, 길이는 3이다. `"bca"`와 `"cab"` 역시 정답으로 인정된다.

### Example 2
```
Input: s = "bbbbb"
Output: 1
```
**설명**: 정답은 `"b"`이며, 길이는 1이다.

### Example 3
```
Input: s = "pwwkew"
Output: 3
```
**설명**: 정답은 `"wke"`이며, 길이는 3이다. 단, 정답은 반드시 substring이어야 하며, `"pwke"`는 subsequence이므로 정답이 아님에 유의하라.

## 제약 조건

- `0 <= s.length <= 5 * 10^4`
- `s`는 영문자, 숫자, 기호, 공백으로 구성된다.