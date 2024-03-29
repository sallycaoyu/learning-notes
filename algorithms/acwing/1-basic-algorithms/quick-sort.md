# 快速排序
## 中心思想
- 分治
<br></br>

## 条件
- 已知有一个长度为n的数组q，一个整数l代表q的最左端位置q[0]， 另一个整数r代表q的最右端位置q[n-1]
<br></br>

## 步骤

1. 确定分界点x (四选一)
    - 左侧：q[l]
    - 中间：q[(l+r)/2]
    - 右侧：q[r]
    - 随机位置
2. 调整区间
    - x左侧的元素<=x
    - x右侧的元素>=x
3. 递归处理左右两段
<br></br>

## 解决方案
### - 暴力解题法
1. 建空数组a和b
2. 遍历数组q
    - 如果元素<=x，将它插入到a数组里
    - 如果元素>x，将它插入到b数组里
3. 将a中的元素，x，和b中的元素分别放回到q里
4. 递归处理x左右两段
<br></br>

### - 双指针法
1. 设一个指针i指向q[0]，j指向q[n-1]
2. 从左侧遍历元素，只要i指向的元素<x，i就右移，直到遇到一个>=x的元素为止
3. 从右侧遍历元素，只要j指向的元素>x，j就左移，直到遇到一个<=x的元素为止
4. 如果i在的位置<j在的位置，交换i和j指向的元素
5. 如果i<j，循环2-4
6. 递归处理j及j左侧，和j右侧

## 重点/难点
1. 分界点与边界
    - 当分界点取l时，递归左右两段不可以用i取[l, i-1], [i, r]， 例如数组为[0,1], 分界点l=0，左右两边分别为左：空集，右：[0,1]，会无限递归
    - 同理，当分界点取r时，递归左右两段不可以用j取[l, j], [j + 1, r]，否则也会发生无限递归问题

2. 左半边<=x，右半边>=x；左半边右边界是j，右半边左边界是i；左半边q[l]至q[j]，右半边q[j+1]至q[r]

3. 快排一次递归后i与j的相对位置
    - 可能为i = j
    - 可能为i = j + 1

4. 为什么分界点会把区间分为n/2？
    - 选分界点一共有n种选择，左半边长度分别为1,2,3...,n
    - 所以左半边平均长度 = (1 + 2 + 3 + ... + n) / n = (n+1)*n/(2n) = (n+1)/2，约等于n/2

5. 快排里跳出递归的条件不能为l==r，必须l>=r，因为快排的区间里可以为空

<br></br><br></br>

# Quick Sort

## Key Idea
- Divide and Conquer


## Condition

Now we have:
- an array q with size n

- an integer l representing the leftmost position of q, which is q[0]

- another integer r representing the rightmost position of q, which is q[n-1]


## Steps

1. Determine a pivot x from one of the following ways
    - leftmost: q[l]
    - midpoint: q[(l+r)/2]
    - rightmost: q[r]
    - a random position on q
<br></br>
2. Adjust the left and right sides, so that:
    - elements on the left side of x are <= x
    - elements on the right side of x are >= x
<br></br>
3. Recursively repeat the above steps for the left and right sides
<br></br>

## Solutions
### - Brute force solution
1. Initialize new arrays a and b
2. Iterate elements in q:
    - if element <= x, put it in a
    - if element > x, put it in b
3. Put elements in a, x, and elements in b back to q respectively
4. Recursively repeat the above steps for the left and right sides
<br></br>

### - Two pointers, in-place solution
1. Set a pointer i pointing at q[0], a pointer j pointing at q[n-1]
2. Iterate from the left, as long as the element i points at < x, move i one step to the right
3. Iterate from the left, as long as the element j points at > x, move j
one step to the left
4. If index of i < index of j, swap q[i] with q[j]
5. Iterate step 2-4 as long as index of i < index of j
6. Recursively repeat the steps for indices <= j and indices > j
<br></br>


# Code for two pointers, in-place solution 双指针解法代码
    # C++
    # include <iostream>

    const int N = 1e6 + 10;

    int n;
    int q[N];

    void quick_sort(int q[], int l, int r)
    {
        if (l >= r) return;
        
        int i = l - 1, j = r + 1, x = q[l + r >> 1];
        
        while (i < j)
        {
            do (i++); while (q[i] < x);
            do (j--); while (q[j] > x);
            if (i < j) std::swap(q[i], q[j]);
        }
        
        quick_sort(q, l, j), quick_sort(q, j + 1, r); // when x != q[r]
        
        // Or do this when x != q[l]:
        // quick_sort(q, l, i - 1), quick_sort(q, i, r);
        
        // Edge case e.g. q = [1, 2]
    }

    int main()
    {
        scanf("%d", &n);
        
        for (int i = 0; i < n; i++)
        {
            scanf("%d", &q[i]);
        }
        
        quick_sort(q, 0, n - 1);
        
        for (int j = 0; j < n; j++)
        {
            printf("%d ", q[j]);
        }
        
        return 0;
    }

作者：yxc<br/>
链接：https://www.acwing.com/blog/content/277/<br/>
来源：AcWing<br/>
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。