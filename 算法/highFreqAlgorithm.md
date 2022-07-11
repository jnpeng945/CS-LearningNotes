#### [239. 滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)

题意：给定一个长度为 $n$ 的整数数组，一个长度为 $k$ 的滑动窗口 （Sliding Window），滑动窗口从数组的最左侧移动到数组的最右侧。我们只能看到滑动窗口内的 $k$ 个数字，滑动窗口每次向右移动一位。要求返回滑动窗口的最大值。

```cpp
// 输入: nums = [1,3,-1,-3,5,3,6,7], k = 3
// 输出: [3,3,5,5,6,7]
```



##### 思路1：单调队列。

易得共有 $(n - k + 1)$ 个滑动窗口，我们假设滑动窗口对应的坐标为 $[i, j]$，则 $i \in[1-k,n-k]$，$j \in [0,n-1]$。

**难点**：每次窗口滑动后，将获取窗口内的最大值的时间复杂度从 $O(k)$ 降低为 $O(1)$ 。

借鉴 [剑指 Offer 30. 包含min函数的栈](https://leetcode-cn.com/problems/bao-han-minhan-shu-de-zhan-lcof/) 的设计思路，该题使用单调栈实现了随意入栈、出栈情况下的 $O(1)$ 时间获取 “ 栈内最小值 ”。该题中出栈操作删除的是 “列表尾部的元素”，本题中窗口滑动删除的是 "列表首部的元素"。

遍历数组时，保证单调队列 deque：

1. deque 内仅包含窗口内的元素，每轮窗口滑动删除了元素 $nums[i - 1]$ ，需要将 deque 内的元素一起删除；
2. deque 内的元素非严格递减，每轮窗口滑动添加了元素 $nums[j + 1]$ ，需要将 deque 内所有 **小于** $nums[j + 1]$ 的元素删除。



**算法**：

1. 初始化：双端队列 deque，数组 $res$ 
2. 滑动窗口处理：
   - 若 $i > 0$ 且队首元素 $deque[0] = nums[i - 1]$ （待删除元素），则队列首元素出队；
   - 删除 deque 内所有 **小于** $nums[j]$ 的元素，保持队列有序性；
   - 将 $nums[j]$ 添加到 deque 尾部；
   - 若已经形成窗口 （即 $i \geq 0$），则将窗口最大值（$deque[0]$）添加到结果数组 $res$。

```cpp
vector<int> maxSlidingWindow(vector<int>& nums, int k) {
    int n = nums.size();
    if (n == 0 || k == 0) {
        return {};
    }
    deque<int> deq;
    vector<int> res(n - k + 1);      // 返回的数组，res[0], ..., res[n - k]
    for (int i = 1 - k, j = 0; j < n; ++i, ++j) {
        // i > 0 or j > k - 1表示已经形成滑动窗口
        // 若 deque 首部元素 = 窗口左侧元素 nums[i - 1]
        if (j > k - 1 && deq.front() == nums[i - 1]) {
            deq.pop_front();
        }

        // 保持 deque 有序性
        while(!deq.empty() && deq.back() < nums[j]) {
            deq.pop_back();
        }
        deq.push_back(nums[j]);

        // 记录窗口的最大值
        if (i >= 0) {
            res[i] = deq.front();
        }
    }
    return res;
}
```

**代码优化**：将 “未形成窗口” 和 “已经形成窗口” 阶段拆分到两个循环中进行实现，减少冗余的判断操作。

```cpp
vector<int> maxSlidingWindow(vector<int>& nums, int k) {
    int n = nums.size();
    if (n == 0 || k == 0) {
        return {};
    }
    deque<int> deq;
    vector<int> res(n - k + 1);      // 返回的数组，res[0], ..., res[n - k]
    // 未形成窗口
    for (int i = 0; i < k; ++i) {
        while (!deq.empty() && deq.back() < nums[i]) {
            deq.pop_back();
        }
        deq.push_back(nums[i]);
    }
    res[0] = deq.front();
    // 已经形成窗口
    for (int i = k; i < n; ++i) {
        if (deq.front() == nums[i - k]) {
            deq.pop_front();
        }
        while (!deq.empty() && deq.back() < nums[i]) {
            deq.pop_back();
        }
        deq.push_back(nums[i]);
        res[i - k + 1] = deq.front();
    }
    return res;
}
```



**补充**：双端队列 deque。

deque 是一种具有队列和栈的性质的数据结构，双端队列的元素可以从两端弹出，其限定插入和删除操作在表的两端进行。

```cpp
// 常见处理
// 队列声明
deque<int> deq;
// 元素入队列
deq.push_front(x);		// 元素插入队列头部
deq.push_back(x);		// 元素插入队列尾部
// 元素出队列
deq.pop_front();		// 队列头部删除元素
deq.pop_back();			// 队列尾部删除元素
deq.clear();			// 清空队列元素
// 判断以及大小值获取
deq.empty();
deq.size();
```



##### 思路2：优先队列。

本题要求滑动窗口中的最大值，考虑优先队列，大根堆实时维护滑动窗口中元素的最大值。

初始时，我们将数组 $nums$ 中的前 k 个元素放入优先队列中，每当我们向右移动窗口时，我们就可以将一个新的元素放入优先队列中，此时堆顶的元素就是堆中所有元素的最大值，当然这个值也可能恰好位于滑动窗口的左侧，此时就需要考虑将其从优先队列中移除。

优先队列中存储二元组 $(num,index)$ 表示元素 $num$ 在数组中的下标为 $index$ 。

```cpp
vector<int> maxSlidingWindow(vector<int>& nums, int k) {
    int n = nums.size();
    if (n == 0 || k == 0) {
        return {};
    }
    priority_queue<pair<int, int>> q;
    for (int i = 0; i < k; ++i) {
        q.emplace(nums[i], i);      // 前 k 对数压入优先队列中
    }
    vector<int> ans = {q.top().first};
    for (int i = k; i < n; ++i) {
        q.emplace(nums[i], i);      // 当前遍历到的数对压入优先队列
        while (q.top().second <= i - k) {   // 移除滑动窗口的左侧元素
            q.pop();
        }
        ans.push_back(q.top().first);
    }
    return ans;
}
```





















