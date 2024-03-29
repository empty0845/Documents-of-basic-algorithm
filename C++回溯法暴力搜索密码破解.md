## 回溯法

```c++
#include <iostream>
#include <vector>

class Solution {
private:
    // 回溯法获取所有可能
    void dfs(std::vector<std::string>& allChances, std::vector<char>& cur, int lower, int upper) {
        if (cur.size() > upper) {
            return;
        }
        if (cur.size() < lower) {
            for (int i = 32; i < 127; i++) {
                char c = i;
                cur.push_back(c);
                dfs(allChances, cur, lower, upper);
                cur.pop_back();
            }
        } else {
            std::string curString(cur.begin(), cur.end());
            allChances.push_back(curString);
            for (int i = 32; i < 127; i++) {
                char c = i;
                cur.push_back(c);
                dfs(allChances, cur, lower, upper);
                cur.pop_back();
            }
        }
    }
public:
    // 将所有可能存入数组
    std::vector<std::string> search(int lower, int upper) {
        std::vector<char> cur;
        std::vector<std::string> allChances;
        dfs(allChances, cur, lower, upper);
        return allChances;
    }
    /*
    // 将所有可能一一遍历，并尝试跟手机的密码匹配（假如有一个函数：```bool isRight(std::string password);```，判断密码是否正确）
    std::string tryPassword(int lower, int upper) {
        std::vector<std::string> allChances = search(lower, upper);
        for (string s: allChances) {
            if (isRight(s)) {
                return s;
            }
        }
        return "";
    }
    */
};


int main() {
    int lower = 0;
    int upper = 0;
    std::cout << "请输入密码最少的可能位数：" << std::endl;
    std::cin >> lower;
    std::cout << "请输入密码最多的可能位数：" << std::endl;
    std::cin >> upper;
    Solution s;
    //std::string password = s.tryPassword(); // 如果有isRight函数，就可以直接用这行代码
    std::vector<std::string> allChances = s.search(lower, upper);
    for (std::string s: allChances) {
        std::cout << s << std::endl;
    }
    return 0;
}
```
