```c++
#include<iostream>
using namespace std;

struct Stack {
    int* nums = new int[100];
    int pos;
    Stack() {
	    pos = 0;
    }
    ~Stack() {
	    delete[] nums;
    }
    bool isEmpty() {
	    return !pos;
    }
    void push(int val) {
        if (pos > 99) {
            return;
        }
		nums[pos] = val;
		pos++;
	}
	int top() {
		return nums[pos - 1];
	}
	int pop() {
		if (pos == 0) {
			return -1;
		}
		int ans = nums[pos - 1];
		pos--;
		return ans;
	}
};

int main() {
	Stack stack;
	stack.push(1);
	stack.push(2);
	cout << stack.isEmpty() << endl;
	cout << stack.top() << endl;
	cout << stack.pop() << endl;
	cout << stack.top() << endl;
	cout << stack.pop() << endl;
	cout << stack.isEmpty() << endl;
	return 0;
}
```
