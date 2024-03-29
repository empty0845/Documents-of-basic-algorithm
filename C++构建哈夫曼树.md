## Using heap

```c++
#include <iostream>
#include <queue>
#include <vector>

class TreeNode {
public:
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int val = 0, TreeNode* left = nullptr, TreeNode* right = nullptr): val(val), left(left), right(right) {}
};

struct compare {
    bool operator()(const TreeNode* a, const TreeNode* b) {
        return a->val > b->val;
    }
};

TreeNode* buildHfmTree(std::vector<int> nums) {
    std::priority_queue<TreeNode*, std::vector<TreeNode*>, compare> q;
    for (int i: nums) {
        TreeNode* node = new TreeNode(i);
        q.push(node);
    }
    while (q.size() > 1) {
        TreeNode* node1 = q.top();
        q.pop();
        TreeNode* node2 = q.top();
        q.pop();
        TreeNode* node3 = new TreeNode(node1->val + node2->val);
        node3->left = node1;
        node3->right = node2;
        q.push(node3);
    }
    return q.top();
}

void showTree(TreeNode* root) {
    if (!root) {
        return;
    }
    showTree(root->left);
    std::cout << root->val << " ";
    showTree(root->right);
}

int main() {
    std::vector<int> nums = {1, 2, 3, 4, 9, 4, 3, 6};
    TreeNode* hfmRoot = buildHfmTree(nums);
    showTree(hfmRoot);
    return 0;
}
```

## Using sorting

```c++
#include <iostream>
#include <algorithm>

class TreeNode {
public:
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int val = 0, TreeNode* left = nullptr, TreeNode* right = nullptr): val(val), left(left), right(right) {}
};

static bool compare(TreeNode* a, TreeNode* b) {
    return a->val < b->val;
}

TreeNode* buildHfmTree(int* nums, int length) {
    TreeNode** nodes = new TreeNode*[100];
    int size = 0;
    for (int i = 0; i < length; i++) {
        TreeNode* node = new TreeNode(nums[i]);
        nodes[i] = node;
        size++;
    }
    std::sort(nodes, nodes + size, compare);
    while (size > 1) {
        TreeNode* node1 = nodes[size - 1];
        size--;
        TreeNode* node2 = nodes[size - 1];
        size--;
        TreeNode* node3 = new TreeNode(node1->val + node2->val);
        node3->left = node1;
        node3->right = node2;
        nodes[size] = node3;
        size++;
        std::sort(nodes, nodes + size, compare);
    }
    return nodes[0];
}

void showTree(TreeNode* root) {
    if (!root) {
        return;
    }
    showTree(root->left);
    std::cout << root->val << " ";
    showTree(root->right);
}

int main() {
    int* nums = new int[8];
    for (int i = 0; i < 8; i++) {
        if (i % 2) {
            nums[i] = i + 2;
        } else {
            nums[i] = i + 3;
        }
    }
    TreeNode* hfmRoot = buildHfmTree(nums, 8);
    showTree(hfmRoot);
    return 0;
}
```
