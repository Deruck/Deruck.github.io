# 排序

## 经典排序算法

>- 测试环境： [912. 排序数组](https://leetcode.cn/problems/sort-an-array/)  

### 基类
```cpp
class Sort {
public:
    virtual vector<int> sort(vector<int>& nums) = 0;
};
```

### 归并排序  
递归地排序左右子序列，然后使用双指针将左右子序列按顺序合并，其中需要用到辅助数组。

```cpp
class MergeSort: Sort {
private:
    vector<int> tmp;
    void mergeSort(vector<int>& nums, int left, int right) {
        if(left >= right) {
            return;
        }
        int mid = (left + right) / 2;
        mergeSort(nums, left, mid);
        mergeSort(nums, mid + 1, right);
        merge(nums, left, right, mid);
    }
    void merge(vector<int>& nums, int left, int right, int mid) {
        int leftCur = left, rightCur = mid + 1, tmpCur = left;
        while(leftCur <= mid && rightCur <= right) {
            if(nums[leftCur] < nums[rightCur]) {
                tmp[tmpCur++] = nums[leftCur++];
            } else {
                tmp[tmpCur++] = nums[rightCur++];
            }
        }
        while(leftCur <= mid) {
            tmp[tmpCur++] = nums[leftCur++];
        }
        while(rightCur <= right) {
            tmp[tmpCur++] = nums[rightCur++];
        }
        for(tmpCur = left; tmpCur <= right; tmpCur++) {
            nums[tmpCur] = tmp[tmpCur];
        }
    }

public:
    vector<int> sort(vector<int>& nums) {
        tmp = vector<int>(nums.size(), 0);
        mergeSort(nums, 0, nums.size() - 1);
        return nums;
    }
};
```

>**复杂度**
>- 时间$O(n)$
>- 空间$O(n\log n)$

### 快速排序
[邓俊辉-快速排序](https://www.bilibili.com/video/BV1jt4y117KR?p=443&vd_source=654cbd7bdbc350a9d26ae562020586b6)

```cpp
class QuickSort: Sort {
public:
    vector<int> sort(vector<int>& nums) {
        quick_sort(nums, 0, nums.size() - 1);
        return nums;
    }

private:
    void quick_sort(vector<int>& nums, int left, int right) {
        if(left >= right) {
            return;
        }
        int pivotIdx = partition(nums, left, right);

        quick_sort(nums, left, pivotIdx - 1);
        quick_sort(nums, pivotIdx + 1, right);
    }
    
    int partition(vector<int>& nums, int left, int right) {
        swap(nums[left], nums[left + rand() % (right - left + 1)]);
        int lo = left, hi = right;
        int pivot = nums[left];
        while(lo < hi) {
            while(lo < hi && nums[hi] >= pivot) hi--;
            nums[lo] = nums[hi];
            while(lo < hi && nums[lo] <= pivot) lo++;
            nums[hi] = nums[lo];
        }
        nums[lo] = pivot;
        return lo;
    }
};
```

>**复杂度**
>- 时间$O(n\log n)$
>- 空间$O(\log n)$
### 冒泡排序
```cpp
class BubbleSort: Sort {
public:
    vector<int> sort(vector<int>& nums) {
        for (int last = nums.size() - 1; last > 0; last--) {
            for (int cur = 0; cur < last; cur++) {
                if(nums[cur] > nums[cur + 1]) {
                    swap(nums[cur], nums[cur + 1]);
                }
            }
        }
        return nums;
    }
};
```

>**复杂度**
>- 时间$O(n^2)$
>- 空间$O(1)$

## 借助排序解决问题
- [0242-有效的字母异位词](_source/DSNA/lc0242.md)