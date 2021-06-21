# 算法

## 1 贪心算法

贪心算法采用贪心的策略，保证**每次操作都是局部最优的**，从而使最后得到的结果是**全局最优**的。

- 回溯算法：需要记录每一个步骤 / 选择，用于回答所有具体解的问题；
- 动态规划：需要记录每一个步骤、所有选择的汇总值（最大、最小或者计数）；
- 贪心算法：每一个步骤只有一种选择，一般只需要记录与当前步骤相关的变量的值。



**动态规划需要考虑原问题的所有子问题；**

<div align="center"> <img src="Figs/LeetCode101_1" width="600"/> </div><br>

**贪心算法每次只需要考虑一个子问题，并且通常是自底向上求解；**



<div align="center"> <img src="Figs/LeetCode101_2" width="600"/> </div><br>



### 1.1 分配问题

#### [455. 分发饼干](https://leetcode-cn.com/problems/assign-cookies/)

**思路：**

1. 给一个孩子的饼干应当尽量小并且又能满足该孩子，这样大饼干才能拿来给满足度比较大的孩子。
2. 因为满足度最小的孩子最容易得到满足，所以先满足满足度最小的孩子。

具体实现时，由于我们需要获得大小关系，所以我们需要将两个数组分别排序。

```cpp
// 优先满足胃口最小的孩子
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        sort(g.begin(), g.end());
        sort(s.begin(), s.end());
        // 从最小的饼干开始，遍历每块饼干
        int res = 0;
        for (int i = 0, j = 0; i < g.size() && j < s.size(); ++j) {
            if (s[j] >= g[i]) {
                ++i;
                ++res; 
            }
        }
        return res;
    }
};
// 优先满足胃口大的孩子
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        if (s.empty())  return 0;
        sort(g.begin(), g.end());
        sort(s.begin(), s.end());
        int j = s.size() - 1, res = 0;
        for (int i = g.size() - 1; i >= 0 && j >= 0; --i) {
            if (s[j] >= g[i]) {
                ++res;
                --j;
            }
        }
        return res;
    }
};
```



#### [135. 分发糖果](https://leetcode-cn.com/problems/candy/)

贪心算法思路（两次遍历）：

- 首先，初始化所有孩子的糖果数为 1；

- 从左往右遍历一遍，如果右边孩子的评分比左边的高，则右边孩子的糖果数更新为左边孩子的糖果数加 1；
- 从右往左遍历一遍，如果左边孩子的评分比右边的高，且左边孩子当前的糖果数不大于右边孩子的糖果数，则左边孩子的糖果数更新为右边孩子的糖果数加 1。

贪心策略：在每次遍历中，只考虑并更新相邻一侧的大小关系。

```cpp
class Solution {
public:
    int candy(vector<int>& ratings) {
        int n = ratings.size();     // 学生总数为 n 
        if (n < 2)  return n;
        vector<int> nums(n, 1);     // 初始化：给所有学生 1 颗糖
        for (int i = 1; i < n; ++i) {    // 从左往右遍历
            if (ratings[i - 1] < ratings[i]) {
                nums[i] = nums[i - 1] + 1;
            }
        }
        for (int j = n - 2; j >= 0; --j) {  // 从右往左遍历
            if (ratings[j] > ratings[j + 1] && nums[j] <= nums[j + 1]) {
                nums[j] = nums[j + 1] + 1;
            }
        }
        return accumulate(nums.begin(), nums.end(), 0);
    }
};
// 改进：二次遍历时
//      for (int j = n - 2; j >= 0; --j) {  // 从右往左遍历
//          if (ratings[j] > ratings[j + 1]) {
//              nums[j] = max(nums[j], nums[j + 1] + 1);
//          }
//      }
```

### 1.2 区间问题

#### [435. 无重叠区间](https://leetcode-cn.com/problems/non-overlapping-intervals/)

求最少的移除区间个数，等价于尽量多保留不重叠的区间。在选择要保留区间时，区间的结尾十分重要：选择的区间结尾越小，余留给其它区间的空间就越大，就越能保留更多的区间。贪心策略：优先保留结尾小且不相交的区间。

算法实现：

- 先把区间按照结尾的大小进行增序排序，每次选择结尾最小且和前一个选择的区间不重叠的区间。我们这里使用 C++ 的 Lambda，结合 std::sort() 函数进行自定义排序。
- 在样例中，排序后的数组为 [[1,2], [1,3], [2,4]]。按照我们的贪心策略，首先初始化为区间 [1,2]；由于 [1,3] 与 [1,2] 相交，我们跳过该区间；由于 [2,4] 与 [1,2] 不相交，我们将其保留。因此最终保留的区间为 [[1,2], [2,4]]。

```cpp
class Solution {
public:
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        int n = intervals.size();
        if (n == 0)     return 0;
        sort(intervals.begin(), intervals.end(), [](vector<int>& a, vector<int>& b) {
            return a[1] < b[1];
        });
        int removed = 0, prev = intervals[0][1];
        for (int i = 1; i < n; ++i) {
            if (intervals[i][0] < prev) {
                ++removed;
            } else {
                prev = intervals[i][1];
            }
        }
        return removed;
    }
};
```

:zap: 注意：根据实际情况判断按区间开头排序还是按区间结尾排序。

### 1.3 自练题

#### [605. 种花问题](https://leetcode-cn.com/problems/can-place-flowers/)

**"跳格子" 思路**：

- 当遍历到 $flowerbed\ [index]=1$，说明这个位置已经有花，那必然要等到 $index + 2$ 的位置才能种花；
- 当遍历到 $flowerbed\ [index]=0$，由于每次遍历到 1 都是跳两格，因此前一格必然是 0 ，此时需要判断下一格是不是 1 即可的出 $index$ 这一格能不能种花。
  - 可以种花，则令 $n$ 减1，且连跳两格；
  - 不能种花，则连跳三格（因为 $index + 1$ 位置是 1） 

```cpp
class Solution {
public:
    bool canPlaceFlowers(vector<int>& flowerbed, int n) {
        int size = flowerbed.size();
        for (int i = 0; i < size; ) {
            if (flowerbed[i] == 1) {
                i += 2;
            } else if (i == size - 1 || flowerbed[i + 1] == 0) {
                --n;
                i += 2;
            } else {
                i += 3;
            }
        }
        return n <= 0;
    }
};
```

**算法抽象**："花坛"抽象为数组，“种花”抽象为数组相应下标处的元素设置为1，"没种花"抽象为对应下标处的元素为0；

**贪心策略**：根据如下规则，计算数组中可以设置为1的元素个数，判断是否大于或等于n ；

- 两个1之间插入1；

  假设数组的下标 $i$ 和 $j$ 处为1，且下标在 $[i + 1,j - 1 ]$ 范围内都是 0，则只有当 $j - i ≥ 4$ 时才能在下标 $i$ 和 下标 $j$ 之间设置更多的1，且可以设置1 的下标范围为 $[i + 2,j - 2]$，令 $p = j - i -3$，最多可以设置 $p/2$ （$p$为偶数时），$(p+1)/2$个（$p$为奇数时）1。因此，无论 $p$ 是奇数还是偶数，最多可以在该范围内**设置 $(p + 1)/2=(j-i-2)/2$ 个1。**

- 两个1的两侧插入1；

  $[0,1,..,l,...,r,..,m-1]$，假设数组下标 $l$ 处是最左边1，数组下标 $r$ 处是最右边的1，且对于任意的 $k<l或k>r$  都有 $flowerbed[k]=0$ ，计算在下标 $l$ 的左边以及下标 $r$ 的右边最多可以设置多少1。

  下标 $l$ 左边有 $l$ 个位置，当 $l<2$ 时无法在下标 $l$ 左边设置1，当 $l≥2$ 时可以在下标范围 $[0,l-2]$ 内设置1，可以设置1的位置个数为 $l-1$，最多可以**设置 $l/2$ 个1**；

  令 $m$ 为数组的长度，下标 $r$ 的右边有 $m - r -1$ 个位置，可以设置 1 的位置数为 $m-r-2$，最多可以**设置 $(m-r-1)/2$ 个1**；

- 如果数组中元素全为0，则有 $m$ 个位置可以设置1，最多可以**设置 $(m + 1)/2$ 个1**；

**算法步骤**：

- 维护 $prev$ 为上一朵已经种植的花的下标位置，初始时 $prev = -1$，表示没有遇到任何已种植的花；
- 从左往右遍历数组 $flowerbed$ ，当遇到 $flowerbed[i]=1$ 时根据 $prev$ 和 $i$ 的值计算上一个区间内可以种植的花的最多数量，然后令 $prev = i$，继续遍历数组 $flowerbed$ 剩下的元素；
- 遍历数组 $flowerbed$ 结束后，根据数组 $prev$ 和 长度 $m$ 计算最后一个区间内可以种植花的最多数量；
- 判断整个花坛内可以种植花的最多数量是否大于等于 $n$;

 ```cpp
 class Solution {
 public:
     bool canPlaceFlowers(vector<int>& flowerbed, int n) {
         int m = flowerbed.size();
         int count = 0;  // 统计花坛中可种植的花朵数目
         int prev = -1;
         for (int i = 0; i < m; ++i) {
             if (flowerbed[i] == 1) {
                 if (prev == -1) {
                     count += i / 2;     // i对应最左边的1的下标
                 } else {
                     count += (i - prev - 2) / 2;    // 两个1中间
                 }
                 if (count >= n) return true;	// 种花数达到要求
                 prev = i;
             }
         }
         if (prev < 0) {
             count += (m + 1) / 2;   // 花坛中全是0
         } else {
             count += (m - prev - 1) / 2;    // prev 对应最右边的 1下标
         }
         return count >= n;
     }
 };
 ```

#### [452. 用最少数量的箭引爆气球](https://leetcode-cn.com/problems/minimum-number-of-arrows-to-burst-balloons/)

与题目 435 十分类似，但是稍有不同

贪心策略：每次选择右边界位置最靠左的那一个，确定下一支箭，直到所有的气球都被引爆。

```cpp
class Solution {
public:
    int findMinArrowShots(vector<vector<int>>& points) {
        int n = points.size();
        if (n < 2)  return n;

        sort(points.begin(), points.end(), [](vector<int>& a, vector<int>& b) {
            return a[1] < b[1];
        });
        int count = 1;
        int prev = points[0][1];
        for (int i = 1; i < n; ++i) {
            if (points[i][0] <= prev) {		// 区间边界处也是可以引爆的
                continue;
            } 
            ++count;
            prev = points[i][1];		// 再次更新为未引爆气球中右边界最靠左的那个
        }
        return count;
    }
};
```

#### [763. 划分字母区间](https://leetcode-cn.com/problems/partition-labels/)

:bulb: 在处理数组前，统计一遍信息（如频率、个数、第一次出现位置、最后一次出现位置等）可以使题目难度大幅降低。

**贪心策略**：寻找每个片段可能的最小结束下标，保证每个片段的长度一定是符合要求的最短长度。

**算法步骤：**

- 从左到右遍历字符串，统计每个字母最后一次出现的下标位置，遍历的同时维护当前片段的开始下标 $start $ 和结束下标 $end$ ，初始时 $start=end=0$；
- 对每个访问到的字母 $c$，得到当前字母的最后一次出现的下标位置 $end_{c}$，则当前片段的结束下标一定不会小于 $end_c$，因此令 $end = max(end,end_c)$ ；
- 当访问到下标 $end$ 时，当前片段访问结束，当前片段的下标范围是 $[start,end]$，长度为 $end-start+1$，将当前片段的长度添加到返回值，然后令 $start=end + 1$，继续寻找下一个片段；
- 重复上述过程，直到遍历完字符串。

```cpp
class Solution {
public:
    vector<int> partitionLabels(string s) {
        vector<int> last(26);
        for (int i = 0; i < s.size(); ++i) {
            last[s[i] - 'a'] = i;       // 统计字符串中每个字母最后一次出现的下标位置
        }
        vector<int> res;
        int start = 0, end = 0;
        for (int j = 0; j < s.size(); ++j) {
            end = max(end, last[s[j] - 'a']);       // 寻找每个片段最小的结束下标位置
            if (j == end) {     // 子片段结束
                res.push_back(end - start + 1);
                start = end + 1;
            }
        }
        return res;
    }
};
```

#### [122. 买卖股票的最佳时机 II](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)

问题：寻找 $x$ 个不相交的区间 $(l_i,r_i]$ 使得如下的等式最大化：
$$
\sum_{i=1}^{x} a[r i]-a[l i]
$$
问题转化为：寻找 $x$ 个长度为 1 的区间 $(l_i, l _i + 1]$ 使得 $\sum_{i=1}^{x} a\left[l_{i}+1\right]-a\left[l_{i}\right]$ 价值最大化。

贪心的角度考虑我们每次选择贡献大于 0 的区间即能使得答案最大化，因此最后答案为：
$$
\text { ans }=\sum_{i=1}^{n-1} \max \{0, a[i]-a[i-1]\}
$$
**贪心策略**：只要今天的股价比昨天高就进行交易。对于【今天的股价 - 昨天的股价】，得到的结果只有 3 种可能：正数，0和负数。

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        int profit = 0;
        for (int i = 1; i < n; ++i) {
            profit += max(0, prices[i] - prices[i - 1]);
        }
        return profit;
    }
};
```

> 1. [股票问题系列通解](https://leetcode-cn.com/circle/article/qiAgHn/)
>
> 2. [股票问题详解](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/solution/tan-xin-suan-fa-by-liweiwei1419-2/)

#### [406. 根据身高重建队列](https://leetcode-cn.com/problems/queue-reconstruction-by-height/) _*

本题同时需要排序和插入操作。

#### [665. 非递减数列](https://leetcode-cn.com/problems/non-decreasing-array/) _*

本题需要思考你的贪心策略在各种情况下，是否仍然是最优解。



## 2 双指针

<u>双指针</u>主要用于遍历数组，两个指针指向不同的元素，从而协同完成任务。

若两个指针指向同一数组，遍历方向相同且不会相交，则称为<u>滑动窗口</u>（两个指针包围的区域即为当前的窗口），常用于区间搜索。

若两个指针指向同一数组，但是遍历方向相反，则可以用来进行搜索，待搜索的数组往往是排好序的。

**指针与常量**

```cpp
int x;
int * p1 = &x; 			// 指针可以被修改，值也可以被修改
const int * p2 = &x; 	// 指针可以被修改，值不可以被修改（const int）
int * const p3 = &x; 	// 指针不可以被修改（* const），值可以被修改
const int * const p4 = &x; // 指针不可以被修改，值也不可以被修改
```

**指针函数与函数指针**

```cpp
// addition是指针函数，一个返回类型是指针的函数
int* addition(int a, int b) {
	int* sum = new int(a + b);
	return sum;
}
int subtraction(int a, int b) {
	return a - b;
}
int operation(int x, int y, int (*func)(int, int)) {
	return (*func)(x,y);
}
// minus是函数指针，指向函数的指针
int (*minus)(int, int) = subtraction;
int* m = addition(1, 2);
int n = operation(3, *m, minus);
```

### 2.1 两数之和

#### [167. 两数之和 II - 输入有序数组](https://leetcode-cn.com/problems/two-sum-ii-input-array-is-sorted/)

本题中数组是已排序数组，我们采用方向相反的双指针寻找两个数。

算法思路：

- 初始化：一个指针指向数组最左侧元素（最小元素），向左遍历；另一个指针指向数组的最右侧元素（最大元素）向右遍历。
- 如果<u>左右指针指向元素之和</u> $sum$ 等于 $target$ ，$break $ 跳出循环；如果 $sum < target$ 就将左指针右移一位，使当前和增加一些；如果 $sum > target$ 就将右指针左移一位，使当前和减少一些。

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        int lhs = 0, rhs = numbers.size() - 1, sum;
        while (lhs != rhs) {
            sum = numbers[lhs] + numbers[rhs];	// 左右指针指向元素之和
            if (sum == target)      break; 
            else if (sum < target)  ++lhs; 
            else    --rhs;
        }
        return vector<int>{lhs + 1, rhs + 1};
    }
};
```



### 2.2 归并两个有序数组

#### [88. 合并两个有序数组](https://leetcode-cn.com/problems/merge-sorted-array/)

​		本题中数组已经排好顺序，因此可将**两个指针分别放在两个数组的末尾**，即 $nums1$ 的 $m - 1$ 位和 $nums2$ 的 $n - 1$ 位。每次将较大的数字复制到 $nums1$ 的后面，然后向前移动一位。因此，这里需要**第三个指针来定位 $nums1$ 的末尾**。

​		算法中，直接利用 $m$ 和 $n$ 作为两个数组的指针，并额外设置一个 $index$ 指针，起始位置为 $m+n-1$ 。每次向前移动 $m$ 或 $n$ 时，我们也需要向前移动 $index$ 。此外，如果 $nums1$ 数组已经复制完，就要继续复制 $nums2$ 数组；如果 $nums2$ 数组已经复制完毕，剩下的 $nums1$ 数组不需要改变，因为他们已经排好顺序。

:bulb: $++$ 和 $--$ 的使用技巧：a++ 和 ++a 都是将 a 加 1，但是 a++ 返回值为 a，而 ++a 返回值为 a + 1。如果只希望增加 a 的值，而不需要返回值，则推荐使用 ++a，其运行速度更快。

```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int index = m-- + n-- - 1;
        while (m >= 0 && n >= 0) {
            nums1[index--] = (nums1[m] > nums2[n] ? nums1[m--] : nums2[n--]);
        }
        while (n >= 0) {
            nums1[index--] = nums2[n--];
        }
    }
};
```



### 2.3 快慢指针

#### [142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

针对链表找环路的问题，可通过[快慢指针](https://zh.wikipedia.org/wiki/Floyd判圈算法)（Floyd 判圈法）以及Brent判圈算法解决。

算法步骤：

- 给定两个指针分别命名为 slow 和 fast，起始位置在链表的开头。每次 fast 前进两步，slow 前进一步。

- **如果 fast可以走到尽头，那么说明没有环路**；**如果 fast 可以无限走下去，那么说明一定有环路**，且一定存在一个时刻 slow 和 fast 相遇。
- <u>当 slow 和 fast 第一次相遇</u>时，我们将 fast 重新移动到链表开头，并让 slow 和 fast 每次都前进一步。<u>当 slow 和 fast 第二次相遇</u>时，相遇的节点即为环路的开始点。

```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode *fast = head, *slow = head;
        // Floyd判圈算法：判断是否存在环路
        while (fast != nullptr && fast->next != nullptr && slow != nullptr) {
            fast = fast->next->next;
            slow = slow->next;
            // 如果存在，查找环路节点
            if (fast == slow) {
                fast = head;
                while (fast != slow) {
                    fast = fast->next;
                    slow = slow->next;
                }
                return slow;
            }
        }
        return nullptr;
    }
};
```

> 数学推导——[Leetcode题解)](https://leetcode-cn.com/problems/linked-list-cycle-ii/solution/huan-xing-lian-biao-ii-by-leetcode-solution/)

<div align="center"> <img src="Figs/LeetCode101_3" width="600"/> </div><br>

假设链表中环外部分的长度为 $a$ ，slow 指针进入环后，又走了 $b$ 的距离与 fast 相遇。此时 fast 已经在环中走了 $n$ 圈，因此它走过的总距离为：
$$
a+n*(b + c)+b=a+(n + 1)*b+n*c
$$
由题设知，fast 指针走过的距离为 slow 指针走过距离的 2 倍，因此有：
$$
a+(n + 1)*b+n*c=2(a+b)\\
\Rightarrow a=c+(n-1)*(b+c)
$$
由此可得，从相遇点到入环点的距离加上 $n-1$ 圈的环长，恰好等于从链表头部到入环点的距离。

### 2.4 滑动窗口

#### [76. 最小覆盖子串](https://leetcode-cn.com/problems/minimum-window-substring/)

思路：两个指针 $l$ 和 $r$ 都是从最左端向最右端移动，且  $l$ 的位置一定在  $r$  的左边或重合。

注意本题虽然在 for 循环里出现了一个 while 循环，但是因为 while 循环负责移动 $l$ 指针，且 $l$ 只会从左到右移动一次，因此总时间复杂度仍然是 *O*(*n*)。

本题使用了<u>长度为 128 的数组来映射字符</u>，也可以用<u>哈希表</u>替代；其中 chars 表示目前每个字符缺少的数量，flag 表示每个字符是否在字符串 t 中存在。

**如何判断当前的窗口包含 t 中所有的字符？**

用一个哈希表表示 t 中所有的字符以及它们的个数，用一个哈希表动态维护窗口中所有的字符以及它们的个数，如果这个动态表中包含 t 的哈希表中的所有字符，并且对应的个数都不小于 t 的哈希表中各个字符的个数。

```cpp
class Solution {
public:
    string minWindow(string s, string t) {
        vector<int> chars(128, 0);
        vector<bool> flag(128, false);
        // 先统计T 中的字符情况
        for (int i = 0; i < t.size(); ++i) {
            flag[t[i]] = true;
            ++chars[t[i]];
        }
        // 移动滑动窗口，不断更改统计数据
        int cnt = 0, l = 0, min_l = 0, min_size = s.size() + 1;
        for (int r = 0; r < s.size(); ++r) {
            if (flag[s[r]]) {
                if (--chars[s[r]] >= 0) {
                    ++cnt;
                }
                // 若目前滑动窗口已包含T中全部字符
                // 尝试将 l 右移，在不影响结果的情况下获得最短字符串
                while (cnt == t.size()) {
                    if (r - l + 1 < min_size) {
                        min_l = l;
                        min_size = r - l + 1;
                    }
                    if (flag[s[l]] && ++chars[s[l]] > 0) {
                        --cnt;
                    }
                    ++l;
                }
            }
        }
        return min_size > s.size() ? "" : s.substr(min_l, min_size);
    }
};
```



> [LeetCode官方题解(更易懂)](https://leetcode-cn.com/problems/minimum-window-substring/solution/zui-xiao-fu-gai-zi-chuan-by-leetcode-solution/)

### 1.3 自练题

#### [633. 平方数之和](https://leetcode-cn.com/problems/sum-of-square-numbers/)

**思路**：假设 $left ≤ right$，初始化 $left = 0, right = \sqrt c$，当 $left=right$ 时查找结束，如果此时仍然没有找到整数符合要求的 $left$ 和 $right$ 则说明没有。

```cpp
class Solution {
public:
    bool judgeSquareSum(int c) {
        long left = 0, right = (int)sqrt(c);
        while (left <= right) {
            long sum = left * left + right * right;
            if (sum == c)   return true;
            else if (sum < c)   ++left;
            else --right;
        }
        return false;
    }
};
```

#### [680. 验证回文字符串 Ⅱ](https://leetcode-cn.com/problems/valid-palindrome-ii/)

Two Sum 题目的变形题之二。





#### [524. 通过删除字母匹配到字典里最长单词](https://leetcode-cn.com/problems/longest-word-in-dictionary-through-deleting/)

归并两个有序数组的变形题。





## 3 二分查找

## 4 排序算法

## 5 BFS & DFS

## 6 动态规划

## 7 分治与递归