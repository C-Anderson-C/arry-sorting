# arry-sorting
# 将用户输入的数组组成数组并按升序进行排序
```cpp
#include <iostream>
#include <sstream>
#include <algorithm>
#include <vector>
#include <string>
#include <stdexcept>
using namespace std;

int main() {
    try {
        vector<vector<int>> arry;
        cout << "请输入每个数组元素，以空格分开，按两次Enter键以完成输入（按q退出）：" << endl;

        int emptyLineCount = 0; // 用于记录连续空行的数量

        while (true) {
            string input;
            getline(cin, input); // 读取一行输入

            // 检测退出条件
            if (input == "q") {
                cout << "程序结束" << endl;
                break;
            }

            // 检测连续空行
            if (input.empty()) {
                emptyLineCount++;
                if (emptyLineCount == 1) { // 如果连续两行为空，结束输入
                    cout << "输入结束" << endl;
                    break;
                }
                continue; // 跳过当前循环，等待下一行输入
            } else {
                emptyLineCount = 0; // 如果输入非空，重置空行计数器
            }

            try {
                stringstream ss(input);
                vector<int> temp;
                int num;

                // 尝试解析输入
                while (ss >> num) {
                    temp.push_back(num);
                }

                // 检查是否有无效输入
                if (ss.fail() && !ss.eof()) {
                    throw invalid_argument("输入包含非整数，请重新输入！");
                }

                // 检查是否解析出了有效数字
                if (temp.empty()) {
                    cout << "输入无效，请输入有效的数字！" << endl;
                    continue;
                }

                ss.clear(); // 重置 stringstream 的状态
                arry.push_back(temp);
            } catch (const invalid_argument& e) {
                cout << "错误：" << e.what() << endl;
            } catch (const exception& e) {
                cout << "未知错误：" << e.what() << endl;
            }
        }

        // 对每个子数组进行排序
        for (size_t i = 0; i < arry.size(); i++) {
            sort(arry[i].begin(), arry[i].end());
        }

        // 输出数组内容
        cout << "你输入的数组是：" << endl;
        for (size_t i = 0; i < arry.size(); i++) {
            cout << "arry[" << i + 1 << "] = [ ";
            if (arry[i].empty()) {
                cout << "]" << endl; // 输出空数组
            } else {
                for (int num : arry[i]) {
                    cout << num << " ";
                }
                cout << "]" << endl;
            }
        }
    } catch (const exception& e) {
        cout << "程序发生未知错误：" << e.what() << endl;
    }

    return 0;
}
