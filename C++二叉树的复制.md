二叉树的复制
===========

## Code


```c++
#include <iostream>
#include <queue>
#include <unordered_map>
#include <random>

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
    std::random_device rd;
    std::mt19937 gen(rd());
    std::uniform_int_distribution<int> dis(0, 1);
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
    std::random_device rd;
    std::mt19937 gen(rd());
    std::uniform_int_distribution<int> dis(lowerBound, upperBound);
    TreeNode* root = new TreeNode(dis(gen));
    for (int i = 0; i < numberOfNodes; i++) {
        TreeNode* newNode = new TreeNode(dis(gen));
        randomlyInsertNode(root, newNode);
    }
    return root;
}

TreeNode* copyTree(TreeNode* root) {
    if (!root) return nullptr;
    auto dfs = [&] (auto& self, TreeNode* node) -> TreeNode* {
        if (!node) return nullptr;
        TreeNode* newNode = new TreeNode(node->val);
        newNode->left = self(self, node->left);
        newNode->right = self(self, node->right);
        return newNode;
    };
    return dfs(dfs, root);
}

void showTree(TreeNode* root) {
    if (!root) return;
    std::queue<TreeNode*> q;
    q.push(root);
    while (!q.empty()) {
        int n = q.size();
        for (int i = 0; i < n; ++i) {
            TreeNode* node = q.front();
            q.pop();
            if (node->left) q.push(node->left);
            if (node->right) q.push(node->right);
            std::cout << node->val << " ";
        }
        std::cout << std::endl;
    }
}

int main() {
    TreeNode* root = randomlyCreateBinaryTree();
    showTree(root);
    std::cout << std::endl;
    std::cout << newRoot->val << std::endl;
    showTree(newRoot);
    return 0;
}
```