Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

**Example:**

```
You may serialize the following tree:

    1
   / \
  2   3
     / \
    4   5

as "[1,2,3,null,null,4,5]"
```

**Clarification:** The above format is the same as how LeetCode serializes a binary tree. You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.

**Note:** Do not use class member/global/static variables to store states. Your serialize and deserialize algorithms should be stateless.

**Solution:**

```js
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */

/**
 * Encodes a tree to a single string.
 *
 * @param {TreeNode} root
 * @return {string}
 */
var serialize = function(root) {
	if (!root) {
		return ''; 
	}

	let arr = [root.val];
	let levelNodes = [root];

	while(true) {
		let nextLevelNodes = [];
		let nextLevelVal = [];
		for (let node of levelNodes) {
			if (!node) {
				nextLevelNodes.push(null, null);
				nextLevelVal.push(null, null);
				continue;
			}

			if (node.left) {
				nextLevelNodes.push(node.left);
				nextLevelVal.push(node.left.val);
			} else {
				nextLevelNodes.push(null);
				nextLevelVal.push(null);
			}

			if (node.right) {
				nextLevelNodes.push(node.right);
				nextLevelVal.push(node.right.val);
			} else {
				nextLevelNodes.push(null);
				nextLevelVal.push(null);
			}
		}

		if (isEmpty(nextLevelNodes)) {
			break;
		}

		levelNodes = nextLevelNodes;
		arr = arr.concat(nextLevelVal);
	}

  return JSON.stringify(arr); 
};

function isEmpty(levelNodes) {
		return !levelNodes || !levelNodes.length || levelNodes.every(node => node == null);
	
}
/**
 * Decodes your encoded data to tree.
 *
 * @param {string} data
 * @return {TreeNode}
 */
var deserialize = function(data) {
	if (!data) {
		return data;
	}

	let arr = JSON.parse(data);
	let nodeMap = {'0': new TreeNode(arr[0])};

	for (let i = 0, len = arr.length; i < len; i++) {
		let left = i * 2 + 1;
		let right = i * 2 + 2;
		let rootNode = nodeMap[i];

		if (left < len && rootNode) {
			let leftNode = new TreeNode(arr[left]);
			rootNode.left = leftNode;
			nodeMap[left] = leftNode;
		}

		if (right < len && rootNode) {
			let rightNode = new TreeNode(arr[right]);
			rootNode.right = rightNode;
			nodeMap[right] = rightNode;
		}
	}

	return nodeMap[0];
};
/**
 * Your functions will be called as such:
 * deserialize(serialize(root));
 */
```
