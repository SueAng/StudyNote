### 881. [Boats to Save People](https://leetcode.com/problems/boats-to-save-people/description/)
You are given an array people where `people[i]` is the weight of the ith person, and an infinite number of boats where each boat can carry a maximum weight of `limit`. Each boat carries at most two people at the same time, provided the sum of the weight of those people is at most `limit`.

Return the minimum number of boats to carry every given person.  

Example 1:
```
Input: people = [1,2], limit = 3
Output: 1
Explanation: 1 boat (1, 2)  
```
Example 2:
```
Input: people = [3,2,2,1], limit = 3
Output: 3
Explanation: 3 boats (1, 2), (2) and (3)  
```
Example 3:
```
Input: people = [3,5,3,4], limit = 5
Output: 4
Explanation: 4 boats (3), (3), (4), (5)
```
Constraints:
```
1 <= people.length <= 5 * 104
1 <= people[i] <= limit <= 3 * 104
```
my solution:first step, sorting.

Submission1:  
```
class Solution {
public:
    int numRescueBoats(vector<int>& people, int limit) {
        int boat_num = 0;
        int people_num = people.size();
        int head = 0;
        int tail = people_num - 1;
        int temp;
        for (int i = 0; i < people_num; i++ ){
            for(int j = i+1; j < people_num; j++ ){
                if(people[i] < people[j]){
                    temp = people[i];
                    people[i] = people[j];
                    people[j] =  temp;
                }
            }
        }
        for (head = 0; head <= tail; head++){
            if(people[head] > limit)
               continue;
            else if (people[head] == limit)
               boat_num++;
            else if (people[head] + people[tail] <= limit){
                boat_num++;
                tail--;
            }
            else 
                boat_num++;
        }
        return boat_num;
    }
};
```
Submission2:
```
class Solution {
public:
    int numRescueBoats(vector<int>& people, int limit) {
        int boat_num = 0;
        int people_num = people.size();
        int head = 0;
        int tail = people_num - 1;
        QuikSort(people, 0, people_num - 1 );
        for (head = 0; head <= tail; head++){
            if(people[head] > limit)
               continue;
            else if (people[head] == limit)
               boat_num++;
            else if (people[head] + people[tail] <= limit){
                boat_num++;
                tail--;
            }
            else 
                boat_num++;
        }
        return boat_num;
    }
    void QuikSort(vector<int>& arr,int begin, int end){
        if(begin > end)
            return;
        int tmp = arr[begin];
        int temp;
        int i = begin;
        int j = end;
        while (i != j){
            while(arr[j] <= tmp && j > i)
                j--;
            while(arr[i] >= tmp && j > i)
                i++;
            if(j > i){
                temp = arr[i];
                arr[i] = arr[j];
                arr[j] = temp;
            }
        }
        arr[begin] = arr[i];
        arr[i] = tmp;
        QuikSort(arr, begin, i - 1);
        QuikSort(arr, i + 1 , end);
    }
};
```