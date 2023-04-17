[toc]

### 1. [**Two Sum**](https://leetcode.com/problems/two-sum/description/) [Easy]

Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.  

**方法1：暴力解法** Runtime `381 ms` Beats `42.14%` Memory `10.3MB` Beats `73.74%`  
用两个嵌套的for循环，遍历所有可能得二元组`(i, j)`，找到`nums[i] + nums[j] == target`时返回结果。  
时间复杂度：**O(n<sup>2</sup>)**  
空间复杂度：**O(1)**

```C++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        int nums_len = nums.size();
        for(int i = 0;i < nums_len; i++){
            for(int j = i + 1;j < nums_len; j++){
                if(nums[i] + nums[j] == target){
                    return {i,j};
                }
            }
        }
        return {};
    }
};
```

**方法1：hash map法** Runtime `12ms` Beats `80.47%` Memory `11MB` Beats `27.81%`  
用一个无序的哈希表记录每个元素相对与`target`的值和对应的`indice`，分别作为map的`key,value`，直接查找`hash map`中是否存在对应的元素的补数，存在则返回元素的`indice`和对应补数的`map[complement].value`。  
时间复杂度：**O(n)**，只需要遍历一遍数组。  
空间复杂度：**O(n)**，需要使用一个unordered_map来存储已经遍历过的数。

```C++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> map;               //声明一个unordered_map
        for(int i = 0; i < nums.size(); i++){
            int complement = target - nums[i];     //计算每个元素的相对于target补数
            if(map.find(complement) != map.end()){ //以补数作为key值，查找是否存在于map中
                return {map[complement],i};        //存在，返回value 
            }
            map[nums[i]] = i;                      //没有找到该元素的补数，将该元素的值作为Key值，位置作为value存入map
        }
        return {};
    }
};
```

**STL关联式容器**
关联式容器与序列式容器相比较，特点在“关联”上，关联式容器通过`键对值`（包括`key`与`value`），将储存的每一个数据value与一个键值key一一对应起来，只需通过键值key就可以读写元素，在查找时效率较高。  
关联式容器分为两种，**有序（ordered）**和**无序（unordered）**，C++98中，STL只有底层用`红黑树`实现的有序关联容器包括`map`,`set`,`multimap`,`multimap`，这四个容器会将插入的元素按照特定的顺序储存，以保证红黑树的平衡，进而实现log<sub>2</sub>N的查询效率。在C++11中，STL又增加了4个`unordered`的无序关联容器，包括[unordered_map](https://cplusplus.com/reference/unordered_map/unordered_map/),`unorderes_set`, `unordered_multimap`,`unordered_multiset`，他们的底层使用`哈希表（hash map）`实现，在不发生哈希冲突(碰撞)时查询效率可达到O(1).  

**方法3：双指针法** Runtime `11ms` Beats` 83.55%` Memory `10.3MB` Beats`49.56%`  
构造一个常规数组`nums_copy`，记录`nums`的数值和每个数值的位置，将`nums`排序，利用双指针找到第一对和为`target`的数，再和`nums_copy`中的数比对，找到最初的indices.  
时间复杂度：**O(nlog<sub>2</sub>n)**，算法中排序函数`sort()`的时间复杂度  
空间复杂度：**O(n)**

```C++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> result{};
        int nums_len = nums.size();
        int nums_copy[nums_len];           //copy the nums to register the in indices;
        int head = 0;
        int tail = nums_len - 1;
        for (int i = 0; i < nums_len; i++){
            nums_copy[i] = nums[i];
        }
        sort(nums.begin(),nums.end());    //将数组排序
        while(head < tail){
            if(nums[head] + nums[tail] == target)
                break;
            else if(nums[head] + nums[tail] < target)
                head++;
            else
                tail--;
        }
        for(int i = 0; i < nums_len; i++){
            if(nums_copy[i] == nums[head])
                result.push_back(i);
            else if(nums_copy[i] == nums[tail])
                result.push_back(i);
        }
        return result;
    }
};
```

### 4. [**Median of Two Sorted Arrays**](https://leetcode.com/problems/median-of-two-sorted-arrays/) [Hard]

Given two sorted arrays nums1 and nums2 of size m and n respectively, return the median of the two sorted arrays.

The overall run time complexity should be O(log (m+n)).  

```C++
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        vector<int> nums{};
        int nums1_len = nums1.size();
        int nums2_len = nums2.size();
        int p1 = 0;
        int p2 = 0;
        while(p1 < nums1_len && p2 < nums2_len){
            if(nums1[p1] < nums2[p2]){
                nums.push_back(nums1[p1]);
                p1++;
            }
            else{
                nums.push_back(nums2[p2]);
                p2++;
            }
        }
        while(p1 < nums1_len){
            nums.push_back(nums1[p1]);
                p1++;
        }
        while(p2 < nums2_len){
            nums.push_back(nums2[p2]);
                p2++;
        }
        int n = p1 + p2;
        if(n % 2 == 0)
            return double(nums[n / 2 - 1] + nums[n / 2]) / 2;
        else
            return nums[n / 2];
    }
};
```

### 15. [**3Sum**](https://leetcode.com/problems/3sum/description/) [Medium] 

Given an integer array nums, return all the triplets [nums[i], nums[j], nums[k]] such that i != j, i != k, and j != k, and nums[i] + nums[j] + nums[k] == 0.
Notice that the solution set must not contain duplicate triplets.  
**方法1：究极暴力解法** `Time Limit Exceeded`

```C++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        int nums_len = nums.size();
        int result_num = 0;
        vector<vector<int>> result{};
        sort(nums.begin(),nums.end());
        for (int i = 0; i < nums_len; i++){
            for (int j = i + 1; j < nums_len; j++){
                for(int k = j + 1; k < nums_len; k++){
                    if(nums[i] + nums[j] + nums[k] == 0 ){
                        result.push_back({nums[i],nums[j],nums[k]});
                        result_num++;
                    }
                }
            }
        }
        for(int i = 0; i < result_num; i++){
            for(int j = 0 ; j < result_num; j++){
                if(result[i] == result[j] && i != j){
                    int k = j;
                    while(k < result_num - 1){
                        result[k] = result[k + 1];
                        k++;
                    }
                    result.pop_back();
                    result_num--;
                    j--;
                }
            }
        }
        return result;
    }
};
```

**方法2：双指针法** Runtime `274ms` Beats `66.76%` Memory `23.8MB` Beats `99.35%`

```C++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        int nums_len = nums.size();
        vector<vector<int>> res{};
        sort(nums.begin(),nums.end());
        for(int i = 0; i < nums_len; i++){
            if(i > 0 && nums[i] == nums[i-1])
                continue;
            if(num[i] > 0)
                break;
            int head = i + 1;
            int tail = nums_len - 1;
            while(head < tail){
                if(nums[i] + nums[head] + nums[tail] == 0){
                    res.push_back({nums[i], nums[head], nums[tail]});
                    head++;
                    tail--;
                    while (head < tail && nums[head] == nums[head - 1]) 
                        head++;
                    while (head < tail && nums[tail] == nums[tail + 1]) 
                        tail--;
                }
                else if(nums[head] + nums[tail] + nums[i] < 0){
                    head++;
                }
                else
                    tail--;
            }
        }
        return res;
    }
};
```

### 49. [**group Anagrams**](https://leetcode.com/problems/group-anagrams/) [Medium]

Given an array of strings strs, group the anagrams together. You can return the answer in any order.

An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.  
**方法1：** Runtime `32ms` Beats `86.33%` Memory `18.7MB` Beats `92.21%`

```C++
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        vector<vector<string>> res{};
        unordered_map<string,int> map;
        int strs_len = strs.size();
        int reg = 0;
        string strs_copy[strs_len];
        for(int i = 0; i < strs_len; i++){
            strs_copy[i] = strs[i];
        }
        for(int i = 0; i < strs_len; i++){
            sort( strs[i].begin(), strs[i].end());
        }
        for(int i = 0; i < strs_len; i++){
            if(map.find(strs[i]) == map.end()){
                map[strs[i]] = reg;
                res.push_back({strs_copy[i]});
                reg++;
            }
            else{
                res[map[strs[i]]].push_back(strs_copy[i]);
            }

        }
        return res;
    }
};
```

### 56. [**Merge Intervals**](https://leetcode.com/problems/merge-intervals/) [Medium]

Given an array of intervals where intervals[i] = [starti, endi], merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.  
**方法1：** Runtime `2853ms` Beats `5.4%` Memory `18.6MB` Beats `98.2%`

```C++
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        vector<vector<int>> res{};
        sort(intervals.begin(), intervals.end());
        for(int i = 0; i < intervals.size() - 1; i++){
            if(intervals[i][1] >= intervals[i + 1][0] && intervals[i][1] <= intervals[i + 1][1]){
                intervals[i][1] = intervals[i + 1][1];
                intervals.erase(intervals.begin() + i + 1);
                i--;
                continue;
            }
            if(intervals[i][1] >= intervals[i + 1][0] && intervals[i][1] >= intervals[i + 1][1]){
                intervals.erase(intervals.begin() + i + 1);
                i--;
                continue;
            }
        }
        return intervals;
    }
};
```

### 75. [**Sort Colors**](https://leetcode.com/problems/sort-colors/) [Medium]

Given an array nums with n objects colored red, white, or blue, sort them in-place so that objects of the same color are adjacent, with the colors in the order red, white, and blue.

We will use the integers 0, 1, and 2 to represent the color red, white, and blue, respectively.

You must solve this problem without using the library's sort function.  

**方法1：** Runtime `0ms` Beats `100%` Memory `8.4MB` Beats `31.29%`

```C++
class Solution {
public:
    void sortColors(vector<int>& nums) {
        unordered_map<int,int> map;
        for (int i = 0; i < nums.size(); i++){
            if(map.find(nums[i]) == map.end())
                map[nums[i]] = 1;
            else
                map[nums[i]]++;
        }
        for(int i = 0; i < nums.size(); i++){
            if(i < map[0])
                nums[i] = 0;
            else if (i < map[0] + map[1])
                nums[i] = 1;
            else
                nums[i] = 2;
        }
    }
};
```

### 88. [**Merge Sorted Array**](https://leetcode.com/problems/merge-sorted-array/) [Easy]

You are given two integer arrays nums1 and nums2, sorted in non-decreasing order, and two integers m and n, representing the number of elements in nums1 and nums2 respectively.

Merge nums1 and nums2 into a single array sorted in non-decreasing order.

The final sorted array should not be returned by the function, but instead be stored inside the array nums1. To accommodate this, nums1 has a length of m + n, where the first m elements denote the elements that should be merged, and the last n elements are set to 0 and should be ignored. nums2 has a length of n.  
**方法1：排序法** Runtime `3ms` Beats `64.35%`Memory `9MB` Beats `92.14%`  
先将两个数组融合，再用`sort()`对其排序。  
时间复杂度：**O(nlog<sub>2</sub>n)**  
空间复杂度：**O(1)**  

```C++
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        for(int i = 0; i < n; i++){
            nums1[i + m] = nums2[i];
        }
        sort(nums1.begin(),nums1.end());
    }
};
```

**方法1：双指针法** Runtime `0ms` Beats `100%` Memory `9MB `Beats `92.14%`  
利用两个分别指向`nums1`与`nums2`的指针比较两个数组中元素的大小，将小的加入到`nums`中，为了不破坏`nums1`中原本的值，声明一个数组`nums1_copy`记录`nums1`的值。  
时间复杂度： **O(m + n)**  
空间复杂度： **O(m)**

```C++
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int nums1_copy[nums1.size()];
        int i = 0;
        int j = 0;
        for(int i = 0; i < nums1.size(); i++)
            nums1_copy[i] = nums1[i];
        while(i < m && j < n){
            if(nums1_copy[i] < nums2[j]){
                nums1[i + j] = nums1_copy[i];
                i++;
            }
            else{
                nums1[i + j] = nums2[j];
                j++;
            }
        }
        while(i < m){
            nums1[n + i] = nums1_copy[i];
            i++;
        }
        while(j < n){
            nums1[m + j] = nums2[j];
            j++;
        }
    }
};
```

### 164. [**Maximum Gap**](https://leetcode.com/problems/maximum-gap/) [Hard]

Given an integer array nums, return the maximum difference between two successive elements in its sorted form. If the array contains less than two elements, return 0.

You must write an algorithm that runs in linear time and uses linear extra space.

```C++
class Solution {
public:
    int maximumGap(vector<int>& nums) {
        if(nums.size() < 2)
            return 0;
        sort(nums.begin(),nums.end());
        int max = nums[1] - nums[0];
        for(int i = 0; i < nums.size() - 1; i++ ){
            if(nums[i+1] - nums[i] > max)
                max = nums[i+1] - nums[i];
        }
        return max;
    }
};
```

### 167. [**Two Sum II - Input Array Is Sorted**](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/description/) [Medium]

**方法1：双指针法** Runtime `8ms` Beats `93.5%` Memory `15.6MB` Beats `81.90%`

```C++
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        int front = 0;
        int back = numbers.size() - 1;
        while(front < back){
            if(numbers[front] + numbers [back] == target)
                return {front + 1, back + 1};
            else if(numbers[front] + numbers [back] < target)
                front++;
            else
                back--;
        }
        return {};
    }
};
```

### 169. [**Majority Element**](https://leetcode.com/problems/majority-element/) [Easy]

Given an array nums of size n, return the majority element.

The majority element is the element that appears more than ⌊n / 2⌋ times. You may assume that the majority element always exists in the array.  
**方法1：双指针法** Runtime `20 ms` Beats `65.88%` Memory `19.5MB` Beats `80.16%`

```C++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int len = nums.size()-1;
        int maj = nums[0];
        int count =1;
        for(int i = 1;i < len;i++)
        {
            if(nums[i] == maj)
                count++;
            else {
                count--;
                if(count == 0)
                maj = nums[i+1];
           }
        }
        return maj;
};
```

### 215. [**Kth Largest Element in an Array**](https://leetcode.com/problems/kth-largest-element-in-an-array/) [Medium]

Given an integer array nums and an integer k, return the kth largest element in the array.

Note that it is the kth largest element in the sorted order, not the kth distinct element.

You must solve it in O(n) time complexity.
**方法1：暴力解法** Runtime `111ms` Beats `71.66%` Memory `45.6MB` Beats `74.50%`
```
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());
        return(nums[nums.size() - k]);
    }
};
```

### 217. [**Contains Duplicate**](https://leetcode.com/problems/contains-duplicate/description/) [Easy]

Given an integer array nums, return true if any value appears at least twice in the array, and return false if every element is distinct.  
**方法1：暴力解法** `Time Limit Exceeded`  
遍历整个数组，注意比较数组中各个元素是否相同：`nums[0]` 与 `nums[1]...nums[nums.len()]`比较；`nums[1]` 与 `nums[2]...nums[nums.len()]`比较；··· 。所以比较次数为`(n-1)+(n-2)+···+(2)+(1)`,故算法的时间复杂度为`n*(n-1)/2`。

```C++
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        int nums_len = nums.size();
        int i;
        int j;
        bool flag = false;
        for(i = 0; i <nums_len; i++){
            for(j = i + 1; j <nums_len; j++){
                if (nums[i] == nums[j])
                    return true;
            }
        }
        return flag;
    }
}
```

**方法2：排序法**  Runtime `123ms` Beats `91.26%` Memory `57.3MB` Beats `71.40%`  
使用`sort(nums.begin(), nums.end())`函数先对数组排序，使数组变为一个有序的数组，这是相同的数组元素必然是相邻的，只需比较相邻两个元素是否相同即可。时间复杂度取决于 `sort()`函数的时间复杂度，后续只需要遍历数组一遍即可，时间复杂度为`n`。

```C++
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        int nums_len = nums.size();
        int i;
        bool flag = false;
        sort(nums.begin(), nums.end());
        for(i = 0; i < nums_len - 1; ++i){
            if(nums[i] == nums[i+1])
                return true;
        }
        return flag;
    }
}
```

**方法3：Set** Runtime `197ms` Beats `17.40%` Memory `73.3MB` Beats `15.7%`

```C++
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        return nums.size() > set<int>(nums.begin(),nums.end()).size();
    }
};
```

**方法4：HashMap** ds

```C++
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        map<int,int> mp;
        for(auto i : nums) mp[i]++;
        bool flag = false;
        for(auto i : mp){
            if(i.second >= 2) return true;
          }
        return flag;
    }
};
```

[**std::sort**](https://cplusplus.com/reference/algorithm/sort/)
我们使用到了`sort`函数，在这里我们认识一下它:

```C++
default (1)	
template <class RandomAccessIterator>  void sort (RandomAccessIterator first, RandomAccessIterator last);
custom (2)	
template <class RandomAccessIterator, class Compare>  void sort (RandomAccessIterator first, RandomAccessIterator last, Compare comp);
```

**排序顺序**：将`[first,last)`的元素按升序排列，它是一个不稳定的排序，就是说对于相等的元素，排序后他们的相对顺序可能会发生改变。  
**参数**：`(first,last，cmp)`(首元素地址(必填), 尾元素地址的下一个地址(必填), 比较函数(非必填)),比较函数可以根据自身需要填写，如果不写比较函数，则默认对前面给出的区间进行递增排序。在STL标准容器中，只有vector、string、deque是可以使用sort的。

### 220. [**Contains Duplicate III**](https://leetcode.com/problems/contains-duplicate-iii/) [Hard]

You are given an integer array nums and two integers indexDiff and valueDiff.

Find a pair of indices (i, j) such that:

i != j,
abs(i - j) <= indexDiff.
abs(nums[i] - nums[j]) <= valueDiff, and
Return true if such pair exists or false otherwise.

```C++
class Solution {
public:
    bool containsNearbyAlmostDuplicate(vector<int>& nums, int indexDiff, int valueDiff) {
        int nums_len = nums.size();
        for (int i = 0; i < nums_len -  1; i++){
            for(int j = i + 1;j < i + indexDiff + 1; j++){
                if(j < nums_len && abs(i - j) <= indexDiff && abs(nums[i] - nums[j]) <= valueDiff)
                    return true;
            }
        }
        return false;
    }
};
```

### 268. [**Missing Number**](https://leetcode.com/problems/missing-number/) [Easy]

Given an array nums containing n distinct numbers in the range [0, n], return the only number in the range that is missing from the array.  
**方法1：求和法** Runtime `24ms` Beats `43.93%` Memory `17.9MB` Beats `69.85%`  
题目比较简单，很容易就能想到，所有元素的和与缺失的元素有一一对应关系，算出无缺元素和与数组元素和的差值，即为缺失的元素。

```C++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int nums_len = nums.size();
        int sum = ((nums_len + 1) * nums_len)/2;
        int nums_sum = 0;
        for (int i = 0; i < nums_len; i++){
            nums_sum += nums[i];
        }
        int miss_num = sum - nums_sum; 
        return miss_num;
    }
};
```

**方法2：排序法** Runtime `20ms` Beats `68.31%` Memory `18.1MB` Beats `9.92%`  

将所有元素排序后逐一对比，当存最、存在`nums[i] != i`即存在缺漏，缺漏者即为 `i`.  

```C++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int nums_len = nums.size();
        int i;
        sort(nums.begin(),nums.end());
        for(i = 0; i < nums_len; i++){
            if(nums[i] != i)
                return i;
        }
        return i;
    }
};
```  

### 347. [**Top K Frequent Elements**](https://leetcode.com/problems/top-k-frequent-elements/) [Medium]

Given an integer array nums and an integer k, return the k most frequent elements. You may return the answer in any order.
**方法1：暴力解法** Runtime `17ms` Beats `59.14%` Memory `13.5MB` Beats `97.87%`

```C++
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        vector<int> res{};
        vector<int> freq{};
        unordered_map<int, int> map;
        if(nums.size() == 1)
            return nums;
        for(int i = 0; i < nums.size(); i++){
            if(map.find(nums[i]) == map.end()){
                map[nums[i]] = 1;
            }
            else 
                map[nums[i]]++;
        }
        for(auto pair:map){
            freq.push_back(pair.second);
        }
        sort(freq.begin(), freq.end(),  greater<int>());
        for(int i = 0; i < k ; i++){
            if(i > 0 && freq[i] == freq[i - 1])
                continue;
            for(auto pair:map){
                if(pair.second == freq[i])
                res.push_back(pair.first);
            }
        }
        return res;
    }
};
```

### 349. [**Intersection of Two Arrays**](https://leetcode.com/problems/intersection-of-two-arrays/description/) [Easy]

Given two integer arrays nums1 and nums2, return an array of their intersection. Each element in the result must be unique and you may return the result in any order.  
 
**方法1：暴力法** Runtime `13ms` Beats `23.6%` Memory `10.1MB` Beats `74.16%`  
直接暴力遍历，重复的元素加到`values`中，结果中会出现重复元素，利用`sort`排序后，用`unique`去重，最后用`values.erase()`擦除重复数据。如你所见，时间复杂度为`O(n<sup>2</sup>)。

```C++
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        vector<int> values{};
        int nums1_len = nums1.size();
        int nums2_len = nums2.size();
        for( int i = 0; i < nums1_len; i++){
            for( int j = 0; j < nums2_len; j++){
                if(nums1[i] == nums2[j]){
                    values.push_back(nums1[i]);
                    break;
                }
            }
        }
        sort(values.begin(),values.end());
        values.erase(unique(values.begin(),values.end()),values.end());
        return values;
    }
};
```

**方法1：暴力法改善** Runtime `0ms` Beats `100%` Memory `10MB` Beats `90.82%`  
先将两数组排序后剔除重复元素，再进行比较，这里循环的时候用了一个小技巧，当`i`遍循环时找到`nums2[j]`与`nums1[i]`,那么此时`nums2[j]`之前的元素不可能与`nums1[i]`之后的元素相等,所以第`i+1`次循环可以从`nums[j+1]`开始比较，如你所见，时间复杂度改善很大。

```C++
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        vector<int> values{};
        int point = 0;
        sort(nums1.begin(),nums1.end());
        sort(nums2.begin(),nums2.end());
        nums1.erase(unique(nums1.begin(),nums1.end()),nums1.end());
        nums2.erase(unique(nums2.begin(),nums2.end()),nums2.end());
        int nums1_len = nums1.size();
        int nums2_len = nums2.size();
        for(int i = 0; i < nums1_len; i++){
             for(int j = point; j < nums2_len; j++){
                 if(nums1[i] == nums2[j]){
                    values.push_back(nums1[i]);
                    point = j;
                    break;
                 }
             }
        }
        return values;
    }
};
```

### 350. [**Intersection of Two Arrays II**](https://leetcode.com/problems/intersection-of-two-arrays-ii/description/) [Easy]

**方法1：暴力法** Runtime `6ms` Beats `61.88%` Memory `10.1MB` Beats `85.10%`  
排序后进行比较

```C++
class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        vector<int> result{};
        int point = 0;
        int nums1_len = nums1.size();
        int nums2_len = nums2.size();
        sort(nums1.begin(),nums1.end());
        sort(nums2.begin(),nums2.end());
        for (int i = 0; i < nums1_len; i++){
            for (int j = point; j < nums2_len; j++){
                if(nums1[i] == nums2[j]){
                    result.push_back(nums1[i]);
                    point = j + 1;
                    break;
                }
            }
        }
    return result;
    }
};
```

### 414. [**Third Maximum Number**](https://leetcode.com/problems/third-maximum-number/description/) [Easy]

Given an integer array nums, return the third distinct maximum number in this array. If the third maximum does not exist, return the maximum number.  
**方法1：排序法** Runtime `7 ms` Beats `64.87%` Memory `9.2MB` Beats `69.16%`

```C++
class Solution {
public:
    int thirdMax(vector<int>& nums) {
        int nums_len = nums.size();
        if(nums_len == 1)
            return nums[0];
        sort(nums.begin(),nums.end());
        nums.erase(unique(nums.begin(),nums.end()),nums.end());
        int nums_len1 = nums.size();
        if (nums_len1 >= 3)
            return nums[nums_len1 - 3];
        else 
            return nums[nums_len1 - 1];
    }
};
```

**方法2：暴力法** Runtime `3ms` Beats `95.71%` Memory `9.1MB` Beats `92.6%`  

```C++
class Solution {
public:
    int thirdMax(vector<int>& nums) {
        int nums_len = nums.size();
        int max = nums[0];
        int second = nums[0];
        int third = nums[0];
        for(int i = 0; i < nums_len; i++){
            if(max <= nums[i]){
                if(max < nums[i]){
                    third = second;
                    second = max;
                    max = nums[i];
                }
            }
            else if(second <= nums[i] || second == max){
                if(second == nums[i]);
                else{
                    third = second;
                    second = nums[i];
                }
            }
            else if(third < nums [i] || third == max || third == second)
                third = nums[i];
        }
        if(max == third || max == second || third == second)
            return max;
        else
            return third;
    }
};
```

### 455. [**Assign Cookies**](https://leetcode.com/problems/assign-cookies/description/) [Easy]

Assume you are an awesome parent and want to give your children some cookies. But, you should give each child at most one cookie.  
Each child i has a greed factor g[i], which is the minimum size of a cookie that the child will be content with; and each cookie j has a size s[j]. If s[j] >= g[i], we can assign the cookie j to the child i, and the child i will be content. Your goal is to maximize the number of your content children and output the maximum number.

```C++
lass Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        int num = 0;
        int point = 0;
        int g_len = g.size();
        int s_len = s.size();
        sort(g.begin(),g.end());
        sort(s.begin(),s.end());
        if(s_len == 0)
            return 0;
        for (int i = 0; i < g_len; i++){
            for (int j = point; j < s_len; j++){
                if(g[i] <= s[j] ){
                    num++;
                    point = j + 1;
                    break;
                }
            }
        }
        return num;
    }
};
```

### 881. [**Boats to Save People**](https://leetcode.com/problems/boats-to-save-people/description/) [Medium]

You are given an array people where `people[i]` is the weight of the ith person, and an infinite number of boats where each boat can carry a maximum weight of `limit`. Each boat carries at most two people at the same time, provided the sum of the weight of those people is at most `limit`.

Return the minimum number of boats to carry every given person.  

my solution:first step, sorting.

**方法1: 排序法**  Runtime `85ms` Beats `67.96%` Memory `41.8MB` Beats `92.87%`

```C++
class Solution {
public:
    int numRescueBoats(vector<int>& people, int limit) {
        int boat_num = 0;
        int people_num = people.size();
        int head = 0;
        int tail = people_num - 1;
        sort(people.begin(),people.end());
        for (tail; tail >= head; tail--){
            if(people[tail] > limit)
               continue;
            else if (people[tail] == limit)
               boat_num++;
            else if (people[head] + people[tail] <= limit){
                boat_num++;
                head++;
            }
            else 
                boat_num++;
        }
        return boat_num;
    }
};
```
