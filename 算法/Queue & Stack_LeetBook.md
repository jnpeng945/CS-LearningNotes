## 队列与栈基本介绍

**队列**和**栈**都是线性数据结构，在**数组**中可以通过索引随机访问元素。 但当我们需要限制处理的顺序，多使用队列和栈。

**队列**：FIFO的处理顺序；**栈**：LIFO的处理顺序。

## 队列

### 1. FIFO以及队列定义

![image-20210511104813154](Figs/Queue%20&%20Stack_LeetBook_1)

FIFO 数据结构中，首先处理添加到队列中的第一个元素。入队（Enqueue）时新元素被添加在队尾元素， 出队（Dequeue)时每次移除队首元素。【插入（insert）— 删除（delete）】

### 2. 队列实现

#### 基于数组

队列实现思路：使用动态数组和指向队列头部的索引，实现队列就是实现**入队和出队**。由于入队会向队列追加一个元素，出队会删除队列第一个元素，我们需要**索引指示起点**。

```cpp
#include<iostream>
#include<vector>
using namespace std;
class MyQueue {
    private:
        vector<int> data;     // 存储队列元素
        int p_start;          // 索引指示起点
    public:
        MyQueue() {p_start = 0;}    // 构造函数
        /** 向队列中插入元素. 成功插入就返回true. */
        bool enQueue(int x) {
            data.push_back(x);
            return true;
        }
        /** 删除队列首元素. 成功删除就返回true. */
        bool deQueue() {
            if (isEmpty()) {return false;}
            p_start++;
            return true;
        };
        /** 获取队列首元素. */
        int Front() { return data[p_start]; };
        /** 验证队列是否为空. */
        bool isEmpty()  { return p_start >= data.size(); }
};

int main() {
    MyQueue q;
    q.enQueue(5);
    q.enQueue(3);
    if (!q.isEmpty())       cout << q.Front() << endl;
    q.deQueue();
    if (!q.isEmpty())       cout << q.Front() << endl;
    q.deQueue();
    if (!q.isEmpty())       cout << q.Front() << endl;
}
```

**基于数组实现队列的缺点**：实现简单，但是某些情况下效率很低。 随着起始指针的移动，浪费了越来越多的空间。 

![image-20210511112038612](Figs/Queue%20&%20Stack_LeetBook_2)

![image-20210511112046919](Figs/Queue%20&%20Stack_LeetBook_5)

假设：我们只能分配一个最大长度为 5 的数组。显然，当我们添加少于 5 个元素时，我们的解决方案很高效。但如果我们将元素 5 出队，此时队列还可以添加一个元素，但是受限于数组实现队列的缺陷，而浪费了空间。

#### 循环队列改进

**思路：** 为了对上述方案中空间浪费进行改进，<u>固定大小的数组</u>和<u>双指针指示起始和结束位置</u>。 

![image-20210511173000605](Figs/Queue%20&%20Stack_LeetBook_3)

![image-20210511173020430](Figs/Queue%20&%20Stack_LeetBook_4)



**设计循环队列：**leetcode [622. 设计循环队列](https://leetcode-cn.com/problems/design-circular-queue/)

```cpp
class MyCircularQueue {
private:
    vector<int> data;
    int head;
    int tail;
    int size;
public:
    MyCircularQueue(int k) {		/** 数据结构初始化. 队列大小设置为 k. */
        data.resize(k);
        head = -1;
        tail = -1;
        size = k;
    }
    bool enQueue(int value) {		/** 循环队列中插入元素. 若成功插入元素则 Return true. */
        if (isEmpty()) {
            head = 0;
        } else if (isFull()) {
            return false;
        } 
        tail = (tail + 1) % size;
        data[tail] = value;
        return true;
    }
    bool deQueue() {		/** 循环队列中删除元素. 若成功删除元素则Return true. */
        if (isEmpty()) {
            return false;
        }
        if (head == tail) {
            head = -1;
            tail = -1;
            return true;
        }
        head = (head + 1) % size;
        return true;
    }
    int Front() {			/** 获得queue首部元素. */
        if (isEmpty()) {
            return -1;
        }
        return data[head];
    }
    int Rear() {			/** 获得queue尾部元素. */
        if (isEmpty()) {
            return -1;
        }
        return data[tail];
    }
    bool isEmpty() {		/** 判断队列是否为空. */
        return head == -1;
    } 
    bool isFull() {			/** 判断队列是否已满. */
        return ((tail + 1) % size) == head;
    }
};
```

上述代码中需要注意的细节：

```cpp
// 如果在函数 enQueue 中执行：
    if (isEmpty()) {
    head = 0;
    } 
    if (isFull()) {
    return false;
    } 
会出现如下错误：Line 1038: Char 34: runtime error: addition of unsigned offset to 0x602000000070 overflowed to 0x60200000006c (stl_vector.h)
解决方法：先判断	isFull()	再判断		isEmpty() 函数,或者加上else if 语句。
```

### 3. 内置队列结构

例子：使用内置队列库

```cpp
#include <iostream>
#include<queue>
using namespace std;
int main() {
    queue<int> q;       // 1. Initialize a queue.
    q.push(5);          // 2. Push new element.
    q.push(13);
    q.push(8);
    q.push(6);
    if (q.empty()) {    // 3. Check if queue is empty.
        cout << "Queue is empty!" << endl;
        return 0;
    }
    q.pop();            // 4. Pop an element.
    cout << "The first element is: " << q.front() << endl;      // 5. Get the first element.
    cout << "The last element is: " << q.back() << endl;        // 6. Get the last element.
    cout << "The size is: " << q.size() << endl;                // 7. Get the size of the queue.
}
```

### 4. 队列及其应用

[剑指 Offer 59 - I. 滑动窗口的最大值](https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/)

## 队列和广度优先搜索

广度优先搜索（**BFS**）是一种**遍历或搜索数据结构**（如树或图）的算法，用于`遍历`和`找出最短路径`。 BFS 可用于在树中执行<u>层序遍历</u>，也可使用 <u>BFS 遍历图</u>。

**扩展：**抽象的情景中使用 BFS 遍历所有可能的状态。此时，将状态看作是图中的结点，而以合法的过渡路径作为图中的边。

### 1 队列和BFS

BFS 应用举例： 找到从根结点到目标结点的最短路径。

1. 结点的处理顺序？

   类似于`树的层序遍历` ，越接近根节点越早被遍历。在第一轮中，处理根结点。第二轮中，我们处理根结点旁边的结点；等等。

2. 队列的入队以及出队顺序？

   首先将根结点排入队列。在每一轮中，我们依次队列中的结点，并将所有邻居加到队列。其中，新添加的节点不会立即遍历，而是在下一轮中处理。

**BFS 中使用队列的原因**：结点的处理顺序与它们添加到队列的顺序是完全相同的顺序，即先进先出（FIFO）。

### 2 BFS模板

BFS使用时注意：确定**结点和边缘**。通常，结点是实际结点或状态，而边缘是实际边缘或可能的转换。

```cpp
// 模板1：Return root 和 target node的最短路径
int BFS(Node root, Node target) {
    Queue<Node> queue;  // store all nodes which are waiting to be processed
    int step = 0;       // 从root 到当前 node 所需的 steps 数
    // initialize
    add root to queue;
    // BFS
    while (!queue.empty()) {
        ++step;
        // 遍历队列中的节点
        int size = queue.size();
        for (int i = 0; i < size; ++i) {
            Node cur = queue.front();
            return step if cur is target;
            for (Node next : the neighbors of cur) {
                add next to queue;
            }
            remove the first node from queue;
        }
    }
    return -1;          // root 到 target无可行路径
}
```

代码解释：

1. 每一轮中，队列中的结点是等待处理的结点;
2. 每个更外层的 while 循环之后，我们距离根结点更远一步。变量 step 指示从根结点到我们正在访问的当前结点的距离。



为确保我们不会访问一个结点两次，否则会陷入无限循环，需要添加**哈希集**。

```cpp
// 模板2：Return root 和 target node的最短路径
int BFS(Node root, Node target) {
    Queue<Node> queue;  // store all nodes which are waiting to be processed
    Set<Node> used;     // store 全部已用的 nodes
    int step = 0;       // 从root 到当前 node 所需的 steps 数
    // initialize
    add root to queue;
    add root to used;
    // BFS
    while (queue is not empty) {
        step = step + 1;
        // 遍历队列中的节点
        int size = queue.size();
        for (int i = 0; i < size; ++i) {
            Node cur = the first node in queue;
            return step if cur is target;
            for (Node next : the neighbors of cur) {
                if (next is not in used) {
                    add next to queue;
                    add next to used;
                }
            }
            remove the first node from queue;
        }
    }
    return -1;          // root 到 target无可行路径
}
```

不需要使用哈希集的情况：

1. 完全确定没有循环，如树遍历；
2. 确实希望多次将结点加入队列中；



### 3 应用

#### 例题1：[200. 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)

给你一个由 '1'（陆地）和 '0'（水）组成的的二维网格，请你计算网格中岛屿的数量。岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形成。此外，你可以假设该网格的四条边均被水包围。

```cpp
输入：grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
输出：1
```

思路1: DFS。将二维网格——>无向图，竖直或水平相邻的 1 之间有边相连。扫描整个二维网格，如果一个位置为 1，则**以其为起始节点并向左右上下**开始进行深度优先搜索，在深度优先搜索的过程中，每个搜索到的 1 都会被重新标记为 0。最终岛屿的数量就是我们进行<u>深度优先搜索的次数</u>。



```cpp
class Solution {
private:
    void dfs(vector<vector<char>> &grid, int i, int j) {
        // i 表示行, j 表示列
        int m = grid.size();
        int n = grid[0].size();
        grid[i][j] = '0';		// 搜索过的 ‘1’ 重新标记为 ‘0’
        if (i - 1 >= 0 && grid[i - 1][j] == '1')    dfs(grid, i - 1, j);	// 左
        if (i + 1 < m && grid[i + 1][j] == '1')     dfs(grid, i + 1, j);	// 右
        if (j - 1 >= 0 && grid[i][j - 1] == '1')    dfs(grid, i, j - 1);	// 上
        if (j + 1 < n && grid[i][j + 1] == '1')     dfs(grid, i, j + 1);	// 下
    }
public:
    int numIslands(vector<vector<char>>& grid) {
        int m = grid.size();
        if (m == 0) return 0;
        int n = grid[0].size();
        int ans = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == '1') {
                    dfs(grid, i, j);
                    ++ans;		// dfs的次数
                }
            }
        }
        return ans;
    }
};
```



思路2：BFS。扫描整个二维网格。如果一个位置为 1，则将其加入队列，开始进行广度优先搜索。在广度优先搜索的过程中，每个搜索到的 1 都会被重新标记为 0。直到队列为空，搜索结束。最终岛屿的数量就是我们进行<u>广度优先搜索的次数</u>。

```cpp
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        int m = grid.size();
        if (m == 0) return 0;
        int n = grid[0].size();

        int ans = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == '1') {
                    ++ans;
                    grid[i][j] = '0';
                    queue<pair<int, int>> que;
                    que.push({i, j});
                    while(!que.empty()) {
                        auto tmp = que.front();
                        que.pop();
                        int row = tmp.first, col = tmp.second;
                        if (row - 1 >= 0 && grid[row - 1][col] == '1') {
                            que.push({row - 1, col});
                            grid[row - 1][col] = '0';
                        }
                        if (row + 1 < m && grid[row + 1][col] == '1') {
                            que.push({row + 1, col});
                            grid[row + 1][col] = '0';
                        }
                        if (col - 1 >= 0 && grid[row][col - 1] == '1') {
                            que.push({row, col - 1});
                            grid[row][col - 1] = '0';
                        }
                        if (col + 1 < n && grid[row][col + 1] == '1') {
                            que.push({row, col + 1});
                            grid[row][col + 1] = '0';
                        }
                    }
                }
            }
        }
        return ans;
    }
};
```

#### 例题2：[752. 打开转盘锁](https://leetcode-cn.com/problems/open-the-lock/)

BFS：从0000开始，每次按照一层一层（就是目前队列的大小）去遍历，然后递加层数，每一层遍历时候取出当前数字，排除deadend的情况，会有8种修改的可能，排除visited后插入到队列里，如果找到则返回层数，则如果都找不到则失败返回-1。

```cpp
#include<bits/stdc++.h>
using namespace std;
class Solution {
public:
    int openLock(vector<string>& deadends, string target) {
        // 记录已经遍历过的数字
        unordered_set<string> visited;
        // 转换为set方便查找
        unordered_set<string> deads(deadends.begin(), deadends.end());
        // 创建队列并将查找起点加入队列中
        queue<string> q;
        q.push("0000");
        visited.insert("0000");
        // 当前层数
        int depth = 0;
        // 增加或者减少一位数字
        vector<int> two{-1, 1};
        while (!q.empty()) {
            int n = q.size();
            // q pop或者 push 会改变q.size()的值，所以不能用 q.size() 代替 n
            for (int i = 0; i < n; ++i) {    // 遍历当前队列元素
                string curr = q.front();
                q.pop();
                visited.insert(curr);
                if (curr == target)     return depth;   // 找到
                else if (deads.find(curr) == deads.end()) { // 没找到但不在deads中
                    for (int j = 0; j < 4; ++j) {
                        for (int k : two) {
                            string next = curr.substr(0, j) + to_string((curr[j] - '0' + k + 10) % 10) + curr.substr(j + 1, 4 - j);
                            // cout << curr << " " << next << endl;
                            if (visited.find(next) == visited.end()) {
                                visited.insert(next);
                                q.push(next);
                            }
                        }
                    }
                }

            }
            ++depth;
        }
        return -1;
    }
};
int main() {
    Solution A;
    // 6
    vector<string> deadends{"0201","0101","0102","1212","2002"};
    string target{"0202"};
    // // 1
    // vector<string> deadends{"0000"};
    // string target{"0009"};
    // // -1
    // vector<string> deadends{"8887","8889","8878","8898","8788","8988","7888","9888"};
    // string target{"8888"};
    // // -1
    // vector<string> deadends{"0000"};
    // string target{"8888"};
    cout << A.openLock(deadends, target);

    return 0;
}
```



#### 例题3：[279. 完全平方数](https://leetcode-cn.com/problems/perfect-squares/)

贪心+BFS：

```c++
class Solution {
public:
    int numSquares(int n) {
        vector<int> squareNums;
        for (int i = 1; i * i <= n; ++i) {
            squareNums.push_back(i * i);
        }
        set<int> s;
        s.insert(n);
        int ans = 0;
        while(s.size() > 0) {
            ++ans;
            set<int> nextS;
            for (int remainder : s) {
                for (int square : squareNums) {
                    if (remainder == square)    return ans;
                    else if (remainder < square)      break;
                    else    nextS.insert(remainder - square);
                }
            }
            s = nextS;
        }
        return ans;
    }
};
```

## 栈

### 1. 定义

```c++
// 入栈操作
push(x)
// 出栈操作
pop()
```

### 2. 基于动态数组实现栈

```cpp
#include <iostream>
#include <vector>
using namespace std;
class MyStack {
    private:
        vector<int> data;              
    public:
        void push(int x) {data.push_back(x);}
        bool isEmpty() {return data.empty();}
        // 从队列中获取顶部的元素
        int top() {return data.back();}
        bool pop() {
            if (isEmpty()) {
                return false;
            }
            data.pop_back();    // 删除data 的尾元素
            return true;
        }
};
int main() {
    MyStack s;
    s.push(1);
    s.push(2);
    s.push(3);
    for (int i = 0; i < 4; ++i) {
        if (!s.isEmpty()) {
            cout << s.top() << endl;
        }
        cout << (s.pop() ? "true" : "false") << endl;
    }
}
```

### 3. 内置栈结构

```cpp
#include <iostream>
#include <stack>
using namespace std;
int main() {
    stack<int> s;
    s.push(5);
    s.push(13);
    s.push(8);
    s.push(6);
    if (s.empty()) {
        cout << "Stack is empty!" << endl;
        return 0;
    }
    s.pop();
    cout << "The top element is: " << s.top() << endl;
    cout << "The size is: " << s.size() << endl;
}
```

### 4. 应用

#### 例题1：[20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)

```cpp
class Solution {
public:
    bool isValid(string s) {
        int n = s.size();
        if (n & 1)  return false;       // s中字符数为奇数，直接return false
        stack<int> stk;
        for (const char &ch : s) {
            if(ch == '(' || ch == '[' || ch == '{') {   // 遇到左括号，加入栈
                stk.push(ch);
                continue;
            }
            if (!stk.empty()) {
                if (stk.top() == '(' && ch == ')')  stk.pop();      // 匹配
                else if (stk.top() == '[' && ch == ']')   stk.pop();
                else if (stk.top() == '{' && ch == '}')   stk.pop();
                else return false;      // case2: 相邻括号不匹配
            } else {         // case1: 栈为空，此时还有右括号需要匹配，false
                return false;
            }
        }
        return stk.empty();     // case3: 栈非空，也就是左括号多余，false; 栈为空，true
    }
};
```

上述方案进行改进：

```c++
class Solution {
public:
    bool isValid(string s) {
        int n = s.size();
        if (n & 1)  return false;       // s中字符数为奇数，直接return false
        stack<int> stk;
        for (const char &ch : s) {
            if (ch == '(')      stk.push(')');		// 遇到左括号，压入右括号
            else if (ch == '[')      stk.push(']');
            else if (ch == '{')      stk.push('}');
            else if (stk.empty() || stk.top() != ch)   return false;	// 右括号多余，左右括号不匹配
            else stk.pop();
        }
        return stk.empty();		// 左括号多余
    }
};
```



#### 例题2：[739. 每日温度](https://leetcode-cn.com/problems/daily-temperatures/)

解法1：针对每个温度值向后进行依次搜索，找到比当前温度更高的值。原理就是除了最后一个数其他所有的数都遍历一次，最后一个数据对应的结果肯定是0，就不需要计算。遍历时，每个数都去向后查询，直到找到比它大的数。

```cpp
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        // 两次遍历，对数时间复杂度
        int n = temperatures.size();
        vector<int> res(n, 0);
        for (int i = 0; i < n - 1; ++i) {
            int ans = 1;
            for (int j = i + 1; j < n; ++j) {
                if (temperatures[i] < temperatures[j]) {
                    res[i] = ans;
                    break;
                } 
                ++ans;
            }
            // 气温在这之后都不会升高，元素位置已经用0代替了
        }
        return res;
    }
};
```

解法2：单调递减栈保存数组下标

遍历整个数组，1）栈非空且当前数字大于栈顶元素（如果直接入栈的话就不是递减栈），取出栈顶元素。由于当前元素大于栈顶元素，而且一定是第一个大于栈顶元素的数，下标之差就是二者的距离。同时将当前数字压入栈中。2）对于新的栈顶元素，直到当前数字小于等于栈顶元素停止，然后将数字入栈，这样就可以一直保持递减栈，且每个数字和第一个大于它的数的距离可求。

 ```cpp
 class Solution {
 public:
     vector<int> dailyTemperatures(vector<int>& temperatures) {
         int n = temperatures.size();
         vector<int> res(n, 0);
         stack<int> stk;     // 单调递减栈存储下标
         // 一次遍历
         for(int i = 0; i < n; ++i) {
             // 栈非空且当前元素值大于栈顶元素值
             while (!stk.empty() && temperatures[i] > temperatures[stk.top()]) {
                 auto t = stk.top();      stk.pop();
                 res[t] = i - t;
             }
             stk.push(i);
         }
         return res;
     }
 };
 ```



#### 例题3：[150. 逆波兰表达式求值](https://leetcode-cn.com/problems/evaluate-reverse-polish-notation/)

```cpp
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        // 遇到数字则入栈，遇到算符就取出栈顶两个数字进行计算，并将计算结果压入栈
        stack<int> stk;
        for (int i = 0; i < tokens.size(); ++i) {
            if (tokens[i] == "+" || tokens[i] == "-" || tokens[i] == "*" || tokens[i] == "/" ) {
                int tmp1 = stk.top();   stk.pop();
                int tmp2 = stk.top();   stk.pop();

                if (tokens[i] == "+")           stk.push(tmp2 + tmp1);
                else if (tokens[i] == "-")      stk.push(tmp2 - tmp1);
                else if (tokens[i] == "*")      stk.push(tmp2 * tmp1);
                else                            stk.push(tmp2 / tmp1);
            } else {
                stk.push(stoi(tokens[i]));
            }
        }
        return stk.top();
    }
};
```



## 栈和深度优先搜索

### 1 递归实现DFS

当我们递归实现 DFS 时，实际上我们使用的是由系统提供的隐式栈，也称调用栈

```cpp
// DFS模板
// 若从 cur 到 target有路径，返回 true
boolean DFS(Node cur, Node target, Set<Node> visited) {
    return true if cur is target;
    for (next : each neighbor of cur) {
        if (next is not in visited) {
            add next to visted;
            return true if DFS(next, target, visited) == true;
        }
    }
    return false;
}
```

### 2 应用

#### 例题1：[133. 克隆图](https://leetcode-cn.com/problems/clone-graph/)

**图的深拷贝**就是构建一张与原图**结构、值**都一样的图，但是其中的节点不再是原来图节点的<u>引用</u>。

思路：题中只给了一个节点的引用，因此为得到整张图的结构以及对应节点的值，我们需要从给定的节点出发，进行图遍历，并在遍历的过程中完成图的深拷贝。

为避免**深拷贝时陷入死循环**，需要理解图的结构。对于一张无向图，任何给定的无向边都可以表示**两个有向边**，即如果节点`A` 和节点`B` 之间存在无向边，则表示该图具有从节点`A`到节点`B`的有向边和从节点`B`到节点`A` 的有向边。因此，使用一种数据结构记录已经被克隆过的节点。

```cpp
// DFS算法
1. 使用哈希表存储所有已被访问和克隆的节点，哈希表中Key 是原始图的节点，value 就是克隆图中的对应节点
2. 从给定节点开始遍历图，如果某个节点已经被访问过，则返回其克隆图中的对应节点。
3. 如果当前节点不在哈希表中，创建它的克隆节点并存储在哈希表中。
4. 递归调用每个节点的邻接点，每个节点递归调用的次数就等于邻接点的数量，每一次调用返回其对应的邻接点的克隆节点，最终返回这些克隆邻接点的列表，将其放入对应克隆节点的邻接表中。
class Solution {
public:
    unordered_map<Node*, Node*> visited;
    Node* cloneGraph(Node* node) {
        if (node == nullptr)    return node;
        // 如果节点已经访问过，从哈希表中取出克隆节点返回
        if (visited.find(node) != visited.end()) {
            return visited[node];
        }
        // 克隆节点，为了深拷贝我们不克隆它的邻居的列表
        Node* cloneNode = new Node(node->val);
        // 哈希表存储
        visited[node] = cloneNode;
        // 遍历该节点的邻居并更新克隆节点的邻居列表
        for(auto &neighbor : node->neighbors) {
            cloneNode->neighbors.emplace_back(cloneGraph(neighbor));
        }
        return cloneNode;
    }
};
```



```cpp
// BFS算法
1. 哈希表存储所有已经访问和克隆的节点，哈希表中的key 是原始图中的节点，value是克隆图中的对应节点
2. 将题目给定的节点添加到队列，克隆节点并存储到哈希表中
3. 每次从队列首部取出一个节点，遍历该节点所有的邻接点。如果某个邻接点已经被访问，则邻接点一定在哈希表中，则从哈希表中获取邻接点；否则创建一个新的节点存储在哈希表中，并将邻接点添加到队列。将克隆的邻接点添加到克隆图对应节点的邻接表中。重复算法操作直到队列为空，则整个图遍历结束。
class Solution {
public:
    Node* cloneGraph(Node* node) {
        if (node == nullptr)    return node;
        unordered_map<Node*, Node*> visited;
        // 题目给定节点加入队列
        queue<Node*> que;
        que.push(node);

        // 克隆第一个节点并存储到哈希表中
        visited[node] = new Node(node->val);
        // BFS
        while(!que.empty()) {
            // 取出队列头节点
            Node* tmp = que.front();
            que.pop();
            // 遍历节点的邻居
            for(auto &neighbor : tmp->neighbors) {
                // 如果没有访问过就存储在哈希表中
                if (visited.find(neighbor) == visited.end()) {
                    visited[neighbor] = new Node(neighbor->val);
                    // 邻居节点加入队列
                    que.push(neighbor);
                }
                // 更新当前节点的邻居列表
                visited[tmp]->neighbors.push_back(visited[neighbor]);
            }
        }
        return visited[node];
    }
};
```

#### 例题2：[494. 目标和](https://leetcode-cn.com/problems/target-sum/)

方法1：递归

枚举所有可能的情况。当我们处理第`i` 个数时，我们添加`+` 或 `-` ，递归搜索这两种情况，当处理完所有的`N` 个数时，我们计算所有的数和，判断是否等于`S`。

```cpp
// 时间复杂度很高
class Solution {
private:
    int count = 0;
    void calculate(vector<int>& nums, int index, int curSum, int target) {
        if (index == nums.size()) {
            if (curSum == target) {     // 递归结束，count自增
                ++count;
            }
        } else {    // 递归搜索 + - 两种情况
            calculate(nums, index + 1, curSum + nums[index], target);
            calculate(nums, index + 1, curSum - nums[index], target);
        }
    }
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        calculate(nums, 0, 0, target);
        return count;
    }
};
```

方法2：动态规划

`dp[i] [j]` 表示用数组的前 `i` 个元素，组成和为 `j` 的方案数，考虑第 `i` 个数为`nums[i]` ，它可以被添加为 `+ -` ，状态转移方程为：
$$
dp[i][j] = dp[i - 1][j - nums[i]] + dp[i - 1][j + nums[i]]
$$
写成递推形式：
$$
dp[i][j + nums[i]] += dp[i - 1][j]\\
dp[i][j - nums[i]] += dp[i - 1][j]
$$
**注意：为什么可以写成上述递推形式？**
$$
dp[i][j] += dp[i - 1][j - nums[i]]\\
dp[i][j] += dp[i - 1][j + nums[i]]
$$
`等式1 右边j - nums[i]用j 替换，等式2 右边j + nums[i]用j 替换`，相应调整左边的 `j`即可。

得到
$$
dp[i][j + nums[i]] += dp[i - 1][j]\\
dp[i][j - nums[i]] += dp[i - 1][j]
$$
**为什么要写成上述递推形式？**





数组中所有数的和不超过1000，那么 `j` 的最小值可以达到 `-1000` 。数组下标为负数就会出现越界情况，因此需要给`dp[i][j]` 的第二维预先增加1000。得到：
$$
dp[i][j + nums[i] + 1000] += dp[i - 1][j + 1000]\\
dp[i][j - nums[i] + 1000] += dp[i - 1][j + 1000]
$$

## 3 栈实现 DFS

`while ` 循环以及 栈 模拟递归期间的 `系统调用栈`。

```cpp
bool dfs(int root, int target) {
    set<Node> visited;
    stack<Node> stk;
    add root to stk;
    while (!stk.empty()) {
        Node cur = stk.top();
        if (cur == target)		return true;
        for (Node next : the neighbors of cur) {
            if (next is not in visited) {
                add next to s;
                add next to visited;
            }
        }
        remove cur from s;
    }
    return false;
}
```

#### 例题3：[94. 二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)

```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        stack<TreeNode*> st;
        while (root != nullptr || !st.empty()) {
            while (root != nullptr) {
                //指针访问结点，访问到最底层
                st.push(root);   //将访问的节点放进栈
                root = root->left;
            }
            root = st.top(); //从栈里弹出的数据，就是要处理的数据（放进result数组里的数据）
            st.pop();
            res.emplace_back(root->val); //中
            root = root->right;   //右
        }
        return res;
    }
};
```

## 小结

#### [394. 字符串解码](https://leetcode-cn.com/problems/decode-string/)

**栈**：

思路：将字母、数字和括号看成独立的符号，并用栈来维护这些符号，遍历这个栈：

- 如果当前字符为数字，解析出一个数字并进栈
- 如果当前字符是字母或者左括号，进栈
- 如果当前字符为右括号，开始出栈，直到左括号出栈。出栈序列反转后拼接成一个字符串，此时取出栈顶的数字，就是这个字符串应该出现的次数，我们根据这个次数和字符串构造新的字符串并进栈

使用不定长数组模拟栈操作，方便从栈底向栈顶遍历。

特殊情况处理：2[a2[bc]] 先转换为 2[abcbc]

```cpp
// case1
class Solution {
public:
    string getDigits(string &s, int &index) {
        string ret = "";
        while (isdigit(s[index])) {
            ret += s[index];
            ++index;
        }
        return ret;
    }
    string getString(vector<string> &v) {
        string ret;
        for (const auto &s : v) {
            ret += s;
        }
        return ret;
    }
    string decodeString(string s) {
        vector<string> stk;
        int index = 0;
        while (index < s.size()) {
            char cur = s[index];
            if (isdigit(cur)) {
                // 获取一个数字并进栈
                string digits = getDigits(s, index);
                stk.push_back(digits);
            } else if (isalpha(cur) || cur == '[') {
                // 字母或者左括号进栈
                stk.push_back(string(1, s[index++]));
            } else {
                ++index;
                vector<string> sub;
                while (stk.back() != "[") {
                    sub.push_back(stk.back());
                    stk.pop_back();
                }
                reverse(sub.begin(), sub.end());
                // 左括号出栈
                stk.pop_back();
                // 栈顶为当前sub 对应的字符串应该出现的次数
                int repTime = stoi(stk.back());
                stk.pop_back();
                string t, o = getString(sub);
                // 构造字符串
                while(repTime--) {
                    t += o;
                }
                // 构造好的字符串入栈
                stk.push_back(t);
            }
        }
        return getString(stk);
    }
};
// case2 辅助栈
class Solution {
public:
    string decodeString(string s) {
        stack<int> nums;
        stack<string> strs;
        int num = 0;
        string res = "";
        
        for (int i = 0; i < s.size(); ++i) {
            if (s[i] >= '0' && s[i] <= '9') {
                num = num * 10 + s[i] - '0';
            } else if ((s[i] >= 'a' && s[i] <= 'z') || (s[i] >= 'A' && s[i] <= 'Z')) {
                res += s[i];
            } else if (s[i] == '[') {   // 将[ 前的数字压入栈，字母字符串压入strs栈内
                nums.push(num);
                num = 0;
                strs.push(res);
                res = "";
            } else {    // 遇到],操作与之相匹配的 [ 之间的字符，使用分配律
                int times = nums.top();
                nums.pop();
                for (int j = 0; j < times; ++j) {
                    strs.top() += res;
                }
                res = strs.top();
                strs.pop();
            }
        }
        return res;
    }
};
```



#### [733. 图像渲染](https://leetcode-cn.com/problems/flood-fill/)

```cpp
// BFS
class Solution {
public:
    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int newColor) {
        int curColor = image[sr][sc];   // 记录起点坐标处的像素值
        if (curColor == newColor)   return image;
        int m = image.size(), n = image[0].size();
        queue<pair<int, int>> que;
        que.emplace(sr, sc);   // 起点坐标元素加入队列中
        image[sr][sc] = newColor;   // 更改起点坐标元素的像素值

        while (!que.empty()) {
            int x = que.front().first, y = que.front().second;
            que.pop();
            // 上 下 左 右, 分四个方向更改像素值
            for (int i = 0; i < 4; ++i) {
                int mx = x + dx[i], my = y + dy[i];
                // 小心下标越界
                if (mx >= 0 && mx < m && my >= 0 && my < n && image[mx][my] == curColor) {
                    que.emplace(mx, my);
                    image[mx][my] = newColor;
                }
            }
        }
        return image;
    }
private:
    const int dx[4] = {0, 0, -1, 1};
    const int dy[4] = {-1, 1, 0, 0};
};

// DFS
class Solution {
private: 
    // 上下左右 进行偏移
    const int dx[4] = {0, 0, -1, 1};
    const int dy[4] = {-1, 1, 0, 0};
    void dfs(vector<vector<int>> &image, int sr, int sc, int &newColor, int &curColor) {
        int m = image.size(), n = image[0].size();
        if(image[sr][sc] == curColor) {
            image[sr][sc] = newColor;   // 起点位置像素值更新
            // 按照起点位置 上下左右进行像素值更新
            for (int i = 0; i < 4; ++i) {
                int mx = sr + dx[i], my = sc + dy[i];
                if (mx >= 0 && mx < m && my >= 0 && my < n) {
                    dfs(image, mx, my, newColor, curColor);
                }
            }
        }
    }
public:
    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int newColor) {
        int curColor = image[sr][sc];
        if (curColor != newColor) {     // 起点位置处像素值和新的像素值不相同，进行dfs
            dfs(image, sr, sc, newColor, curColor);
        }
        return image;   // 起点位置处像素值和新的像素值相同，直接return
    }
};
```





## 岛屿问题

[参考](https://leetcode-cn.com/problems/island-perimeter/solution/tu-jie-jian-ji-er-qiao-miao-de-dfs-fang-fa-java-by/)

如何在网格上做DFS

网格问题背景：有m * n 个小方格组成一个网格，每个方格与其上下左右四个方格相邻。

首先，每个方格与其上下左右的四个方格相邻，则 DFS 分四个方向进行：

```cpp
void dfs(vector<int> grid, int i, int j) {
    dfs(grid, i - 1, j);
    dfs(grid, i + 1, j);
    dfs(grid, i, j - 1);
    dfs(grid, i, j + 1);
}
```

对于网格边缘的方格，上下左右并不都有邻居。解决方法：

1. 递归调用前判断方格的位置，例如位于左边缘，则不访问其左邻居。
2. 用“先污染后治理”的方法，先做递归调用，再在每个 DFS 函数的开头判断坐标是否合法，不合法的直接返回。同样地，我们还需要判断该方格是否有岛屿（值是否为 1），否则也需要返回。

```cpp
void dfs(vector<int> grid, int i, int j) {
    // 坐标位置越界
    if(!(0 <= i && i < grid.size() && 0 <= j && j < gird[0].size())) {
        return;
    }
    // 方格不是岛屿，直接返回
    if(grid[i][j] != 1)	{
        return;
    }
    dfs(grid, i - 1, j);
    dfs(grid, i + 1, j);
    dfs(grid, i, j - 1);
    dfs(grid, i, j + 1);
}
```

然后我们需要标记遍历过的方格，保证方格不进行重复遍历，为此只需改变方格中的值。

```cpp
void dfs(vector<int> grid, int i, int j) {
    // 坐标位置越界
    if(!(0 <= i && i < grid.size() && 0 <= j && j < gird[0].size())) {
        return;
    }
    // 方格不是岛屿，直接返回
    if(grid[i][j] != 1)	{
        return;
    }
    grid[i][j] = 2;		// 方格标记为“已经遍历”
    dfs(grid, i - 1, j);
    dfs(grid, i + 1, j);
    dfs(grid, i, j - 1);
    dfs(grid, i, j + 1);
}
```

岛屿周长：岛屿方格和非岛屿方格相邻的边的数量，非岛屿方格包括水域方格以及网格的边界。

```cpp
int dfs(vector<int> grid, int i, int j) {
    // 从岛屿方格走向网格边界，周长加1
    if(!(0 <= i && i < grid.size() && 0 <= j && j < gird[0].size())) {
        return 1;
    }
    // 从岛屿方格走向水域方格，周长加1
    if(grid[i][j] == 0)	{
        return 1;
    }
    // 不是岛屿
    if(grid[i][j] != 1) {
        return 0;
    }
    grid[i][j] = 2;		// 方格标记为“已经遍历”
    return dfs(grid, i - 1, j) + dfs(grid, i + 1, j) + dfs(grid, i, j - 1) + dfs(grid, i, j + 1);
}
```



参考：[队列 & 栈 - LeetBook](https://leetcode-cn.com/leetbook/detail/queue-stack/)
