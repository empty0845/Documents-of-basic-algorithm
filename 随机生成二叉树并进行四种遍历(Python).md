## Python

```python
import random

class Queue:
    class Node:
        def __init__(self, val=None, next=None):
            self.val = val
            self.next = next

    def __init__(self):
        self.lenth = 0
        self.dummy = self.Node()
        self.tail = self.dummy

    def pop(self):
        if not self.lenth:
            return
        newNode = self.dummy.next
        if newNode == self.tail:
            self.tail = self.dummy
        self.dummy.next = self.dummy.next.next
        newNode.next = None
        self.lenth -= 1

    def push(self, val):
        newNode = self.Node(val)
        self.tail.next = newNode
        self.tail = self.tail.next
        self.lenth += 1
        return True

    def front(self):
        if not self.lenth:
            return None
        return self.dummy.next.val

    def getSize(self):
        return self.lenth

    def isEmpty(self):
        return not self.lenth


class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


def show(node):
    p = node
    while p:
        print(p.val, end=' ')
        p = p.next
    print("\t")

def sequenceTraversal(root):
    ans = list()
    if not root:
        return ans
    q = Queue()
    q.push(root)
    while not q.isEmpty():
        n = q.getSize()
        level = list()
        for i in range(n):
            node = q.front()
            q.pop()
            level.append(node.val)
            if node.left:
                q.push(node.left)
            if node.right:
                q.push(node.right)
        ans.append(level)
    return ans

def midorderTraversal(root):
    ans = list()
    if not root:
        return ans
    left = midorderTraversal(root.left)
    right = midorderTraversal(root.right)
    ans += left
    ans.append(root.val)
    ans += right
    return ans

def buildBinarySearchTree(root, val):
    if not root:
        return
    newNode = TreeNode(val)
    if root.val > val:
        if not root.left:
            root.left = newNode
            return
        else:
            buildBinarySearchTree(root.left, val)
    else:
        if not root.right:
            root.right = newNode
            return
        else:
            buildBinarySearchTree(root.right, val)
    return

def randomlyInsertNode(root, node):
    if not root or not node:
        return
    randomFactor = random.randint(0, 2)
    if randomFactor:
        if root.val > node.val:
            if not root.left:
                root.left = node
                return
            else:
                randomlyInsertNode(root.left, node)
        else:
            if not root.right:
                root.right = node
                return
            else:
                randomlyInsertNode(root.right, node)
    else:
        if root.val < node.val:
            if not root.left:
                root.left = node
                return
            else:
                randomlyInsertNode(root.left, node)
        else:
            if not root.right:
                root.right = node
                return
            else:
                randomlyInsertNode(root.right, node)

def randomlyCreateBinaryTree(lowerBound=1, upperBound=50, numbeerOfRoots=25):
    root = TreeNode(random.randint(lowerBound, upperBound + 1))
    for i in range(numbeerOfRoots):
        newNode = TreeNode(int(random.uniform(lowerBound, upperBound + 1)))
        randomlyInsertNode(root, newNode)
    return root

root = TreeNode(25)
for i in range(1, 50):
    random_number = random.uniform(1, 50)
    buildBinarySearchTree(root, int(random_number))
nums = sequenceTraversal(root)
for i in nums:
    print(i)
print()
print(midorderTraversal(root))
print("-----------------------------------")
randomTree = randomlyCreateBinaryTree(1, 100, 100)
showRandomTree = sequenceTraversal(randomTree)
for i in showRandomTree:
    print(i)
```

## C++

```c++
#include <iostream>
#include <random>
#include <vector>
#include <queue>
using namespace std;
 
class TreeNode {
public:
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int val = 0, TreeNode* left = nullptr, TreeNode* right = nullptr): val(val), left(left), right(right) {}
};

void randomlyInsertNode(TreeNode* root, TreeNode* node) {
    if (!root || !node) {
        return;
    }
    random_device rd;
    mt19937 gen(rd());
    uniform_int_distribution<int> dis(0, 1);
    int randomFactor = dis(gen);
    if (randomFactor) {
        if (root->val > node->val) {
            if (!root->left) {
                root->left = node;
                return;
            } else {
                randomlyInsertNode(root->left, node);
            }
        } else {
            if (!root->right) {
                root->right = node;
                return;
            } else {
                randomlyInsertNode(root->right, node);
            }
        }
    } else {
        if (root->val < node->val) {
            if (!root->left) {
                root->left = node;
                return;
            } else {
                randomlyInsertNode(root->left, node);
            }
        } else {
            if (!root->right) {
                root->right = node;
                return;
            } else {
                randomlyInsertNode(root->right, node);
            }
        }
    }
}

TreeNode* randomlyCreateBinaryTree(int lowerBound = 0, int upperBound = 50, int numberOfNodes = 25) {
    random_device rd;
    mt19937 gen(rd());
    uniform_int_distribution<int> dis(lowerBound, upperBound);
    TreeNode* root = new TreeNode(dis(gen));
    for (int i = 0; i < numberOfNodes; i++) {
        TreeNode* newNode = new TreeNode(dis(gen));
        randomlyInsertNode(root, newNode);
    }
    return root;
}

vector<int> midOrderTravesal(TreeNode* root) {
    vector<int> ans;
    if (!root) {
        return ans;
    }
    vector<int> left = midOrderTravesal(root->left);
    vector<int> right = midOrderTravesal(root->right);
    ans.insert(ans.end(), left.begin(), left.end());
    ans.push_back(root->val);
    ans.insert(ans.end(), right.begin(), right.end());
    return ans;
}

vector<int> postOrderTravesal(TreeNode* root) {
    vector<int> ans;
    if (!root) {
        return ans;
    }
    vector<int> left = postOrderTravesal(root->left);
    vector<int> right = postOrderTravesal(root->right);
    ans.insert(ans.end(), left.begin(), left.end());
    ans.insert(ans.end(), right.begin(), right.end());
    ans.push_back(root->val);
    return ans;
}

vector<int> preOrderTravesal(TreeNode* root) {
    vector<int> ans;
    if (!root) {
        return ans;
    }
    vector<int> left = preOrderTravesal(root->left);
    vector<int> right = preOrderTravesal(root->right);
    ans.push_back(root->val);
    ans.insert(ans.end(), left.begin(), left.end());
    ans.insert(ans.end(), right.begin(), right.end());
    return ans;
}

vector<vector<int>> sequenceTravesal(TreeNode* root) {
    vector<vector<int>> ans;
    if (!root) {
        return ans;
    }
    queue<TreeNode*> q;
    q.push(root);
    while (!q.empty()) {
        int n = q.size();
        vector<int> level;
        for (int i = 0; i < n; i++) {
            TreeNode* node = q.front();
            q.pop();
            if (node->left) {
                q.push(node->left);
            }
            if (node->right) {
                q.push(node->right);
            }
            level.push_back(node->val);
        }
        ans.push_back(level);
    }
    return ans;
    
}

void showVector(vector<int> nums) {
     for (int i: nums) {
         cout << i << ' ';
     }
     cout << endl;
 }

void showBinaryTree(TreeNode* root) {
    if (!root) {
        return;
    }
    vector<vector<int>> nodes = sequenceTravesal(root);
    for (vector<int> nums: nodes) {
        showVector(nums);
    }
}
 
int main() {
    cout << "Build a Binary Tree and Show it:" << endl;
    TreeNode* root = randomlyCreateBinaryTree();
    showBinaryTree(root);
    
    cout << "Mid Order Travesal:" << endl;
    showVector(midOrderTravesal(root));
    cout << "Post Order Travesal:" << endl;
    showVector(postOrderTravesal(root));
    cout << "Preoder Travesal:" << endl;
    showVector(preOrderTravesal(root));
    
    return 0;
}
```
