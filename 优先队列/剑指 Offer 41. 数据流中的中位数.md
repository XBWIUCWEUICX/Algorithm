### [剑指 Offer 41. 数据流中的中位数](https://leetcode.cn/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof/)

如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。

例如，

[2,3,4] 的中位数是 3

[2,3] 的中位数是 (2 + 3) / 2 = 2.5

设计一个支持以下两种操作的数据结构：

- void addNum(int num) - 从数据流中添加一个整数到数据结构中。
- double findMedian() - 返回目前所有元素的中位数。

示例 1：
```
输入：
["MedianFinder","addNum","addNum","findMedian","addNum","findMedian"]
[[],[1],[2],[],[3],[]]
输出：[null,null,null,1.50000,null,2.00000]
```

示例 2：
```
输入：
["MedianFinder","addNum","findMedian","addNum","findMedian"]
[[],[2],[],[3],[]]
输出：[null,null,2.00000,null,2.50000]
```

可以将数据流保存在一个列表中，并在添加元素时 保持数组有序 。

此方法的时间复杂度为 `O(N)` ，其中包括： 查找元素插入位置 `O(logN)` （二分查找）、向数组某位置插入元素 `O(N)` （插入位置之后的元素都需要向后移动一位）。

所以考虑借助堆来优化时间复杂度


```java
class MedianFinder {

    private PriorityQueue<Integer> a; //小顶堆
    private PriorityQueue<Integer> b; //大顶堆

    /** initialize your data structure here. */
    public MedianFinder() {
        a = new PriorityQueue<>();
        b = new PriorityQueue<>((v1, v2) -> v2 - v1);
    }

    public void addNum(int num) {
        if (a.size() == b.size()){
            b.offer(num);
            a.offer(b.poll());
        }
        else {
            a.offer(num);
            b.offer(a.poll());
        }
    }

    public double findMedian() {
        return a.size() == b.size() ? (a.peek() + b.peek()) / 2.0 : a.peek();
    }
}
```
