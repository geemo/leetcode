In a deck of cards, each card has an integer written on it.

Return true if and only if you can choose X >= 2 such that it is possible to split the entire deck into 1 or more groups of cards, where:

Each group has exactly X cards.
All the cards in each group have the same integer.

**Example 1:**
```
Input: deck = [1,2,3,4,4,3,2,1]
Output: true
Explanation: Possible partition [1,1],[2,2],[3,3],[4,4].
```
**Example 2:**
```
Input: deck = [1,1,1,2,2,2,3,3]
Output: false´
Explanation: No possible partition.
```
**Example 3:**
```
Input: deck = [1]
Output: false
Explanation: No possible partition.
```
**Example 4:**
```
Input: deck = [1,1]
Output: true
Explanation: Possible partition [1,1].
```
**Example 5:**
```
Input: deck = [1,1,2,2,2,2]
Output: true
Explanation: Possible partition [1,1],[2,2],[2,2].
```

**Constraints:**

- 1 <= deck.length <= 10^4
- 0 <= deck[i] < 10^4

**Solution:**

```golang
func hasGroupsSizeX(deck []int) bool {
  m := make(map[int]int)
  for _, v := range deck {
    m[v]++
  }

  var x int
  for _, v := range m {
    x = gcd(x, v)
    if x == 1 {
      return false
    }
  }
  return x >= 2
}

func gcd(a, b int) int {
  if b == 0 {
    return a
  }
  return gcd(b, a % b)
}
```
