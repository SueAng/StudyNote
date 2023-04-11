### 217. [**Contains Duplicate**](https://leetcode.com/problems/contains-duplicate/description/) [Easy]

Given an integer array nums, return true if any value appears at least twice in the array, and return false if every element is distinct.  
**方法1：暴力解法** `Time Limit Exceeded`  
遍历整个数组，注意比较数组中各个元素是否相同：`nums[0]` 与 `nums[1]...nums[nums.len()]`比较；`nums[1]` 与 `nums[2]...nums[nums.len()]`比较；··· 。所以比较次数为`(n-1)+(n-2)+···+(2)+(1)`,故算法的时间复杂度为`n*(n-1)/2`。
```
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
```
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
```
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        return nums.size() > set<int>(nums.begin(),nums.end()).size();
    }
};
```
**方法4：HashMap**
```
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

### [**std::sort**](https://cplusplus.com/reference/algorithm/sort/)
我们使用到了`sort`函数，在这里我们认识一下它:
```
default (1)	
template <class RandomAccessIterator>  void sort (RandomAccessIterator first, RandomAccessIterator last);
custom (2)	
template <class RandomAccessIterator, class Compare>  void sort (RandomAccessIterator first, RandomAccessIterator last, Compare comp);
```
**排序顺序**：将`[first,last)`的元素按升序排列，它是一个不稳定的排序，就是说对于相等的元素，排序后他们的相对顺序可能会发生改变。  
**参数**：`(first,last，cmp)`(首元素地址(必填), 尾元素地址的下一个地址(必填), 比较函数(非必填)),比较函数可以根据自身需要填写，如果不写比较函数，则默认对前面给出的区间进行递增排序。在STL标准容器中，只有vector、string、deque是可以使用sort的。

### 268. [**Missing Number**](https://leetcode.com/problems/missing-number/) [Easy]
Given an array nums containing n distinct numbers in the range [0, n], return the only number in the range that is missing from the array.  
**方法1：求和法** Runtime `24ms` Beats `43.93%` Memory `17.9MB` Beats `69.85%`  
题目比较简单，很容易就能想到，所有元素的和与缺失的元素有一一对应关系，算出无缺元素和与数组元素和的差值，即为缺失的元素。
```
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
```
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

### 349. [**Intersection of Two Arrays**](https://leetcode.com/problems/intersection-of-two-arrays/description/) [Easy]
Given two integer arrays nums1 and nums2, return an array of their intersection. Each element in the result must be unique and you may return the result in any order.  
 
**方法1：暴力法** Runtime `13ms` Beats `23.6%` Memory `10.1MB` Beats `74.16%`  
直接暴力遍历，重复的元素加到`values`中，结果中会出现重复元素，利用`sort`排序后，用`unique`去重，最后用`values.erase()`擦除重复数据。如你所见，时间复杂度为`O(n<sup>2</sup>)。
```
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
```
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
### 350. [***Intersection of Two Arrays II**](https://leetcode.com/problems/intersection-of-two-arrays-ii/description/) [Easy]
**方法1：暴力法** Runtime `6ms` Beats `61.88%` Memory `10.1MB` Beats `85.10%`  
排序后进行比较
```
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
### 414. [**Third Maximum Number**](https://leetcode.com/problems/third-maximum-number/description/)[Easy]  
Given an integer array nums, return the third distinct maximum number in this array. If the third maximum does not exist, return the maximum number.  
**方法1：排序法** Runtime `7 ms` Beats `64.87%` Memory `9.2MB` Beats `69.16%`
```
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
```
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
```
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
```
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
