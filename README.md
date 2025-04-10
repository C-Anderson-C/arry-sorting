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
        vector<vector<int>> arry;  //两个vector表示二维动态数组，外层为所有行，内层为每行中的元素
        cout << "请输入每个数组元素，以空格分开，按两次Enter键以完成输入（按q退出）：" << endl;

        int emptyLineCount = 0; // 用于记录连续空行的数量

        while (true) {
            string input;  //将输入转换为字符串
            getline(cin, input); // 读取一行输入

            // 检测退出条件
            if (input == "q") {
                cout << "程序结束" << endl;
                break;
            }

                         if (input.empty()) {  // 检测连续空行
                emptyLineCount++;
                if (emptyLineCount == 1) { // 如果一行为空，结束输入
                    cout << "输入结束" << endl;
                    break;
                }
                continue; // 跳过当前循环，等待下一行输入
            } else {
                emptyLineCount = 0; // 如果输入非空，重置空行计数器
            }

            try {
                stringstream ss(input); //将输入放入stringstream流中，他会读取输入并自动跳过空格，以识别用户输入的整数
                vector<int> temp;  //每次将用户输入的一行数据存到该数组
                int num;

                while (ss >> num) {  //逐个将用户的输入存到num中
                    temp.push_back(num);  //将num中的整数推入temp动态数组中
                }

                if (ss.fail() && !ss.eof()) {  //如果解析失败且未到达字符串末端，说明输入含有非法字符
                    throw invalid_argument("输入包含非整数，请重新输入！");
                }

                if (temp.empty()) {  //检查输入是否为空，若为空可能是解析失败，跳出循环，提示用户重新输入
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

        
        for (size_t i = 0; i < arry.size(); i++) {  // 对每个子数组进行排序
            sort(arry[i].begin(), arry[i].end());
        }

  
        cout << "你输入的数组是：" << endl;
        for (size_t i = 0; i < arry.size(); i++) {  // 输出数组内容
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
