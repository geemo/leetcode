Given an input string, reverse the string word by word. 

**Example 1:**

```
Input: "the sky is blue"
Output: "blue is sky the"
```

**Example 2:**

```
Input: "  hello world!  "
Output: "world! hello"
Explanation: Your reversed string should not contain leading or trailing spaces.
```

**Example 3:**

```
Input: "a good   example"
Output: "example good a"
Explanation: You need to reduce multiple spaces between two words to a single space in the reversed string.
```

**Note:**

- A word is defined as a sequence of non-space characters.
- Input string may contain leading or trailing spaces. However, your reversed string should not contain leading or trailing spaces.
- You need to reduce multiple spaces between two words to a single space in the reversed string.
 

**Follow up:**

For C programmers, try to solve it in-place in O(1) extra space.

**Solution:**

```golang
func reverseWords(s string) string {
  if s == "" {
    return s
  }

  arr := cleanSpaces([]uint8(s))
  reverse(arr, 0, len(arr) - 1)
  _reverseWords(arr)

  return string(arr)
}

func cleanSpaces(arr []uint8) []uint8 {
  i, j, n := 0, 0, len(arr)
  for j < n {
    for j < n && arr[j] == ' ' {
      j++
    }
    for j < n && arr[j] != ' ' {
      arr[i] = arr[j]
      i++
      j++
    }
    for j < n && arr[j] == ' ' {
      j++
    }
    if j < n {
      arr[i] = ' '
      i++
    }
  }
  return arr[0:i]
}

func _reverseWords(arr []uint8) {
  i, j, n := 0, 0, len(arr)
  for j < n {
    for j < n && arr[j] != ' ' {
      j++
    }
    reverse(arr, i, j - 1)
    if j < n && arr[j] == ' ' {
      j++
      i = j
    }
  }
}

func reverse(arr []uint8, l, r int) {
  for l < r {
    arr[l], arr[r] = arr[r], arr[l]
    l++
    r--
  }
}
```

```golang
import (
  "bytes"
)

func reverseWords(s string) string {
  ns := bytes.TrimSpace([]byte(s))
  n := len(ns)
  i, j := n - 1, n - 1
  var ans [][]byte
  for i >= 0 {
    for i >= 0 && ns[i] != ' ' {
      i--
    }
    ans = append(ans, ns[i+1:j+1])
    for i >= 0 && ns[i] == ' ' {
      i--
    }
    j = i
  }

  return string(bytes.Join(ans, []byte(" ")))
}
```
