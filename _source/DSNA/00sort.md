# 排序

## 经典排序算法

>- 测试环境： [912. 排序数组](https://leetcode.cn/problems/sort-an-array/)  

**基类**  
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







## 借助排序解决问题
- [0242-有效的字母异位词](_source/DSNA/lc0242.md)