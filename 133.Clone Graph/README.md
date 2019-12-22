Given a reference of a node in a connected undirected graph, return a deep copy (clone) of the graph. Each node in the graph contains a val (int) and a list (List[Node]) of its neighbors.

**Example:**

![pic](https://assets.leetcode.com/uploads/2019/11/04/133_clone_graph_question.png)

```
Input:
{"$id":"1","neighbors":[{"$id":"2","neighbors":[{"$ref":"1"},{"$id":"3","neighbors":[{"$ref":"2"},{"$id":"4","neighbors":[{"$ref":"3"},{"$ref":"1"}],"val":4}],"val":3}],"val":2},{"$ref":"4"}],"val":1}

Explanation:
Node 1's value is 1, and it has two neighbors: Node 2 and 4.
Node 2's value is 2, and it has two neighbors: Node 1 and 3.
Node 3's value is 3, and it has two neighbors: Node 2 and 4.
Node 4's value is 4, and it has two neighbors: Node 1 and 3.
```

**Note:**

- The number of nodes will be between 1 and 100.
- The undirected graph is a simple graph, which means no repeated edges and no self-loops in the graph.
- Since the graph is undirected, if node p has node q as neighbor, then node q must have node p as neighbor too.
- You must return the copy of the given node as a reference to the cloned graph.

**Solution:**

```js
/**
 * // Definition for a Node.
 * function Node(val,neighbors) {
 *    this.val = val;
 *    this.neighbors = neighbors;
 * };
 */
/**
 * @param {Node} node
 * @return {Node}
 */
var cloneGraph = function(node) {
  if (!node) {
    return node
  }

  let clone = new Node(node.val, [])
  let nodeMap = new Map()
  nodeMap.set(node, clone)
  let queue = [node]

  while (queue.length) {
    let _node = queue.shift()
    let _clone = nodeMap.get(_node)
    for (let n of _node.neighbors) {
      if (!nodeMap.get(n)) {
        nodeMap.set(n, new Node(n.val, []))
        queue.push(n)
      }
      _clone.neighbors.push(nodeMap.get(n))
    }
  }

  return clone
};
```
