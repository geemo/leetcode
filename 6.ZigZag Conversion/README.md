The string `"PAYPALISHIRING"` is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

```
P   A   H   N
A P L S I I G
Y   I   R
```

And then read line by line: `"PAHNAPLSIIGYIR"`

Write the code that will take a string and make this conversion given a number of rows:

string convert(string s, int numRows);

**Example 1:**

```
Input: s = "PAYPALISHIRING", numRows = 3
Output: "PAHNAPLSIIGYIR"
```

**Example 2:**

```
Input: s = "PAYPALISHIRING", numRows = 4
Output: "PINALSIGYAHRPI"

Explanation:

P     I    N
A   L S  I G
Y A   H R
P     I
```

**Solution:**

```golang
func convert(s string, numRows int) string {
  if s == "" || numRows < 2 {
    return s
  }

  strArr := make([]string, numRows)
  base := 1
  idx := 0
  for i := range s {
    if idx == 0 {
      base = 1
    } else if idx == numRows - 1 {
      base = -1
    }
    strArr[idx] += s[i:i + 1]
    idx += base 
  }

  str := ""
  for _, s := range strArr {
    str += s
  }
  return str
}
```
