---
title: 排序算法
date: 2018-11-02 22:28:51
categories: 算法
tags: [计算机科学]
---
# 排序算法
## 选择排序
```
void select_sort(vector<int> &a) {
    int m = a.size();
    for (int i = 0; i < m; i++) {
        int minIndex = i;
        for (int j = i + 1; j < m; j++) {
            if (a[j] < a[minIndex]) {
                minIndex = j;
            }
        }
        swap(a[i], a[minIndex]);
    }
}
```
## 插入排序
C++实现
```
void insert_sort(vector<int> &a) {
    int m = a.size();
    for (int i = 0; i < m; i++) {
        int j = i;
        while (j > 0 && a[j] < a[j - 1]) {
            swap(a[j], a[j - 1]);
            j--;
        }
    }
}
```
Python实现
```
1.	class Insert_sort:  
2.	    ''''' 
3.	    插入排序 
4.	    :type nums: List[int] 要排序的数组 
5.	    '''  
6.	  
7.	    def sort(self, nums):  
8.	        ''''' 
9.	        用swap的插入排序 
10.	        :type nums: List[int] 要排序的数组 
11.	        '''  
12.	        m = len(nums)  
13.	        for i in range(m):  
14.	            j = i  
15.	            while j > 0 and nums[j] < nums[j-1]:  
16.	                nums[j], nums[j-1] = nums[j-1], nums[j]  
17.	                j -= 1  
18.	  
19.	    def sort_2(self, nums):  
20.	        ''''' 
21.	        标准插入排序 
22.	        :type nums: List[int] 要排序的数组 
23.	        '''  
24.	        m = len(nums)  
25.	        for i in range(m):  
26.	            key = nums[i]  
27.	            j = i-1  
28.	            while j >= 0 and nums[j] > key:  
29.	                nums[j+1] = nums[j]  
30.	                j -= 1  
31.	            nums[j+1] = key  

```
## 冒泡排序
```
//冒泡排序
void bubble_sort(vector<int> &a) {
    int m = a.size();
    for (int i = m - 1; i >= 0; i--) {
        for (int j = 0; j < i; j++) {
            if (a[j] > a[j + 1]) {
                swap(a[j], a[j + 1]);
            }
        }
    }
}

//冒泡排序2
void bubble_sort2(vector<int> &a) {
    int m = a.size();
    for (int i = 0; i < m - 1; i++) {
        bool flag = true;
        for (int j = 0; j < m - 1 - i; j++) {
            if (a[j] > a[j + 1]) {
                swap(a[j], a[j + 1]);
                flag = false;
            }
        }
        if (flag) {
            break;
        }
    }
}
```
Python实现
```
class Bubble_sort:
    '''
    冒泡排序
    '''

    def sort(self, nums):
        '''
        标准版冒泡排序
        :type nums: List[int] 要排序的数组
        '''
        m = len(nums)
        for i in range(m-1, -1, -1):
            for j in range(i):
                if nums[j] > nums[j+1]:
                    nums[j], nums[j+1] = nums[j+1], nums[j]

    def sort_optimize(self, nums):
        '''
        优化版冒泡排序
        :type nums: List[int] 要排序的数组
        '''
        m = len(nums)
        for i in range(m-1, -1, -1):
            is_sorted = True
            for j in range(i):
                if nums[j] > nums[j+1]:
                    nums[j], nums[j+1] = nums[j+1], nums[j]
                    is_sorted = False
            if is_sorted:
                break
```
## 归并排序
Python实现
```
1.	class Merge_sort:  
2.	    ''''' 
3.	    归并排序 
4.	    '''  
5.	  
6.	    def sort(self, nums):  
7.	        ''''' 
8.	        : type nums: List[int] 要排序的数组 
9.	        '''  
10.	        self.merge_sort(nums, 0, len(nums)-1)  
11.	  
12.	    def merge_sort(self, nums, left, right):  
13.	        if left < right:  
14.	            mid = (left + right)//2  
15.	            self.merge_sort(nums, left, mid)  
16.	            self.merge_sort(nums, mid+1, right)  
17.	            self.merge_arr(nums, left, mid, right)  
18.	  
19.	    def merge_arr(self, nums, left, mid, right):  
20.	        m = right - left + 1  
21.	        helper_arr = [0]*m  
22.	        i, j = left, mid+1  
23.	        for k in range(m):  
24.	            if self.less(nums, i, j, mid, right):  
25.	                helper_arr[k] = nums[i]  
26.	                i += 1  
27.	            else:  
28.	                helper_arr[k] = nums[j]  
29.	                j += 1  
30.	  
31.	        for i in range(left, right+1):  
32.	            nums[i] = helper_arr[i - left]  
33.	  
34.	    def less(self, nums, i, j, mid, right):  
35.	        if j > right or (i <= mid and nums[i] <= nums[j]):  
36.	            return True  
37.	        return False  
```
## 堆排序
```
// 和c++的模板库稍微的区别是 他把_siftup和_siftdown合在一起写
void adjust_heap(vector<int> &a, int parent, int length) {
    int temp = a[parent], j = parent * 2 + 1;
    while (j < length) {
        if (j + 1 < length && a[j] < a[j + 1]) {
            j++;
        }
        if (temp > a[j]) {
            break;
        } else {
            a[parent] = a[j];
            parent = j;
            j = 2 * parent + 1;
        }
    }
    a[parent] = temp;
}

void heap_sort(vector<int> &a) {
    int m = a.size();
    for (int i = m / 2 - 1; i >= 0; i--) {
        adjust_heap(a, i, m);
    }

    for (int i = m - 1; i >= 0; i--) {
        swap(a[i], a[0]);
        adjust_heap(a, 0, i);
    }
}
```
Python实现
```
1.	class Heap_sort:  
2.	    ''''' 
3.	    堆排序 
4.	    '''  
5.	  
6.	    def sort(self, nums):  
7.	        ''''' 
8.	        :type nums: List[int] 要排序的数组 
9.	        '''  
10.	        m = len(nums)  
11.	        for i in range(m//2-1, -1, -1):  
12.	            self.adjust_heap(nums, i, m)  
13.	        for i in range(m-1, -1, -1):  
14.	            nums[i], nums[0] = nums[0], nums[i]  
15.	            self.adjust_heap(nums, 0, i)  
16.	  
17.	    def adjust_heap(self, nums, parent, length):  
18.	        ''''' 
19.	        堆调整算法 
20.	        :type nums: List[int] 要排序的数组 
21.	        :type parent: int 要调整的父节点 
22.	        :type length: int 要调整的数组长度 
23.	        '''  
24.	        orgin_parent_value, childpos = nums[parent], 2*parent+1  
25.	        while childpos < length:  
26.	            rightpos = childpos+1  
27.	            if rightpos < length and nums[rightpos] >= nums[childpos]:  
28.	                childpos = rightpos  
29.	            if nums[childpos] < orgin_parent_value:  
30.	                break  
31.	            else:  
32.	                nums[parent] = nums[childpos]  
33.	                parent = childpos  
34.	            childpos = 2*parent+1  
35.	        nums[parent] = orgin_parent_value  
```
## 快速排序
```
int Partition(vector<int> &a, int left, int right) {
    int i = left, j = right + 1;
    int x = a[left];
    while (true) {
        while (a[++i] < x && i < right);
        while (a[--j] > x);
        if (i >= j) {
            break;
        }
        swap(a[i], a[j]);
    }
    swap(a[left], a[j]);
    return j;
}
// 更好的partion写法
int partion(vector<int> &nums, int left, int right) {
    int low = left, pivot = nums[right];
    while (left < right) {
        if (nums[left] < nums[right]) {
            swap(nums[left], nums[low]);
            low++;
        }
        left++;
    }
    swap(nums[low], nums[right]);
    return low;
}
void QuickSort(vector<int> &a, int left, int right) { 
    if (left < right) {
        int position = Partition(a, left, right);
        QuickSort(a, left, position - 1);
        QuickSort(a, position + 1, right);
    }
}
```
Python实现稍有不同
```
class Quick_sort:

    def sort(self, nums):
        '''
        快速排序
        :type nums: List[int] 要排序的数组
        '''
        self.quick_sort(nums, 0, len(nums)-1)
        # print(sorted(nums))

    def quick_sort(self, nums, left, right):
        '''
        :type nums: List[int] 要排序的数组
        '''
        if left < right:
            index = self.partition(nums, left, right)
            self.quick_sort(nums, left, index-1)
            self.quick_sort(nums, index+1, right)

    def partition(self, nums, left, right):
        pivot, i, j = nums[left], left, right
        while i < j:
            while i < j and nums[j] >= pivot:
                j -= 1
            nums[i] = nums[j]
            while i < j and nums[i] <= pivot:
                i += 1
            nums[j] = nums[i]
        nums[i] = pivot
        return i
```
## shell排序
```
class shell_Solution {
  public:
    void sort(vector<int> &num) {
        int m = num.size();
        for (int gap = m / 2; gap > 0; gap /= 2) {
            for (int i = gap; i < m; i++) {
                int j = i;
                while (j - gap >= 0 && num[j] < num[j - gap]) {
                    swap(num[j], num[j - gap]);
                    j = j - gap;
                }
            }
        }
    }
};
```
Python实现
```
1.	class Shell_sort:  
2.	    ''''' 
3.	    shell排序 
4.	    :type nums: List[int] 要排序的数组 
5.	    '''  
6.	  
7.	    def sort(self, nums):  
8.	        ''''' 
9.	        :type nums: List[int] 要排序的数组 
10.	        '''  
11.	        m = len(nums)  
12.	        gap = m//2  
13.	        while gap > 0:  
14.	            for i in range(gap, m):  
15.	                j = i  
16.	                while j - gap >= 0 and nums[j] < nums[j - gap]:  
17.	                    nums[j], nums[j - gap] = nums[j - gap], nums[j]  
18.	                    j = j - gap  
19.	            gap = gap//2  
20.	  
21.	    def sort_2(self, nums):  
22.	        m = len(nums)  
23.	        gap = m//2  
24.	        while gap > 0:  
25.	            for i in range(gap, m):  
26.	                j, key = i, nums[i]  
27.	                while j - gap >= 0 and key < nums[j - gap]:  
28.	                    nums[j] = nums[j - gap]  
29.	                    j -= gap  
30.	                nums[j] = key  
31.	            gap //= 2  
```