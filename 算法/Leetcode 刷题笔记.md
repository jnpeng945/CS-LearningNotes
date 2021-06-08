## 数组

#### 35. 搜索插入位置-二分法

**解题思路：**

<img src="Figs/Leetcode%20%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0_1" alt="图片" style="zoom:67%;" />

目标值在数组中的位置分为四种情况，刚好对应于四种输入示例。

- 在数组所有元素之前
- 等于数组中的某个元素
- 目标值在数组元素之间
- 目标值在数组元素的最后

**解法1：**

```c++
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        for (int i = 0; i < nums.size(); i++) {
        //在数组所有元素之前
        //等于数组中的某个元素
        //目标值在数组元素之间
            if ( nums[i] >= target) {
                return i;
            }
        }
        //目标值在数组元素的最后
        return nums.size();

    }
};
```

**解法2：**

**只要看到面试题里给出的数组是有序数组，都可以想一想是否可以使用二分法。**

```c++
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int length = nums.size();
        int left = 0;
        int right = length - 1;	// 定义target在左闭右闭的区间里，[left, right] 
        while (left <= right) {	// 当left==right，区间[left, right]依然有效
            int middle = (left + right) / 2;
            if ( nums[middle] < target ) {
                left = middle + 1;	// target 在右区间，所以[middle + 1, right]
            }
            else if (  nums[middle] > target ) {
                right = middle - 1;	// target 在左区间，所以[left, middle - 1]
            }
            else {
                return middle;
            }     
        }
        // 分别处理如下四种情况
        // 目标值在数组所有元素之前  [0, -1]
        // 目标值等于数组中某一个元素  return middle;
        // 目标值插入数组中的位置 [left, right]，return  right + 1
        // 目标值在数组所有元素之后的情况 [left, right]， return right + 1
        return right + 1;
    }
};
```

#### 27. 移除元素

**解题思路：**数组的元素在内存地址中是连续的，数组中的元素是不能删掉的，只能覆盖。

- 双指针法
- 暴力求解法，两次 for 循环

```c++
// 时间复杂度：O(n)
// 空间复杂度：O(1)
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int slowIndex = 0; 
        for (int fastIndex = 0; fastIndex < nums.size(); fastIndex++) {  
            if (val != nums[fastIndex]) { 
                nums[slowIndex++] = nums[fastIndex]; 
            }
        }
        return slowIndex;
    }
};
```



#### 209. 长度最小的子数组

**解法1：**暴力解法，两层循环嵌套。

```c++
class Solution {
public:
    int minSubArrayLen(int s, vector<int>& nums) {
        int result = INT32_MAX; // 最终的结果
        int sum = 0; // 子序列的数值之和
        int subLength = 0; // 子序列的长度
        for (int i = 0; i < nums.size(); i++) { // 设置子序列起点为i
            sum = 0;
            for (int j = i; j < nums.size(); j++) { // 设置子序列终止位置为j
                sum += nums[j];
                if (sum >= s) { // 一旦发现子序列和超过了s，更新result
                    subLength = j - i + 1; // 取子序列的长度
                    result = result < subLength ? result : subLength;
                    break; // 因为我们是找符合条件最短的子序列，所以一旦符合条件就break
                }
            }
        }
        // 如果result没有被赋值的话，就返回0，说明没有符合条件的子序列
        return result == INT32_MAX ? 0 : result;
    }
};
```

**解法2：**滑动窗口，不断调节子序列的起始位置和终止位置。滑动窗口可以理解为双指针的一种，只不过这种解法更像是窗口的移动。

本题实现滑动窗口，确定如下两点：

- 窗口内是什么？
  - 窗口是满足其和＞target 的长度最小的连续子数组；
- 如何移动窗口的起始位置和结束位置？
  - 起始位置如何移动：如果当前窗口的值大于target ，窗口就需要向前移动，也就是缩小。
  - 结束位置如何移动：窗口结束的位置就是遍历数组的指针，窗口的起始位置设置为数组的起始位置就可以。

**滑动窗口的精妙之处在于根据当前子序列和大小的情况，不断调节子序列的起始位置。从而将O(n^2)的暴力解法降为O(n)。**

```c++
while (sum >= target) {
	subLength = j - i + 1;
	result = result < subLength ? result : subLength;
	sum -= nums[i++];
}
```

**题解：**

```c++
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int result = INT_MAX;
        int sum = 0;    //滑动窗口数值之和
        int i = 0;      //滑动窗口起始位置
        int subLength = 0; //滑动窗口的长度
        for (int j = 0; j < nums.size(); j++) {
            sum += nums[j];
            // 这里使用while,每次更新 i 起始位置，并不断比较子序列是否符合条件
            while (sum >= target) {
                subLength = j - i + 1;
                result = result < subLength ? result : subLength;
                sum -= nums[i++];
            }
        }
        // 如果result 没有被赋值的话，就返回0，说明没有符合条件的子序列
        return result == INT_MAX ? 0 : result;
    }
};
```

#### 59. 螺旋矩阵 II

求解本题坚持**循环不变量原则**。模拟顺时针画矩阵的过程：

- 填充上行从左到右
- 填充右列从上到下
- 填充下行从右到左
- 填充左列从下到上

这一圈下来，我们要画四条边，这四条边都要坚持一致的左闭右开，或者左开右闭的原则。

<img src="Figs/Leetcode%20%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0_2" alt="image-20210302160400467" style="zoom:50%;" />

这里每一种颜色，代表一条边，我们遍历的长度，每个拐角处的处理规则：拐角处都让新的一条边来继续画。

```c++
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> res(n, vector<int>(n, 0)); //使用vector定义一个二维数组
        int startx = 0, starty = 0; //定义每循环一个圈的起始位置
        int loop = n / 2;   //每个圈循环几次，例如n为奇数3，那么loop = 1只是循环一圈，矩阵中间的值需要单独处理
        int mid = n / 2 ; //矩阵中间的位置，例如:n为3，中间的位置就是（1，1）,n为5，中间位置就是（2，2）
        int count = 1; //用来给矩阵中的每一个空格赋值
        int offset = 1; //每一圈循环，需要控制每一条边遍历的长度
        int i, j;
        while (loop--) {
            i = startx;
            j = starty;
            // 下面的四个循环模拟转了一圈
            // 模拟填充上行从左到右(左闭右开)
            for (j = starty; j < starty + n - offset; j++) {
                res[startx][j] = count++;
            }
            //模拟填充右列从上到下(左闭右开)
            for (i = startx; i < startx + n - offset; i++) {
                res[i][j] = count++;
            }
            // 模拟填充下行从右到左(左闭右开)
            for (; j > starty; j--) {
                res[i][j] = count++;
            }
            //模拟填充左列从下到上(左闭右开)
            for (; i > startx; i--) {
                res[i][j] = count++;
            }
            //第二圈开始的时候，起始位置各自加1
            startx++;
            starty++;
            
            //offset 控制每一圈里每一条边遍历的长度
            offset += 2;
        }

        //如果n为奇数的话，需要单独给矩阵最中间的位置赋值
        if (n % 2) {
            res[mid][mid] = count;
        }
        return res;
    }
};
```



#### [数组总结](https://mp.weixin.qq.com/s/X7R55wSENyY62le0Fiawsg)

数组是存放在连续内存空间上相同类型数据的集合。

- 数组下标是从0开始的；
- 数组内存空间的地址是连续的；
- 数组元素是不能删除的，只能覆盖；

vector 和 array 的区别，vector 的底层实现是 array，严格来讲 vector是容器，不是数组。

```c++
// 定义一个3行4列的矩阵
int[][] rating = new int[3][4];
```

<img src="Figs/Leetcode%20%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0_3" alt="image-20210302173222353" style="zoom:67%;" />

二维数据在内存中不是 3*4 的连续地址空间，而是四条连续的地址空间组成；

[二分法](https://mp.weixin.qq.com/s?__biz=MzUxNjY5NTYxNA==&mid=2247484289&idx=1&sn=929fee0ac9f308a863a4fc4e2e44506e&scene=21#wechat_redirect)

暴力解法时间复杂度：O(n)；二分法时间复杂度：O(logn)；

利用循环不变量原则，在循环中坚持对区间的定义才能清楚把握循环中的各种细节。



[双指针法](https://mp.weixin.qq.com/s?__biz=MzUxNjY5NTYxNA==&mid=2247484304&idx=1&sn=ad2e11d171f74ad772fd23b10142e3f3&scene=21#wechat_redirect)

双指针法（快慢指针法）：通过一个快指针和慢指针在一个for循环下完成两个for循环的工作。

暴力解法时间复杂度：O(n^2)
双指针时间复杂度：O(n)

为什么数组中的元素不能删除，因为以下两点：

- 数组在内存中是连续的地址空间，不能释放单一元素，如果要释放，就是全释放（程序运行结束，回收内存栈空间）。
- C++中vector和array的区别一定要弄清楚，vector的底层实现是array，所以vector展现出友好的一些都是因为经过包装了。

双指针法（快慢指针法）在数组和链表的操作中是非常常见的，很多考察数组和链表操作的面试题，都使用双指针法。



[滑动窗口](https://mp.weixin.qq.com/s?__biz=MzUxNjY5NTYxNA==&mid=2247484315&idx=1&sn=414b885abba34abfd8d9f35c9f61b857&scene=21#wechat_redirect)

暴力解法时间复杂度：O(n^2)
滑动窗口时间复杂度：O(n)

主要要理解滑动窗口如何移动 窗口起始位置，达到动态更新窗口大小的，从而得出长度最小的符合条件的长度。

**滑动窗口的精妙之处在于根据当前子序列和大小的情况，不断调节子序列的起始位置。从而将O(n^2)的暴力解法降为O(n)。**



[模拟行为](https://mp.weixin.qq.com/s?__biz=MzUxNjY5NTYxNA==&mid=2247484331&idx=1&sn=dc41b2ba53227743f6a1b0433f9db6ef&scene=21#wechat_redirect)

模拟类的题目在数组中很常见，不涉及到什么算法，就是单纯的模拟，十分考察对代码的掌控能力。

在这道题目中，我们再一次介绍到了**「循环不变量原则」**，其实这也是写程序中的重要原则。

相信大家又遇到过这种情况：感觉题目的边界调节超多，一波接着一波的判断，找边界，踩了东墙补西墙，好不容易运行通过了，代码写的十分冗余，毫无章法，其实**「真正解决题目的代码都是简洁的，或者有原则性的」**，大家可以在这道题目中体会到这一点。



## [链表](https://mp.weixin.qq.com/s/ntlZbEdKgnFQKZkSUAOSpQ)

链表是一种通过指针串联在一起的线性结构，每一个节点是由两部分组成，一个是数据域一个是指针域（存放指向下一个节点的指针），最后一个节点的指针域指向 null （空指针）。

**链表类型**：

- 单链表：单链表中的节点只能指向节点的下一个节点；
- 双链表：每个节点有两个指针域，一个指向下一个节点，一个指向上一个节点；双链表即可以向前查询也可以向后查询；
- 循环链表：链表首尾相连；可以用来解决约瑟夫环问题；

**链表的存储方式**：

数组在内存中是连续分布的，但是链表在内存中不是连续分布的，链表通过指针域的指针链接在内存中各个节点。链表散乱分布在内存中的某些地址上，分配机制取决于操作系统的内存管理。

C/C++的定义链表节点方式：

```c++
//单链表
struct ListNode {
    int val;	//节点上存储的元素
    ListNode *next;	//指向下一个节点的指针
    ListNode(int x) : val(x), next(NULL) {}	//节点的构造函数
}；
```



C++默认生成一个构造函数，但是这个构造函数不会初始化任何成员变化，下面举例说明：

```c++
//通过自己定义构造函数初始化节点
ListNode* head = new ListNode(5);
```



```c++
//使用默认构造函数初始化节点
ListNode* head = new ListNode();
head->val = 5;
```

所以如果不定义构造函数使用默认构造函数的话，在初始化的时候就不能直接给变量赋值；



**链表的操作**：

- 删除节点

  <img src="Figs/Leetcode%20%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0_4" alt="image-20210302200746408" style="zoom:67%;" />

  更改C节点的 next指针指向下一个节点，此时D节点已经不在链表中了，C++中最好还是手动释放这个D节点内存。其他语言如 Java 和 Python 有自己的内存回收机制，就不需要手动释放。

- 添加节点

  <img src="Figs/Leetcode%20%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0_5" alt="image-20210302201024184" style="zoom:67%;" />

  链表的增添和删除都是 O(1)操作，也不是影响到其他节点。但是要删除第五个节点，需要从头节点查找到第四个节点通过next指针进行删除操作，查找的时间复杂度是O(n)。

**性能分析**：

|      | 插入/删除（时间复杂度） | 查询（时间复杂度） |             适用场景             |
| ---- | :---------------------: | :----------------: | :------------------------------: |
| 数组 |          O(n)           |        O(1)        |  数据量固定，频繁查询，较少增删  |
| 链表 |          O(1)           |        O(n)        | 数据量不固定，频繁增删，较少查询 |

数组在定义的时候，长度就是固定的，如果想改动数组的长度，就需要重新定一个新的数组。

链表的长度可以是不固定的，并且可以动态增删，适合数据量不固定，频繁增删，较少查询的场景。



#### 203.移除链表元素

使用C、C++编程，需要从内存中删除这两个移除的节点，**清理节点内存**。Java 和 Python不需要手动管理内存。

**链表操作的两种方式：**

- 直接使用原来的链表来进行操作

  - 移除头节点，需要将头节点向后移动一位；
  - 移动其他节点，通过前一个节点移除当前节点；

- 设置一个虚拟头节点再进行操作

  - 此方式下原链表中的所有节点都可以按照统一的方式进行移除；

  - 设置虚拟头节点：

    <img src="Figs/Leetcode%20%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0_6" alt="image-20210303104635072" style="zoom:67%;" />

    ```c++
    //最后return 头节点
    return dummyNode -> next;
    ```

```c++
//解法1：直接在原来的链表上进行删除
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        //删除头节点
        while (head != NULL && head->val == val) {
            ListNode* tmp = head;
            head = head->next;
            delete tmp;
        }

        //删除非头节点
        ListNode* cur = head;
        while (cur != NULL && cur->next != NULL) {
            if (cur->next->val == val) {
                ListNode* tmp = cur->next;
                cur->next = cur->next->next;
                delete tmp;
            } else {
                cur = cur->next;
            }
        }
        return head;
    }
};

//解法2：设置虚拟头节点
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        ListNode* dummyHead = new ListNode(0);  //设置一个虚拟头节点
        dummyHead->next = head; //将虚拟头节点指向Head，方便后面删除操作
        ListNode* cur = dummyHead;

        while (cur->next != NULL) {
            if (cur->next->val == val) {
                ListNode* tmp = cur->next;
                cur->next = cur->next->next;
                delete tmp;
            } else {
                cur = cur->next;
            }
        }
        return dummyHead->next;
    }
};
```



#### 707. 设计链表

```c++
//设置一个虚拟头节点的方式进行操作
class MyLinkedList {
public:

    //定义链表节点结构体
    struct LinkedNode {
        int val;
        LinkedNode* next;
        LinkedNode(int val):val(val), next(nullptr) {}
    };
    
    /** Initialize your data structure here. */
    MyLinkedList() {
        _dummyHead = new LinkedNode(0); //这里定义的头节点是一个虚拟头节点，而不是真正的链表头节点
        _size = 0;
    }
    
    /** Get the value of the index-th node in the linked list. If the index is invalid, return -1. */
    int get(int index) {
        if (index >(_size - 1) || index < 0) {
            return -1;
        }
        LinkedNode* cur = _dummyHead->next;
        while(index--) {    //如果--index就会陷入死循环
            cur = cur->next;
        }
        return cur->val;
    }
    
    /** Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list. */
    void addAtHead(int val) {
        LinkedNode* newNode = new LinkedNode(val);
        newNode->next = _dummyHead->next;
        _dummyHead->next = newNode;
        _size++;
    }
    
    /** Append a node of value val to the last element of the linked list. */
    void addAtTail(int val) {
        LinkedNode* newNode = new LinkedNode(val);
        LinkedNode* cur = _dummyHead;
        while(cur->next != nullptr) {
            cur = cur->next;
        }
        cur->next = newNode;
        _size++;
    }
    
    /** Add a node of value val before the index-th node in the linked list. If index equals to the length of linked list, the node will be appended to the end of linked list. If index is greater than the length, the node will not be inserted. */
    void addAtIndex(int index, int val) {
        if (index > _size) {
            return;
        }
        LinkedNode* newNode = new LinkedNode(val);
        LinkedNode* cur = _dummyHead;
        while(index--) {
            cur = cur->next;
        }
        newNode->next = cur->next;
        cur->next = newNode;
        _size++;
    }
    
    /** Delete the index-th node in the linked list, if the index is valid. */
    void deleteAtIndex(int index) {
        if (index >= _size || index < 0) {
            return;
        }
        LinkedNode* cur = _dummyHead;
        while(index--) {
            cur = cur->next;
        }
        LinkedNode* tmp = cur->next;
        cur->next = cur->next->next;
        delete tmp;
        _size--;
    }

    //打印链表
    void printLinkedList() {
        LinkedNode* cur = _dummyHead;
        while(cur->next != nullptr) {
            cout << cur->next->val << " ";
            cur = cur->next;
        }
        cout << endl;
    }
    private:
    int _size;
    LinkedNode* _dummyHead;
};
```



#### 206.反转链表

改变链表的next指针的指向，直接将链表反转，而不用重新定义一个新的链表。

- 首先定义一个cur指针，指向头节点，再定义一个pre指针，初始化为null；
- 反转：首先将 cur->next 节点用tmp 指针保存，也就是保存这个节点；
- 保存这个节点，是因为接下来要改变 cur->next 的指向，将 cur->next 指向 pre，此时已经反转了第一个节点；
- 接下来就是循环走如下代码逻辑，继续移动pre 和 cur指针；
- 最后 cur指针已经指向了 null，循环结束，链表也反转完毕了。此时我们 return pre指针就可以了， pre指针指向了新的头节点。

```c++
//双指针法
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* temp;
        ListNode* cur = head;
        ListNode* pre = NULL;
        while(cur) {
            temp = cur->next;   //保存 cur 的下一个节点，因为接下来要改变 cur->next
            cur->next = pre;    //节点指向翻转
            //更新pre 和 cur指针
            pre = cur;
            cur = temp;
        }
        return pre;
    }
};
```



递归法相对抽象，但是和双指针法是一样的逻辑，当 cur为空的时候循环结束，不断将cur 指向pre 的过程。

```c++
//递归法
class Solution {
public:
    ListNode* reverse(ListNode* pre, ListNode* cur) {
        if (cur == NULL) return pre;
        ListNode* temp = cur->next;
        cur->next = pre;
        //和双指针法的代码进行对比，如下递归的写法，其实就是做了这两步
        // pre = cur;
        // cur = temp;
        return reverse(cur, temp);
    }

    ListNode* reverseList(ListNode* head) {
        //和双指针法初始化是一样的逻辑
        // ListNode* cur = head;
        // ListNode* pre = NULL;
        return reverse(NULL, head);
    }
};
```



#### 142. 环形链表 II

**双指针法思路：**

考察两个知识点

- 判断链表是否为环

  使用快慢指针，分别定义 fast 和slow 指针，从头节点出发，fast指针每次移动两个节点，slow指针每次移动一个节点，如果fast指针和slow指针在途中相遇，说明这个链表有环。

  【fast指针一定先进入环中，如果fast指针和slow指针相遇的话，一定是在环中相遇。根据相对运动，fast指针是一步一步靠近slow指针】

- 如果有环，如何找到这个环的入口

  假设从头节点到环形入口节点的节点数为 x，环形入口节点到fast指针与slow指针相遇节点的节点数为 y，从相遇节点到环形入口节点的节点数为z。

  <img src="Figs/Leetcode%20%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0_7" alt="image-20210303155616644" style="zoom:67%;" />

  相遇时，slow指针走过的节点数为：x+y，fast指针走过的节点数为：x+y+n(y+z)，n为fast指针在环内走了n圈才遇到slow指针，（y+z）为一圈内节点的个数。

  由于fast指针走过的节点数 = slow指针走过的节点数 * 2 : 

  (x+y)*2 = x+y+n(y+z)	----->	x+y=n(y+z)

  因为要找到环形的入口，那么要求的是 x，因为x表示头节点到环形入口节点的距离。所以要求x，将x单独放在左边：x = (n-1)(y+z) + z ，这里n 为 ≥ 1 的整数。

  **公式意义**：假设n = 1，意味着fast指针在环形内走了一圈就遇到了slow指针，此时 x = z 。也就是说从头节点出发一个指针，从相遇节点也出发一个指针，这两个指针每次只走一个节点，那么当这两个指针相遇的时候就是环形入口的节点。在相遇节点处，定义一个指针 index1，在头节点出定义一个指针 index2。让index1 和index2 同时移动，每次移动一个节点，那么他们相遇的地方就是环形入口的节点。 n > 1时，就是fast指针在环内转了n 圈才遇到slow指针。

```c++
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode* fast = head;
        ListNode* slow = head;
        while(fast != NULL && fast->next != NULL) {
            slow = slow->next;
            fast = fast->next->next;
            
            if(slow == fast) {
                ListNode* index1 = fast;
                ListNode* index2 = head;
                while(index1 != index2) {
                    index1 = index1->next;
                    index2 = index2->next;
                }
                return index2;  //返回环的入口
            }
        }
        return NULL;
    }
};
```



**哈希表法思路：**

遍历链表中的每个节点，并将它记录下来；一旦遇到了此前遍历过的节点，就可以判定链表中存在环。

```c++
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        unordered_set<ListNode *> visited;
        while (head != nullptr) {
            if (visited.count(head)) {
                return head;
            }
            visited.insert(head);
            head = head->next;
        }
        return nullptr;
    }
};
```



#### [链表总结](https://mp.weixin.qq.com/s/vK0JjSTHfpAbs8evz5hH8A)

[**虚拟头节点**](https://mp.weixin.qq.com/s?__biz=MzUxNjY5NTYxNA==&mid=2247484132&idx=1&sn=032d3d00bdfb7179941306a2aa50c9f1&scene=21#wechat_redirect)

链表的一大问题就是操作当前节点必须要找前一个节点才能操作，使用虚拟头节点就可以解决头“节点的问题”。

[**链表的基本操作**](https://mp.weixin.qq.com/s?__biz=MzUxNjY5NTYxNA==&mid=2247484144&idx=1&sn=d2783ac63a1e93f7fc7d174308f6b400&scene=21#wechat_redirect)

[**反转链表**](https://mp.weixin.qq.com/s?__biz=MzUxNjY5NTYxNA==&mid=2247484158&idx=1&sn=60a756f681e2edeab28962c70b603ef9&scene=21#wechat_redirect)

反转链表是面试中的高频考题，两种反转的方式：迭代法和递归法（比较绕）。

[**环形链表**](https://mp.weixin.qq.com/s?__biz=MzUxNjY5NTYxNA==&mid=2247484171&idx=1&sn=72ba729f2f4b696dfc4987e232f1ad2d&scene=21#wechat_redirect)

推理中，为什么第一次在环中相遇，slow的步数是 x+y 而不是 x + 若干环的长度 + y ？

<img src="Figs/Leetcode%20%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0_8" alt="image-20210303162609381" style="zoom:67%;" />

首先slow进环的时候，fast一定是先进环来了。如果slow进环入口，fast也在环入口，那么把这个环展开成直线，就是如下图所示：

<img src="Figs/Leetcode%20%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0_9" alt="image-20210303162805940" style="zoom:67%;" />

可以看出如果slow指针和fast指针同时在环入口开始走，一定会在环入口3相遇，slow走了一圈，fast走了两圈。

slow进环的时候，fast一定是在环的任意一个位置，如图：

<img src="Figs/Leetcode%20%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0_10" alt="image-20210303163004928" style="zoom:67%;" />

fast指针走到环入口3的时候，已经走了 k + n 个节点，slow相应的应该走了 （k+n）/2个节点，因为k 是小于 n，所以（k+n）/2 一定小于 n。

也就是说slow一定没有走到环入口3，而fast已经到环入口3。这说明slow 开始走的那一环已经和fast相遇了。fast 相对于slow是一次移动一个节点，所以不可能跳过去。



## [哈希表](https://mp.weixin.qq.com/s/g8N6WmoQmsCUw3_BaWxHZA)

哈希表是根据关键码的值而直接进行访问的数据结构。数组就是一张哈希表，数组的索引下标就是关键码，通过数组下标访问数组中的元素。

哈希表的作用：哈希表都是用来快速判断一个元素是否出现在集合中。



```c++
//哈希函数 hash function
index = hashFunction(name)
hashFunction = hashCode(name) % tableSize
```



当学生数量大于哈希表的大小时，就算哈希函数的计算再均匀，也避免不了会有几位学生的名字同时映射到哈希表同一个索引下标的位置。

**哈希碰撞**

指两个值进行哈希函数映射之后，得到相同的哈希值。

**哈希碰撞解决方法**：拉链法、线性探测法

1. 拉链法

   将发生碰撞的元素存储在链表中，通过索引来寻找。

   <img src="Figs/Leetcode%20%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0_11" alt="image-20210304132206907" style="zoom:67%;" />

   拉链法就是要选择适当的哈希表，这样既不会因为数组空值而浪费大量内存，也不会因为链表太长而在查找上浪费太多时间。

2. 线性探测法

   使用此法需要保证 tableSize 大于 dataSize，我们需要依靠哈希表中的空位来解决碰撞问题。也就是在冲突的位置上放置一个信息，那么就向下寻找一个空位放置另一个人的信息。

   <img src="Figs/Leetcode%20%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0_12" alt="image-20210304132610779" style="zoom:67%;" />



**常见的三种哈希结构**

使用哈希法解决问题时，一般选择如下三种数据结构：

- 数组
- set（集合）
- map（映射）

C++中，set 和 map 分别提供了三种数据结构，如下所示

| 集合               | 底层实现 | 是否有序 | 数值是否可以重复 | 能否更改数值 | 查询效率 | 增删效率 |
| :----------------- | :------: | :------: | :--------------: | ------------ | -------- | -------- |
| std::set           |  红黑树  |   有序   |        否        | 否           | O(logn)  | O(logn)  |
| std::multiset      |  红黑树  |   有序   |        是        | 否           | O(logn)  | O(logn)  |
| std::unordered_set |  哈希表  |   无序   |        否        | 否           | O(1)     | O(1)     |

红黑树是一种平衡二叉搜索树，所以key值是有序的，但是key不可修改，改动key值会导致整棵树的错乱，所以只能删除和增加。

| 映射               | 底层实现 |  是否有序   | 数值是否可以重复 | 能否更改数值 | 查询效率 | 增删效率 |
| :----------------- | :------: | :---------: | :--------------: | ------------ | -------- | -------- |
| std::map           |  红黑树  | **key有序** |   key不可重复    | key不可修改  | O(logn)  | O(logn)  |
| std::multimap      |  红黑树  | **key有序** |    key可重复     | key不可修改  | O(logn)  | O(logn)  |
| std::unordered_map |  哈希表  |   key无序   |   key不可重复    | key不可修改  | O(1)     | O(1)     |



使用集合解决哈希问题时，优先使用 unordered_set，因为它的查询和增删效率是最优的，如果需要集合是有序的，那么就用set，如果要求不仅有序还要有重复数据的，就用 multiset。

map是一个 key value 的数据结构，map中对 key 是有限制，对 value 没有限制，因为 key 的存储方式使用红黑树实现。

虽然set、multiset 的底层实现是红黑树，不是哈希表，但是set、multiset 依然使用哈希函数来做映射，只不过底层的符号使用红黑树来存储数据，所以使用这些数据结构来解决映射问题的方法，我们仍称之为哈希法。

<img src="Figs/Leetcode%20%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0_13" alt="image-20210304134437300" style="zoom:67%;" />

总结：当需要快速判断一个元素是否出现在集合中时，就需要考虑哈希法。哈希法是牺牲了空间换取时间，因为我们需要用额外的数组、set或是map来存放数据，才能实现快速的查找。



#### 242.有效的字母异位词

解法1：暴力解法，两层for循环，同时记录字符是否重复出现，明显时间复杂度为 O(n^2)；

解法2：定义一个数组记录字符s 里字符出现的次数，数组 record 大小为26 （因为字符a 到字符z 的ASCII 就是26个连续的数值）即可，初始化为0，数组用来记录字符串s 中字符出现的次数。

将字符映射到数组也就是哈希表的索引下标上，因为字符a 到字符 z 的ASCII 是26个连续的数值，所以字符 a 映射到下标0的位置，字符z映射到下标25的位置，对于重复的字符就进行加1操作。

检查字符串t 中是否出现相同的字符时，在遍历字符串t 的 时候，对 t中出现的字符映射到哈希表索引上的数值再进行减 1的操作。

最后检查record数组中如果有的元素不为0，说明字符串s 和t 不是字母异位词，反之 return ture。时间复杂度为O(n) ，空间上因为定义的是常量大小的辅助数组空间复杂度为O(1)。

```c++
class Solution {
public:
    bool isAnagram(string s, string t) {
        int record[26] = {0};   //数组record初始化
        for (int i = 0; i < s.size(); i++) {
            record[s[i] - 'a']++;
        }
        for (int j = 0; j < t.size(); j++) {
            record[t[j] - 'a']--;
        }
        for (int n = 0; n < 26; n++) {
            if (record[n] != 0) {
                return false;
            }
        }
        return true;
    }
};
```



#### 349.两个数组的交集

本题考察一种哈希数据结构：unordered_set，此数据结构可以解决很多类似问题。

题中特别说明：输出结果中的每个元素一定是唯一的，也就是输出结果是不重复的。

上题中用数组作为哈希表是因为限制的是小写字母，数值较小，本题没有限制数值的大小，就无法使用数组来做哈希表。

如果哈希值比较少、特别分散、跨度非常大，使用数组就造成了空间的巨大浪费。

此时使用另外一种结构体set，关于set共有三种可用的数据结构：因为std:: unordered_set的底层实现是哈希表，使用unordered_set的读写效率是最高的，并不需要对数据进行排序，而且还不要让数据重复，故选择它。

<img src="Figs/Leetcode%20%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0_14" alt="image-20210305004043642" style="zoom:67%;" />

```c++
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> result_set;  //存放结果
        unordered_set<int> nums_set(nums1.begin(), nums1.end());

        for (int num : nums2) {
            //发现nums2的元素，在nums_set里出现过
            if (nums_set.find(num) != nums_set.end()) {
                result_set.insert(num);
            }
        }
        return vector<int>(result_set.begin(), result_set.end());
    }
};
```



#### 202.快乐数

题中提到无线循环，也就是 sum 过程回重复出现。

使用哈希法来判断这个sum是否重复出现，如果出现就是 return false，否则一直找到 sum为1为止。

```c++
class Solution {
public:
    // 取数值各个位上的单数之和
    int getSum (int n) {
        int sum = 0;
        while(n) {
            sum += (n % 10)*(n % 10);
            n /= 10;
        }
        return sum;
    }
    bool isHappy(int n) {
        unordered_set<int> set;
        while(1) {
            int sum = getSum(n);
            if (sum == 1) {
                return true;
            }

            if (set.find(sum) != set.end()) {
                return false;
            } else {
                set.insert(sum);
            }
            n = sum;
        }
    }
};
```



#### 1.两数之和

数组和set 来做哈希法的局限：

- 数组的大小受限制，如果元素很少，而哈希值太大会造成内存空间的浪费；
- set 是一个集合，里面放的元素只能是一个 key ，本题不仅要判断 y 是否存在而且还要记录 y 的下标位置，因此要返回 x 和 y 的下标。

map 是一种 key value 的存储结构，可以用 key 保存数值，用 value 在保存数值所在的下标。



| 映射               | 底层实现 |  是否有序   | 数值是否可以重复 | 能否更改数值 | 查询效率 | 增删效率 |
| :----------------- | :------: | :---------: | :--------------: | ------------ | -------- | -------- |
| std::map           |  红黑树  | **key有序** |   key不可重复    | key不可修改  | O(logn)  | O(logn)  |
| std::multimap      |  红黑树  | **key有序** |    key可重复     | key不可修改  | O(logn)  | O(logn)  |
| std::unordered_map |  哈希表  |   key无序   |   key不可重复    | key不可修改  | O(1)     | O(1)     |

本题不需要 key 有序，选择 std::unordered_map效率最高。

```c++
//哈希表
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        std::unordered_map <int,int> map;
        for(int i = 0; i < nums.size(); i++) {
            auto iter = map.find(target - nums[i]);
            if(iter != map.end()) {
                return {iter->second, i}; 
            }
            map.insert(pair<int, int> (nums[i], i));
            // map[nums[i]] = i;
        }
        return {};
    }
};

//解法2 暴力求解，枚举数组中每一个数，寻找是否存在 target-x。
// 每个位于 x之前的元素都已经和x匹配过，每一个元素不能被使用两次，所以我们只需要在 x 后面的元素中寻找 target - x，因此 j = i + 1。
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        for (int i = 0; i < nums.size(); i++) {
            for ( int j = i + 1; j < nums.size(); j++) {
                if (nums[i] + nums[j] == target) {
                    return{i, j};
                }
            }
        }
        return {};
    }
};
```



#### 454.四数相加 II

思路：

1. 将四个数组分为两部分，A和B为一组，C和D为另外一组；
2. 对于A和B，使用二重循环对其进行遍历，得到所有 A[i] + B[j] 的值并存入哈希映射中。对于哈希映射中的每个 key和value对，每个key表示一种 A[i] + B[j]，对应的值为A[i] + B[j] 出现的次数；
3. 对于C和D，同样使用二重循环进行遍历，当遍历到 C[k]+D[l] 时，如果 -( C[k]+D[l] ) 出现在哈希映射中，我们将  -( C[k]+D[l] ) 对应的值累加进答案。
4. 最终得到满足A[i] + B[j] + C[k]+D[l] = 0的四元组。

```c++
class Solution {
public:
    int fourSumCount(vector<int>& A, vector<int>& B, vector<int>& C, vector<int>& D) {
        unordered_map<int, int> umap;   //key为a+b的数值，value为a+b数值出现的次数
        //遍历A 和B 数组，统计两个数组元素之和以及出现的次数，放入map中
        for (int a : A) {
            for (int b : B) {
                umap[a + b]++;
            }
        }
        int count = 0;  //统计四数之和为0的元组个数
        //遍历 C和D数组，如果找到 -(c+d) 在map中出现过的话，就将map 中key 对应的value 也就是出现的次数统计出来
        for (int c : C) {
            for (int d : D) {
                if (umap.find(0 - (c + d)) != umap.end()) {
                    count += umap[(0 - (c + d))];
                }
            }
        }
        return count;
    }
};
```



#### 383.赎金信

题中只有小写字母，可以采用空间换时间的哈希策略，用一个长度为 26 的数组来记录magazine 里字母出现的次数。

然后用ransomNote 验证这个数组是否包含了ransomNote所需要的所有字母。

本题使用map的空间消耗比数组大，因为map需要维护红黑树或者哈希表，还要做哈希函数，所以数组更加简单直接。

```c++
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        int record[26] = {0};
        for (int i = 0; i < magazine.length(); i++) {
            //数组记录magazine 中各个字符出现的次数
            record[magazine[i] - 'a']++;
        }
        for (int j = 0; j < ransomNote.length(); j++) {
            //遍历ransomNote并在record 里对应的字符个数上做 -- 处理
            record[ransomNote[j] - 'a']--;
            if (record[ransomNote[j] - 'a'] < 0) {
                return false;
            }
        }
        return true;
    }
};
```



#### 15. 三数之和

双指针法：

- 关键字：不可以重复
- 模式识别：利用排序避免重复答案
- 降低复杂度变成twoSum
- 利用双指针找到所有解

```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result;
        sort(nums.begin(), nums.end());
        // 找出 a + b + c = 0
        //a = nums[i], b = nums[left], c = nums[right]
        for (int i = 0; i < nums.size(); i++) {
            //排序之后如果第一个元素已经大于零，那么无论如何组合都不能凑成三元组，直接返回结果即可
            if (nums[i] > 0) {
                return result;
            }
            // 去重
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            int left = i + 1;
            int right = nums.size() - 1;
            while (right > left) {
                if (nums[i] + nums[left] + nums[right] > 0) {
                    right--;
                } else if (nums[i] + nums[left] + nums[right] < 0) {
                    left++;
                } else {
                    result.push_back(vector<int>{nums[i], nums[left], nums[right]});
                    //去重逻辑应该放在找到一个三元组之后
                    while (right > left && nums[right] == nums[right - 1 ]) right--;
                    while (right > left && nums[left] == nums[left + 1]) left++;
                    right--;
                    left++;
                }
            }
        }
        return result;
    }
};
```



#### 18. 四数之和

基本解法：在三数之和的基础之上再套一层for 循环，四数之和的双指针解法是两层for 循环 nums[k] + nums[i] 为确定值，循环内有left 和 right 作为双指针，找到 nums[k] + nums[i] + nums[left] + nums[right] == target 的情况，三数之和时间复杂度为 O(n^2) ，四数之和时间复杂度为 O(n^3) 。

```c++
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>> result;
        sort(nums.begin(), nums.end()); //数组排序
        for (int k = 0; k < nums.size(); k++) {
            if (k > 0 && nums[k] == nums[k - 1]) {
                continue;
            }
            for (int i = k + 1; i < nums.size(); i++) {
                if (i > k + 1 && nums[i] == nums[i - 1]) {
                    continue;
                }
                int left = i + 1;
                int right = nums.size() - 1;
                while (right > left) {
                    if (nums[k] + nums[i] + nums[left] + nums[right] > target) {
                        right--;
                    } else if (nums[k] + nums[i] + nums[left] + nums[right] < target) {
                        left++;
                    } else {
                        result.push_back(vector<int>{nums[k], nums[i], nums[left], nums[right]});
                        // 找到一个四元组之后进行去重
                        while (right > left && nums[right] == nums[right - 1]) right--;
                        while (right > left && nums[left] == nums[left + 1]) left++;
                        
                        //找到正确答案后，双指针同时收缩
                        left++;
                        right--;
                    }
                }
            }
        }
        return result;
    }
};
```

 

#### 哈希表总结

哈希表一般用来判断一个元素是否出现在集合中。

哈希函数是把传入的 key 映射到符号表的索引上；哈希碰撞处理有多个 key 映射到相同索引上时的情景，处理碰撞的普通方式是拉链法和线性探测法。

三种哈希结构：数组、set、map

某些情况下map 的空间消耗比数组大一些，因为map 要维护红黑树或者符号表，而且还要做哈希函数的运算，所以数组更加简单有效。

map 是一种 <key, value> 的结构，key 用来保存数值，value 用来保存数值所在的下标，使用map 最合适。



## 字符串

#### 344. 反转字符串

双指针法。

字符串也是一种数组，元素在内存中是连续分布。对于字符串，我们定义两个指针（也可以是索引下标），一个从字符串前面，一个从字符串后面，两个指针同时向中间移动并交换元素。

```c++
// swap函数实现
// 法1：交换数值
int tmp = s[i];
s[i] = s[j];
s[j] = tmp;

//法2：通过位运算
s[i] ^= s[j];
s[j] ^= s[i];
s[i] ^= s[j];
```



```c++
class Solution {
public:
    void reverseString(vector<char>& s) {
        for (int i = 0, j = s.size() - 1; i < s.size() / 2; i++, j--) {
		swap(s[i], s[j]);
        }
    }
};
```



#### 541. 反转字符串 II

```c++
class Solution {
public:
    string reverseStr(string s, int k) {
        for (int i = 0; i < s.size(); i += 2*k) {
            // 每隔2k 个字符的前k 个字符进行反转
            // 剩余字符小于 2k 但是大于或等于 k 个，则反转前 k 个字符
            if (i + k <= s.size()) {
                reverse(s.begin() + i, s.begin() + i + k);
                continue;
            }
            // 剩余字符少于 k 个，则将剩余字符全部反转
            reverse(s.begin() + i, s.end());
            //reverse(s.begin() + i, s.begin() + s.size());
        }
        return s;
    }
};
```



#### 剑指 Offer 05. 替换空格

首先扩充数组到每个空格替换成 “ %20 ” 之后的大小，然后从后向前替换空格，也就是双指针法。

过程：i指向新长度的末尾，j指向旧长度的末尾。从后向前填充。

```c++
class Solution {
public:
    string replaceSpace(string s) {
        int count = 0;  //统计空格的个数
        int sOldSize = s.size();
        for (int i = 0; i < s.size(); i++) {
            if (s[i] == ' ') {
                count++;
            }
        }
        //扩充字符串s 的大小，也就是每个空格替换成 "%20" 之后的大小
        s.resize(s.size() + count * 2);
        int sNewSize = s.size();
        // 从后往前将空格替换为 "%20"
        for (int i = sNewSize - 1, j = sOldSize - 1; j < i; i--, j--) {
            if (s[j] != ' ') {
                s[i] = s[j];
            } else {
                s[i - 2] = '%'; 
                s[i - 1] = '2';
                s[i] = '0';
                i -= 2;
            }
        }
        return s;
    }
};
```



##### 字符串和数组之间的区别

字符串是若干字符组成的有限序列，也可理解为是一个字符数组。C语言中，把一个字符串存入一个数组中，也将结束符 ‘ \0 ’ 存入数组，并以此作为该字符串是否结束的标志。而**在C++中**提供了一个 string 类，string 类提供size 接口，可以用来判断 string 类字符串是否结束，就不用 ' \0 ' 来判断是否结束。

vector< char > 和 string 区别？

基本操作上没有区别，但是 string 提供了更多的字符串处理的相关接口，如 string 重载了 + ，而vector没有。



#### 151. 翻转字符串里的单词

解题思路：

- 移除多余的空格
  - 双指针法移除空格，resize重新设置字符串的大小；
  - 代码说明：fastIndex走得快，slowIndex走得慢，最后slowIndex就标记着移除多余空格后新字符串的长度。
- 将整个字符串反转
- 将每个单词反转

```c++
class Solution {
public:
    void removeExtraSpaces(string& s) {
        int slowIndex = 0, fastIndex = 0;   //定义快指针，慢指针
        //去掉字符串前面的空格
        while (s.size() > 0 && fastIndex < s.size() && s[fastIndex] == ' ') {
            fastIndex++;
        }
        for (; fastIndex < s.size(); fastIndex++) {
            //去掉字符串中间部分的冗余空格
            if (fastIndex - 1 > 0 && s[fastIndex - 1] == s[fastIndex] && s[fastIndex] == ' ') {
                continue;
            } else {
                s[slowIndex++] = s[fastIndex];
            }
        }
        if (slowIndex - 1 > 0 && s[slowIndex - 1] == ' ') {
            //去掉字符串末尾的空格
            s.resize(slowIndex - 1);
        } else {
            s.resize(slowIndex);
        }
    }
    //反转字符串s中左闭右闭的区间 [start, end]
    void reverse(string& s, int start, int end) {
        for (int i = start, j = end; i < j; i++, j--) {
            swap(s[i], s[j]);
        }
    }

    string reverseWords(string s) {
        removeExtraSpaces(s);   //去掉冗余空格
        reverse(s, 0, s.size() - 1);    //字符串全部反转
        int start = 0, end = 0;
        bool entry = false; //标记枚举字符串的过程是否已经进入了单词区间
        for (int i = 0; i < s.size(); i++) {
            if ((!entry) || (s[i] != ' ' && s[i - 1] == ' ')) {
                start = i;  //确定单词起始位置
                entry = true;   //进入单词区间
            }
            //单词后面有空格的情况，空格就是分词符
            if (entry && s[i] == ' ' && s[i - 1] != ' ') {
                end = i - 1;    //确定单词终止位置
                entry = false;  // 结束单词区间
                reverse(s, start, end);
            }
            //最后一个结尾单词之后没有空格的情况
            if (entry && (i == (s.size() - 1)) && s[i] != ' ') {
                end = i;    //确定单词终止位置
                entry = false;  //结束单词区间
                reverse(s, start, end);
            }
        }
        return s;
    }
};
```



#### 剑指 Offer 58 - II. 左旋转字符串

```c++
class Solution {
public:
    string reverseLeftWords(string s, int n) {
        reverse(s.begin(), s.begin() + n);
        reverse(s.begin() + n, s.end());
        reverse(s.begin(), s.end());
        return s;
    }
};
```



#### KMP算法

KMP算法主要应用于字符串匹配，KMP算法经典思想：当出现字符串不匹配时，可以记录一部分之前已经匹配的文本内容，利用这些信息避免从头再去做匹配。

next数组就是一个前缀表，前缀表是用来回溯的，它记录了模式串和文本串不匹配的时候，模式串应该从哪里开始重新匹配。



#### 28. 实现 strStr()

```c++
class Solution {
public:
    void getNext(int* next, const string& s) {
        int j = -1;
        next[0] = j;
        for (int i = 1; i < s.size(); i++) {    // i从1开始
            while (j >= 0 && s[i] != s[j + 1]) {
                j = next[j];    //向前回溯
            }
            if (s[i] == s[j + 1]) {
                j++;
            }
            next[i] = j;    //将j (前缀的长度)赋值给next[i]
        }
    }
    int strStr(string haystack, string needle) {
        if (needle.size() == 0) {
            return 0;
        }
        int next[needle.size()];
        getNext(next, needle);
        int j = -1; //next数组里记录的起始位置为1
        for (int i = 0; i < haystack.size(); i++) { //i从0开始
            while (j >= 0 && haystack[i] != needle[j + 1]) {
                j = next[j];    //j 寻找之前匹配的位置
            }
            if (haystack[i] == needle[j + 1]) { //匹配i 和 j同时向后移动
                j++;
            }
            if (j == (needle.size() - 1)) { //文本串s里出现了模式串t
                return (i - needle.size() + 1);
            }
        }
        return -1;
    }
};
```



#### 459. 重复的子字符串

如果

```c++
//说明（数组长度 - 最长相等前后缀的长度）正好可以被数组的长度整除，说明该字符串有重复的子字符串
len % (len - (next[len - 1] + 1)) == 0;
```

```c++
class Solution {
public:
    //KMP里标准构建next数组的过程
    void getNext(int* next, const string& s) {
        next[0] = -1;
        int j = -1;
        for (int i = 1; i < s.size(); i++) {
            while (j >= 0 && s[i] != s[j + 1]) {
                j = next[j];
            }
            if (s[i] == s[j + 1]) {
                j++;
            }
            next[i] = j;
        }
    }
    bool repeatedSubstringPattern(string s) {
        if (s.size() == 0) {
            return false;
        }
        int next[s.size()];
        getNext(next, s);
        int len = s.size();
        if (next[len - 1] != -1 && len % (len - (next[len - 1] + 1)) == 0) {
            return true;
        }
        return false;
    }
};
```



#### 字符串总结

字符串是若干字符组成的有限序列，可理解为一个字符数组。C语言中将一个字符串存入数组时，也将结束符 ' \0 ' 存入数组，并作为该字符串的结束标志。C++中，提供一个string类，string类会提供 size接口，可以用来判断string类字符串是否结束，就不用'\0'来判断是否结束。

1. vector< char > 和 string 区别？

   在基本操作上没有区别，但是 string提供更多的字符串处理的相关接口，例如string 重载了+，而vector却没有。所以想处理字符串，我们还是会定义一个string类型。



## [双指针总结](https://mp.weixin.qq.com/s/_p7grwjISfMh0U65uOyCjA)



## 栈和队列

1. C++中 Stack 是容器吗？
2. 我们使用的 Stack 是属于哪个版本的STL ?
3. 我们使用的 Stack 是如何实现的？
4. Stack 提供迭代器来遍历Stack 空间吗？

栈和队列是 STL （C++ 标准库）中的两个数据结构。其中三个最为普遍的STL版本：

1.  HP STL 

   其他版本的 C++ STL一般是以 HP STL为蓝本实现出来， HP STL 是 C++ STL的第一个实现版本，且开放源码；

2. P.J.Plauger STL

   由P.J.Plauger 参照HP STL实现出来的，被Visual C++编译器所采用，不是开源的。

3. SGI STL

   由Silicon Graphics Computer Systems公司参照HP STL实现，被Linux的C++编译器GCC所采用，SGI STL是开源软件，源码可读性甚高。**我们一般使用的STL也是 SGI STL版本**。



栈提供 push 和pop 接口，所有元素必须符合先进后出规则，所以栈不提供走访功能，也不提供迭代器（iterator）。不像 set 或是 map 提供迭代器 来遍历所有元素。

**栈是以底层容器完成其所有的工作，对外提供统一的接口，底层容器是可插拔的，也即我们可以控制使用哪种容器来实现栈的功能。**所以STL 中栈往往不被归类为容器，而被归类为 container adapter （容器适配器）。

栈的底层实现可以是vector, deque, list，主要就是数组和链表的底层实现。

<img src="Figs/Leetcode%20%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0_15" alt="image-20210309213635723" style="zoom:80%;" />

我们常用的 SGI STL 若没有指定底层实现的话，默认是以 deque 为缺省情况下栈的底层结构。

deque 是一个双向队列，主要封住一段，只开通另一端就可以实现栈的逻辑。

SGI STL中队列底层实现缺省情况下一样使用deque 实现。



```c++
//初始化 stack 语句，指定vector 为栈的底层实现
std::stack<int, std::vector<int> > third;	//使用vector 为底层容器的栈
```



队列先进先出的数据结构，不允许由遍历行为，不提供迭代器，SGI STL中队列一样是以 deque 为缺省情况下的底部结构。

```c++
//初始化 queue 语句，定义以list 为底层容器的队列
std::queue<int, std::list<int>> third;	//定义以list 为底层容器的队列
```

STL中队列也不被归类为容器，而是被归类为 container adapter （容器适配器）。



#### 232. 用栈实现队列

```c++
class MyQueue {
public:
    stack<int> stIn;
    stack<int> stOut;
    /** Initialize your data structure here. */
    MyQueue() {

    }
    
    /** Push element x to the back of queue. */
    void push(int x) {
        stIn.push(x);
    }
    
    /** Removes the element from in front of queue and returns that element. */
    int pop() {
        //当stOut为空时，才从stIn中导入数据
        if (stOut.empty()) {
            //从stIn中导入全部数据
            while (!stIn.empty()) {
                stOut.push(stIn.top());
                stIn.pop();
            }
        }
        int result = stOut.top();
        stOut.pop();
        return result;
    }
    
    /** Get the front element. */
    int peek() {
        int res = this->pop();  //使用已有的pop函数
        stOut.push(res);    //将刚才弹出的元素res 添加回输出栈
        return res;
    }
    
    /** Returns whether the queue is empty. */
    bool empty() {
        return stIn.empty() && stOut.empty();
    }
};
```



#### 225. 用队列实现栈

```c++
class MyStack {
public:
    queue<int> que;
    /** Initialize your data structure here. */
    MyStack() {

    }
    
    /** Push element x onto stack. */
    void push(int x) {
        que.push(x);
    }
    
    /** Removes the element on top of the stack and returns that element. */
    int pop() {
        int size = que.size();
        size--;
        while (size--) {
            que.push(que.front());
            que.pop();
        }
        int result = que.front();
        que.pop();
        return result;
    }
    
    /** Get the top element. */
    int top() {
        return que.back();
    }
    
    /** Returns whether the stack is empty. */
    bool empty() {
        return que.empty();
    }
};
```



#### 20.有效的括号

括号匹配是使用栈解决的经典问题，栈结构的特殊性适合做对称匹配类题。

本问题情况分类：

1. 括号多余；
   - 左方向括号多余
   - 右方向括号多余
2. 括号不对，也就是对应位置不匹配；

```c++
class Solution {
public:
    bool isValid(string s) {
        stack<int> st;
        for (int i = 0; i < s.size(); i++) {
            if (s[i] == '(')        st.push(')');
            else if (s[i] == '{')   st.push('}');
            else if (s[i] == '[')   st.push(']');
            //第三种情况：遍历字符串匹配的过程中，栈已经为空了
            else if (st.empty() || st.top() != s[i])    return false;
            else st.pop();  //st.pop() 与 s[i] 相等，栈弹出元素
        }
        return st.empty();
    }
};
```



#### 1047. 删除字符串中的所有相邻重复项

递归实现：每一次递归调用都将函数的局部变量、参数值和返回地址等压入调用栈中，然后递归返回的时候，从栈顶弹出上一次递归的各项参数。

```c++
class Solution {
public:
    string removeDuplicates(string S) {
        stack<char> st;
        for (char s : S) {
            if (st.empty() || s != st.top()) {
                st.push(s);
            } else {
                st.pop();   //s 与 st.pop() 相等的情况
            }
        }
        string result = "";
        while (!st.empty()) {   //将栈中元素放到result 字符串汇总
            result += st.top();
            st.pop();
        }
        reverse (result.begin(), result.end());
        return result;
    }
};
```



#### 150. 逆波兰表达式求值

逆波兰表达式相当于二叉树中的后序遍历，



#### 239. 滑动窗口最大值

本题是单调队列的经典题目，使用队列来放进窗口中的元素，窗口移动变化队列一进一出，每次移动之后队列告诉我们里面的最大值是什么。

```c++
//队列结构模型化
class MyQueue {
public:
    //窗口移动时调用que.pop(),移除元素值
    void pop (int value) {
        
    }
    //窗口移动时调用que.pop(),添加元素值
    void push (int value) {
        
    }
    //返回需要的最大值
    int front() {
        return que.front();
    }
};
```

队列里的元素一定是要排序的，而且要最大值放在出对口以便知道最大值。

队列只需维护有可能成为窗口最大值的元素就可以，同时保证队列里的元素数值是由大到小的。

设计单调队列时，pop和push需要保持如下规则：

1. pop(value)：如果窗口移除的元素value 等于单调队列的出口元素，那么队列弹出元素否则不用任何操作；
2. push(value)：如果push元素value大于入口元素，那么就将队列入口的元素弹出，直到push元素的数值小于等于队列入口的元素数值为止；

保持上述规则，每次窗口移动时，只需要问que.front() 就能返回当前窗口的最大值。

```c++
class Solution {
private:
    class MyQueue { //单调队列
    public:
        deque<int> que; //使用deque实现单调队列
        //每次弹出的时候，比较当前弹出的数值是否等于队列出口元素的数值，如果相等则弹出
        //同时Pop之前判断队列当前是否为空
        void pop(int value) {
            if (!que.empty() && value == que.front()) {
                que.pop_front();
            }
        }
        //如果push的数值大于入口元素的数值，那么就将队列后端的数值弹出，直到push的数值小于等于队列入口元素的数值为止
        //如此则保持了队列里的数值是单调从大到小的
        void push(int value) {
            while (!que.empty() && value > que.back()) {
                que.pop_back();
            }
            que.push_back(value);
        }
        //查询当前队列里的最大值，直接返回队列前端也就是front 就可以
        int front() {
            return que.front();
        }

        };
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        MyQueue que;
        vector<int> result;
        for (int i = 0; i < k; i++) {
            que.push(nums[i]);
        }
        result.push_back(que.front());  //result记录前 k 的元素的最大值
        for (int i = k; i < nums.size(); i++) {
            que.pop(nums[i - k]);   //滑动窗口移除最前面的元素
            que.push(nums[i]);  //滑动窗口加入最后的元素
            result.push_back(que.front());  //记录对应最大值
        }
        return result;
    }
};
```



#### 347. 前 K 个高频元素

1. 统计元素出现的频率
   - 使用map来进行统计
2. 对频率排序
   - 优先级队列：“披着队列外衣的堆”，优先级队列对外接口是从队头取元素，从队尾添加元素，再无其他取元素的方式，看起来就是一个队列。优先级队列内部元素是自动依照元素的权值排序。
   - 默认情况下，priority_queue 利用 max-heap（大顶堆）完成对元素的排序，这个大顶堆是以vector为表现形式的complete binary tree（完全二叉树）。
   - 求前K个高频元素，用小顶堆每次将最小的元素弹出，最后小顶堆里累计的是K个最大的元素。
3. 找出前K个高频元素

```c++
class Solution {
public:
    //小顶堆
    class myComparison {
    public:
        bool operator() (const pair<int, int> &lhs, const pair<int, int> &rhs) {
            return lhs.second > rhs.second;
        }
    };

    vector<int> topKFrequent(vector<int>& nums, int k) {
        //统计单词出现的频率
        unordered_map<int, int> map;    //map<nums[i],对应出现的次数>
        for (int i = 0; i < nums.size(); i++) {
            map[nums[i]]++;
        }
        //对频率排序
        //定义一个小顶堆，大小为K
        priority_queue<pair<int, int>, vector<pair<int, int>>, myComparison> pri_que;
        //固定大小为K的小顶堆,扫描所有频率的数值
        for (unordered_map<int, int> :: iterator it = map.begin(); it != map.end(); it++) {
            pri_que.push(*it);
            if (pri_que.size() > k) {
                //堆的大小大于K，队列弹出，保持堆的大小一直为K
                pri_que.pop();
            }
        }
        //找出前K个高频元素，因为小顶堆先弹出的是最小的，所以倒叙来输出到数组
        vector<int> result(k);
        for (int i = k - 1; i >= 0; i--) {
            result[i] = pri_que.top().first;
            pri_que.pop();
        }
        return result;
    }
};
```



## 二叉树理论

刷题时主要用到的二叉树：

- 满二叉树
- 完全二叉树

1）**满二叉树**

定义：一个二叉树只有度为 0 和度为 2 的结点，并且度为 0 的结点在同一层上。

2）**完全二叉树**

定义：除了最底层结点可能没有填满，其余每层的结点数达到最大值，并且最下面一层的结点都集中在该层最左边的若干位置。

【优先级队列其实就是一个堆，堆就是一颗完全二叉树，同时保证父子结点的顺序关系】



3）**二叉搜索树**

二叉搜索树是一个有序树：

- 若它的左子树不空，则左子树上所有结点的值均小于它的根结点的值；
- 若它的右子树不空，则右子树上所有结点的值均大于它的根结点的值；
- 它的左右子树分别为二叉排序树；



4）**平衡二叉搜索树**

定义：又称AVL树，它是一颗空树或者它的左右两个子树的高度差的绝对值不超过1，并且左右子树都是一颗平衡二叉树。

【C++中map、set、multimap、multiset的底层实现都是平衡二叉搜索树】，故map、set的增删操作时间复杂度是 log(n) ，这里没有提到unordered_map、unordered_set、unordered_map、unordered_map的底层实现是哈希表。



#### 1. 二叉树存储方式

- 链式存储——指针方式
- 顺序存储——数组方式

1）链式存储

通过指针将分布在散落各个地址的结点串联一起。

<img src="Figs/Leetcode%20%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0_16" alt="image-20210316103129465" style="zoom:60%;" />





2）顺序存储

顺序存储的元素在内存是连续分布的。

<img src="Figs/Leetcode%20%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0_17" alt="image-20210316103236555" style="zoom:60%;" />



#### 2. 二叉树遍历方式

二叉树主要有两种遍历方式：

1. 深度优先遍历：先往深走，遇到叶子结点再往回走；**主要是用递归法**；
   - 前序遍历（递归法、迭代法）：中左右
   - 中序遍历（递归法、迭代法）：左中右
   - 后序遍历（递归法、迭代法）：左右中
2. 广度优先遍历：一层一层的遍历
   - 层次遍历（迭代）



**例子：**

```c++
//链式存储二叉树结点的定义方式
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};
```



### 递归算法

递归算法构思三步曲：

1. 确定递归函数的参数和返回值

   确定哪些参数是递归的过程中需要处理的，就在递归函数中加入这个参数，并且明确每次递归的返回值是什么，以及相应的函数返回类型；

2. 确定终止条件

   运行若遇到栈溢出的错误多半是没写终止条件或者终止条件不对，操作系统也是用一个栈的结构保存每一层递归的信息，如果递归没有终止，操作系统的内存栈必然就会溢出；

3. 确定单层递归的逻辑

   确定每层递归需要处理的信息，在这里也就会重复调用自己来实现递归的过程。

   

#### 144. 二叉树的前序遍历

```c++
//递归法
class Solution {
public:
    void traversal(TreeNode* cur, vector<int>& vec) {
        if (cur == nullptr) return; //节点为空的情况
        vec.push_back(cur->val);    //中
        traversal(cur->left, vec);   //左
        traversal(cur->right, vec);    //右
    }
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> result; //存储前序遍历结果
        traversal(root, result);    //调用递归遍历函数
        return result;  //结果返回
    }
};
```



#### 94. 二叉树的中序遍历

```c++
//递归法
class Solution {
public:
    void traversal(TreeNode* cur, vector<int> &vec) {
        //递归函数终止条件
        if (cur == nullptr) return;
        //左中右的顺序遍历二叉树
        traversal(cur->left, vec);
        vec.push_back(cur->val);
        traversal(cur->right, vec);
    }
    vector<int> inorderTraversal(TreeNode* root) {
        //result 存储遍历结果
        vector<int> result;
        traversal(root, result);
        return result;
    }
};
```





#### 145. 二叉树的后序遍历

```c++
//递归法
class Solution {
public:
    void traversal(TreeNode* cur, vector<int>& vec) {
        if (cur == nullptr) return;   //递归函数终止条件
        //左、右、中的顺序
        traversal(cur->left, vec);
        traversal(cur->right, vec);
        vec.push_back(cur->val);
    }
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> result;
        traversal(root, result);
        return result;
    }
};
```



层序遍历：从左到右一层一层的去遍历二叉树，这种二叉树遍历过程中需要借助一个辅助数据结构即队列来实现，队列先进先出符合一层一层遍历的逻辑。栈先进后出适合模拟深度优先遍历也就是递归的逻辑。本题的层序遍历方式就是图论中的广度优先遍历，只不过我们将其应用在二叉树上。



#### 102.二叉树的层序遍历

```c++
class Solution {
public:
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        queue<TreeNode*> que;
        if ( root != nullptr) que.push(root);
        vector<vector<int>> result;
        while( !que.empty()) {
            int size= que.size();
            vector<int> vec;
            //固定大小的size,不要使用que.size(),因为其是不断变化的。
            for (int i = 0; i < size; i++) {
                TreeNode* node = que.front();
                que.pop();
                vec.push_back(node->val);
                if (node->left) que.push(node->left);
                if (node->right) que.push(node->right);
            }
            result.push_back(vec);
        }
        return result;
    }
};
```





#### 107. 二叉树的层序遍历 II

```c++
class Solution {
public:
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        queue<TreeNode*> que;
        if ( root != nullptr) que.push(root);
        vector<vector<int>> result;
        while( !que.empty()) {
            int size= que.size();
            vector<int> vec;
            //固定大小的size,不要使用que.size(),因为其是不断变化的。
            for (int i = 0; i < size; i++) {
                TreeNode* node = que.front();
                que.pop();
                vec.push_back(node->val);
                if (node->left) que.push(node->left);
                if (node->right) que.push(node->right);
            }
            result.push_back(vec);
        }
        reverse(result.begin(), result.end());	//反转
        return result;
    }
};
```



#### 199. 二叉树的右视图

```c++
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        queue<TreeNode*> que;
        if ( root!= nullptr) que.push(root);    //非空根子树进入队列
        vector<int> result;
        while ( !que.empty()) {
            int size = que.size();
            for (int i = 0; i < size; i++) {
                TreeNode* node = que.front();   //引用队首对象
                que.pop();
                if ( i == (size - 1)) result.push_back(node->val);  //每层最后的元素放入result数组
                if (node->left) que.push(node->left);
                if (node->right) que.push(node->right);
            }
        }
        return result;
    }
};
```



#### 637. 二叉树的层平均值

```c++
class Solution {
public:
    vector<double> averageOfLevels(TreeNode* root) {
        queue<TreeNode*> que;
        if (root != nullptr) que.push(root);
        vector<double> result;
        while (!que.empty()) {
            int size = que.size();
            double sum = 0;
            for (int i = 0; i < size; i++) {
                TreeNode* node = que.front();   //引用队首元素
                que.pop();  //对首元素出队列
                sum += node->val;
                if (node->left) que.push(node->left);
                if (node->right) que.push(node->right);
            }
            result.push_back(sum/size); //每层均值放入结果集合
        }
        return result;
    }
};
```



#### 429. N 叉树的层序遍历

```c++
class Solution {
public:
    vector<vector<int>> levelOrder(Node* root) {
        queue<Node*> que;
        if (root != nullptr) que.push(root);
        vector<vector<int>> result;
        while (!que.empty()) {
            int size = que.size();
            vector<int> vec;
            for (int i = 0; i < size; i++) {
                Node* node = que.front();
                que.pop();
                vec.push_back(node->val);
                for (int i = 0; i < node->children.size(); i++) {   //孩子节点入队列
                    if (node->children[i]) que.push(node->children[i]);
                }
            }
            result.push_back(vec);
        } 
        return result;
    }
};
```



#### 515. 在每个树行中找最大值

```c++
class Solution {
public:
    vector<int> largestValues(TreeNode* root) {
        queue<TreeNode*> que;
        if ( root!= nullptr) que.push(root);
        vector<int> result;
        while (!que.empty()) {
            int size = que.size();
            int maxValue = INT_MAX; //取每层的最大值
            for (int i = 0; i < size; i++) {
                TreeNode* node = que.front();
                que.pop();
                maxValue = node->val > maxValue ? node->val : maxValue;
                if (node->left) que.push(node->left);
                if (node->right) que.push(node->right);
            }
            result.push_back(maxValue); //将最大值放进数组
        }
        return result;
    }
};
```



#### 116. 填充每个节点的下一个右侧节点指针

```c++
class Solution {
public:
    Node* connect(Node* root) {
        if (root == nullptr)    return nullptr;
        //从根节点开始
        Node* leftmost = root;
        while (leftmost->left != nullptr) {
            //遍历这一层节点组成的链表，为下一层节点更新next指针
            Node* head = leftmost;
            while (head != nullptr) {
                // 1
                head->left->next = head->right;
                // 2
                if (head->next != nullptr) {
                    head->right->next = head->next->left;
                }
                //指针向后移动
                head = head->next;

            }
            //去下一层的最左的节点
            leftmost = leftmost->left;
        }
        return root;
    }
};
```



#### 226. 翻转二叉树



#### 101. 对称二叉树

需要遍历两棵树比较内侧和外侧节点，所以准确的来说是一个树的遍历顺序是左右中，另外一棵树的遍历顺序是右左中。

**递归法**：

1. 确定递归函数的参数和返回值

   目的：比较根节点的两个子树是否相互翻转，进而判断这个树是不是对称树，所以要比较的是两个树，递归函数参数就是左子树节点和右子树节点。返回值就是bool类型

   ```c++
   bool compare(TreeNode* left, TreeNode* right)
   ```

2. 确定终止条件

   两个节点为空的情况：

   - 左节点为空，右节点不为空，不对称，return false；
   - 左节点不为空，右节点为空，不对称 return false；
   - 左右都为空，对称，返回true；

   左右节点不为空的情况：

   - 左右都不为空，比较节点数值，不相同就 return false；

   ```cpp
   if (left == nullptr && right != nullptr)	return false;
   else if (left != nullptr && right == nullptr)	return false;
   else if (left == nullptr && right == nullptr)	return true;
   else if (left->val != right->val) return false;	//注意这里不能使用else
   ```

   上面情况都排除之后，剩下就是左右节点不为空，且数值相同的情况。

3. 确定单层递归逻辑

   处理左右节点不为空且数值相同的情况；

   - 比较二叉树外侧是否对称，传入的是左节点的左孩子，右节点的右孩子；
   - 比较内侧是否对称，传入左节点的右孩子，右节点的左孩子；
   - 如果左右都对称就返回true，有一侧不对称就返回false；

   ```c++
   bool outside = compare(left->left, right->right);
   bool inside = compare(left->right, right->left);
   bool isSame = outside && inside;
   return isSame;
   ```

   

```c++
class Solution {
public:
    bool compareSymTree (TreeNode* left, TreeNode* right) {
        //存在空节点情况下的判定
        if (left == nullptr && right == nullptr) return true;
        else if (left == nullptr && right != nullptr) return false;
        else if (left != nullptr && right == nullptr) return false;
        //左右节点数值相等情况
        else if (left->val == right->val)   return true;
        //递归进入下一层进行判断
        else {
            bool isSame = compareSymTree(left->right, right->left) && compareSymTree(left->left, right->right);
            return isSame;
        }
    }

    bool isSymmetric(TreeNode* root) {
        if (root == nullptr) return true;
        else return compareSymTree(root->left, root->right);

    }
};
```



```c++
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        int m = matrix.size();
        int n = matrix[0].size();
        int flag0_row = 0, flag0_col = 0;   //标记变量定义
        //标记变量记录第1行是否有0
        for (int j = 0; j < n; ++j) {
            if (!matrix[0][j])
            flag0_row = true;
        }
        //标记变量记录第1列是否有0
        for (int i = 0; i < m; ++i) {
            if (!matrix[i]][0])
            flag0_col = true;
        }
        //第1行第1列作为标记数组
        for (int i = 1; i < m; ++i) {
            for (int j = 1; j < n; ++j) {
                if (!matrix[i][j]) {
                    matrix[i][0] = matrix[0][j] = 0;
                }
            }
        }
        //第1行以及第1列更新其他列
        for (int i = 1; i < m; ++i) {
            for (int j = 1; j < n; ++j) {
                if (!matrix[i][0] || !matrix[0][j]) {
                    matrix[i][j] = 0;
                }
            }
        }
        //标记变量更新第1行
        if (flag0_row) {
            for (int j = 0; j < n; ++j) {
                matrix[0][j] = 0;
            }
        }
        //标记变量更新第1列
        if (flag0_col) {
            for (int i = 0; i < m; ++i) {
                matrix[i][0] = 0;
            }
        }

    }
};
```



#### 110. 平衡二叉树

一颗二叉树是平衡二叉树当且仅当其所有子树也是平衡二叉树，因此使用递归法，递归的顺序可以是自顶向下或自底向上。

求深度：从上到下去查，需要前序遍历（中左右）；

求高度：从下到上去查，需要后序遍历（左右中）；

```c++
class Solution {
public:
    //返回以该节点为根节点的二叉树的高度，如果不是二叉搜索树则返回-1
    int getDepth (TreeNode* node) {
        if (node == nullptr) return 0;
        int leftDepth = getDepth(node->left);
        if (leftDepth == -1) return -1; //说明左子树已经不是二叉平衡树
        int rightDepth = getDepth(node->right);
        if (rightDepth == -1) return -1;    //说明右子树已经不是二叉平衡树4
        return abs(leftDepth - rightDepth) > 1 ? -1 : 1 + max(leftDepth, rightDepth);
    }

    bool isBalanced(TreeNode* root) {
        return getDepth(root) == -1 ? false : true;
    }
};
```

