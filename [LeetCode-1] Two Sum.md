## 题目要求
> Given an array of integers, return indices of the two numbers such that they add up to a specific target.
> You may assume that each input would have exactly one solution, and you may not use the same element twice.


> 给定一个整数数组 nums和一个目标值 target，请你在该数组中找出和为目标值的那两个整数，并返回他们的数组下标。
> 你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

## 示例
> Given nums = [2, 7, 11, 15], target = 9, 
> Because nums[0] + nums[1] = 2 + 7 = 9,return [0, 1].


> 给定 nums = [2, 7, 11, 15], target = 9，
> 因为 nums[0] + nums[1] = 2 + 7 = 9,所以返回 [0, 1]。


## 题解思路
题目要点是 一个数不能被重复的使用，比如 [3,2,4], 3是不能够被重复使用的，所以答案应该是[1,2] 而不是[0,0]，这是一个注意事项。

针对这道题，我的想法是逐步去迭代优化解法。第一个想到的是暴力解法。

#### 1.暴力解法
对列表中的每一个元素 x，再从剩余的其他元素中，寻找是否存在一个值等于 target-x。
##### 复杂度分析：
时间复杂度：O($n^2$)，因为需要有循环嵌套。
空间复杂度：O(1)。

#### 2.循环赋值哈希 + 循环使用哈希表查找判断
接下来想到的方向是优化时间复杂度，可以将循环里寻找匹配元素的过程，放到哈希表中进行，将所有元素的值x和下标存在哈希表中，寻找是否存在一个值等于 target-x，在O(1)的时间复杂度内就可以解决。
并且需要注意，循环判断的时候，要避开相同位置的元素，不可使用一个元素两次。

##### 示例代码：
**Java版本**
```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int[] result = new int[2];

        Map<Integer,Integer> numberMap = new HashMap();

        for(int i = 0; i < nums.length; i++) {
            numberMap.put(nums[i], i);
        }

        for(int i = 0; i < nums.length; i++) {
            int need = target - nums[i];
            if(numberMap.get(need) != null && numberMap.get(need) != i) {
                return new int[] {i, numberMap.get(need)};
            }
        }
        throw new IllegalArgumentException("No two sum solution");
    }
}
```
##### 复杂度分析：
时间复杂度：O($n$)，需要遍历两次列表，然后查找的过程的时间复杂度是O(1)。
空间复杂度：O($n$)，因为引入额外的哈希表存储，存储n个元素。

#### 3.合并赋值及使用哈希查找的过程
在上面的解法中，需要首先用列表赋值一次哈希表，然后在循环中再次遍历列表，循环使用哈表判断，这两步可以再次进行合并，在一次遍历列表中，赋值哈希表，并且对是否存在符合条件的数字进行判断，如果有，则直接返回，如果没有，则添加进哈希表。这样的话，也可以避开第二种解法，要特意避开值符合，但位置相同的元素的情况。

##### 示例代码：
**Java版本**
```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer,Integer> numberMap = new HashMap();

        for(int i = 0; i < nums.length; i++) {
            int need = target - nums[i];
            if (numberMap.get(need) != null) {
                return new int[] {i, numberMap.get(need)};
            }
            numberMap.put(nums[i], i);   
        }

        throw new IllegalArgumentException("No two sum solution");
    }
}
```
**Python版本**
```java
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        number_map = {}
        for i,num in enumerate(nums):
            need = target - num
            if target - num in number_map:
                return [number_map[target - num], i]
            number_map[num] = i
```
##### 复杂度分析：
时间复杂度：O($n$)，只需要遍历一次列表，然后查找的过程的时间复杂度是O(1)。
空间复杂度：O($n$)，因为引入额外的哈希表存储，存储n个元素。

## Tag

* 哈希表
* 数组
