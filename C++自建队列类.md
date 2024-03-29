```c++
struct ListNode {
  int val;
	ListNode* next;
	ListNode(int val = 0, ListNode* next = nullptr): val(val), next(next) {}
};

struct Queue {
	ListNode* dummy = new ListNode();
	ListNode* tail = dummy;
	int len;
	Queue() {
		len = 0;
	}
	bool empty() {
		return !len;
	}
	int front() {
		if (!dummy->next) {
			return -1;
		}
		return dummy->next->val;
	}
	void push(int val) {
		ListNode* newNode = new ListNode(val);
		tail->next = newNode;
		tail = tail->next;
		len++;
	}
	int pop() {
		if (!dummy->next) {
			return -1;
		}
		ListNode* node = dummy->next;
		int ans = node->val;
		dummy->next = dummy->next->next;
		node->next = nullptr;
		len--;
		return ans;
	}
};

```
