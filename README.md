# Sort-an-Array
Sort an Array
### Problem
**Example 1**:  
**Input**: [5, 2, 3, 1]  
**Output**: [1, 2, 3, 5]  

**Example 2**:  
**Input**: [5, 1, 1, 2, 0, 0]  
**Output**: [0, 0, 1, 1, 2, 5]  

**Note**:
1. `1 <= A.length <= 10000`
2. `-50000 <= A[i] <= 50000`  

### Solution 1
```c++
// Insertion sort
// TLE on LeetCode
class Solution {
public:
    vector<int> sortArray(vector<int>& nums) {
        if (nums.size() <= 1)
            return nums;

        for (int j = 1; j < nums.size(); ++j) {
            int temp = nums[j];
            int i = j - 1;
            while (i >= 0 && nums[i] > temp) {
                nums[i+1] = nums[i];
                i--;
            }
            nums[i+1] = temp;
        }

        return nums;
    }
}
```

*Time complexity:*  
![](square.png)  
*Space complexity:*  
![](constant.png)  
*Analysis:*  
Two nested loop. No additional array and no recursive function call.

**Idea**  
The empty array or an array with only one element is always sorted.  
Then iterate the array from index 1 to the end of the array, to make sure that after each loop, the size of the sorted array[0..j] is incremented by 1. 

### Solution 2
```c++
// Merge sort
class Solution {
private:
    vector<int> mergeSort(vector<int>& nums) {
        if (nums.size() <= 1)
            return nums;
        int mid = nums.size() / 2;
        vector<int> left(nums.begin(), nums.begin() + mid);
        vector<int> right(nums.begin() + mid, nums.end());
        left = mergeSort(left);
        right = mergeSort(right);
        return merge(left, right);
    }
    
    vector<int> merge(vector<int>& left, vector<int>& right) {
        int leftSize = left.size(), rightSize = right.size();
        vector<int> spare(leftSize + rightSize);
        int index = 0, leftIndex = 0, rightIndex = 0;
        while (leftIndex < leftSize && rightIndex < rightSize) {
            if (left[leftIndex] <= right[rightIndex]) {
                spare[index++] = left[leftIndex++];
            }
            else {
                spare[index++] = right[rightIndex++];
            }
        }
        
        while (leftIndex < leftSize) {
            spare[index++] = left[leftIndex++];
        }
        
        while (rightIndex < rightSize) {
            spare[index++] = right[rightIndex++];
        }
        return spare;
    }
    
public:
    vector<int> sortArray(vector<int>& nums) {
        return mergeSort(nums);
    }
};
```

*Time complexity:*  
![](loglinear.png)  
*Space complexity:*  
![](linear.png)  
*Analysis:*
Divide and conquer. Need an additional spare array to store the elements. 
(Recursive call has a space complexity of O(lg n), which is less than O(n).)

**Idea**  
Divide and conquer.

### Solution 3
```c++
// Quick sort
class Solution {
private:
    void quickSort(vector<int>& nums, int p, int r) {
        if (p < r) {
            int q = partition(nums, p, r);
            quickSort(nums, p, q - 1);
            quickSort(nums, q + 1, r);
        }
    }
    
    int partition(vector<int>& nums, int p, int r) {
        int pivot = nums[r];
        int i = p;
        for (int j = p; j < r; ++j) {
            if (nums[j] < pivot) {
                int temp = nums[i];
                nums[i++] = nums[j];
                nums[j] = temp;
            }
        }
        int temp = nums[i];
        nums[i] = nums[r];
        nums[r] = temp;
        return i;
    }
    
public:
    vector<int> sortArray(vector<int>& nums) {
        quickSort(nums, 0, nums.size() - 1);
        return nums;
    }
};
```

*Time complexity:*  
![](loglinear.png)  
*Space complexity:*  
![](constant.png)

**Idea** 
Divide and conquer.
