Equations are given in the format A / B = k, where A and B are variables represented as strings, and k is a real number (floating point number). Given some queries, return the answers. If the answer does not exist, return -1.0.

**Example:**
```
Given a / b = 2.0, b / c = 3.0.
queries are: a / c = ?, b / a = ?, a / e = ?, a / a = ?, x / x = ? .
return [6.0, 0.5, -1.0, 1.0, -1.0 ].

The input is: vector<pair<string, string>> equations, vector<double>& values, vector<pair<string, string>> queries , where equations.size() == values.size(), and the values are positive. This represents the equations. Return vector<double>.
```
According to the example above:
```
equations = [ ["a", "b"], ["b", "c"] ],
values = [2.0, 3.0],
queries = [ ["a", "c"], ["b", "a"], ["a", "e"], ["a", "a"], ["x", "x"] ]. 
```

**Solution:**

```golang
func calcEquation(equations [][]string, values []float64, queries [][]string) []float64 {
  uf := CreateUF()
  for i, e := range equations {
    uf.Add(e[0])
    uf.Add(e[1])
    uf.Union(e[0], e[1], values[i])
  }
  ans := make([]float64, len(queries))
  for i, query := range queries {
    ans[i] = uf.Ratio(query[0], query[1])
  }
  return ans
}

type UF struct {
  root map[string]string
  ratio map[string]float64
}

func CreateUF() *UF {
  return &UF{
    root: make(map[string]string),
    ratio: make(map[string]float64),
  }
}

func (this *UF) Add(p string) {
  if this.root[p] != "" {
    return
  }
  this.ratio[p] = 1
  this.root[p] = p
}

func (this *UF) Union(p, q string, r float64) {
  pr, qr := this.findRoot(p), this.findRoot(q)
  if pr == qr || pr == "" || qr == "" {
    return
  }
  this.ratio[pr] = this.ratio[q] / this.ratio[p] * r
  this.root[pr] = qr
}

func (this *UF) Ratio(p, q string) float64 {
  pr, qr := this.findRoot(p), this.findRoot(q)
  if pr != qr {
    return -1.0
  }
  return this.ratio[p] / this.ratio[q]
}

func (this *UF) findRoot(p string) string {
  if pr, ok := this.root[p]; ok {
    if p == pr {
      return p
    }
    prr := this.findRoot(pr)
    this.ratio[p] *= this.ratio[pr]
    this.root[p] = prr
    return prr
  }
  return ""
}

```
