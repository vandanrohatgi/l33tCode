Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

 

Example 1:

Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].

Example 2:

Input: nums = [3,2,4], target = 6
Output: [1,2]

Example 3:

Input: nums = [3,3], target = 6
Output: [0,1]

 

Constraints:

    2 <= nums.length <= 104
    -109 <= nums[i] <= 109
    -109 <= target <= 109
    Only one valid answer exists.

 
Follow-up: Can you come up with an algorithm that is less than O(n2) time complexity?



### Solution

So here's my attempt at a very brute force solution:

```go
func twoSum(nums []int, target int) []int {
    length:=len(nums)
    for outerIndex,outerValue := range nums[:length-1]{
        //fmt.Println(nums[i+1:])
        for innerIndex,innerValue := range nums[outerIndex+1:]{
            //fmt.Printf("Matching %d with %d \n",i,x)
            if outerValue+innerValue == target{
                return []int{outerIndex,outerIndex+innerIndex+1}
            }
        }
    }
    return []int{420,420}
   }
```

I don't need to be a genius to know this is the worst possible answer. O(n^2) something something. Give me a break this my first coding problem in a while.
Someone said use a hashmap and I looked at the answer, the answer was in python and Golang does not have the helper functions that python does. Here's my interpretation of it:

```go
func twoSum(nums []int, target int) []int {
    digits:=make(map[int]int,len(nums))
    for index,digit := range nums{
        digits[digit]=index
    }
    for i,_ := range nums{
        tmp := target - nums[i]
        second,ok := digits[tmp]
        if second != i && ok{
            return []int{i,second}
        }
    }
    return []int{420,420}
}
```

This one performed much better. Onto my quest for even better answer. I tried calculating time and space complexity for this but yeah... no. I'm too tired already.

## Day 2
Looking further, we can just add the whole thing inside one loop. 
Oh and the time complexity appears to be O(n) and space complexity to be O(n). Trust me, I saw it on leetcode solutions.
