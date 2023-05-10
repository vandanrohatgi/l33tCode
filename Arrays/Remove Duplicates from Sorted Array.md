Given an integer array nums sorted in non-decreasing order, remove the duplicates in-place such that each unique element appears only once. The relative order of the elements should be kept the same. Then return the number of unique elements in nums.

Consider the number of unique elements of nums to be k, to get accepted, you need to do the following things:

    Change the array nums such that the first k elements of nums contain the unique elements in the order they were present in nums initially. The remaining elements of nums are not important as well as the size of nums.
    Return k.

Custom Judge:

The judge will test your solution with the following code:

int[] nums = [...]; // Input array
int[] expectedNums = [...]; // The expected answer with correct length

int k = removeDuplicates(nums); // Calls your implementation

assert k == expectedNums.length;
for (int i = 0; i < k; i++) {
    assert nums[i] == expectedNums[i];
}

If all assertions pass, then your solution will be accepted.

 

Example 1:

Input: nums = [1,1,2]
Output: 2, nums = [1,2,_]
Explanation: Your function should return k = 2, with the first two elements of nums being 1 and 2 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).

Example 2:

Input: nums = [0,0,1,1,1,2,2,3,3,4]
Output: 5, nums = [0,1,2,3,4,_,_,_,_,_]
Explanation: Your function should return k = 5, with the first five elements of nums being 0, 1, 2, 3, and 4 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).

 

Constraints:

    1 <= nums.length <= 3 * 104
    -100 <= nums[i] <= 100
    nums is sorted in non-decreasing order.


Well, I'm learning interfaces and structs so why not use them?:

```go
func removeDuplicates(nums []int) int {
	var goodMap myMap
	goodMap.numbers = make(map[int]struct{})
	for i := range nums {
		goodMap.numbers[i] = struct{}{}
	}
	for i, j := range goodMap.sort() {
		nums[i] = j
	}
	return goodMap.count
}

type myMap struct {
	count   int
	numbers map[int]struct{}
}

type myMapMethods interface {
	sort() []int
	getArray() []int
}

func (m myMap) sort() []int {
	keyArray := m.getArray()
	sort.Slice(keyArray, func(i, j int) bool { return keyArray[i] < keyArray[j] })
	return keyArray
}

func (m myMap) getArray() []int {
	keys := make([]int, len(m.numbers))
	for i := range m.numbers {
		keys[m.count] = i
		m.count++
	}
	return keys
}
```

it doesn't work but, it's my repository. I can post here what I want... sue me. 

Well, turns out I just had to use pointers. I kinda figured that was the case. I'm actually kinda proud of this solution, coz not only it ran close to the first time , it also beat 40% of the runtimes.

```go
func (m *myMap) sort() []int {
	....
}

func (m *myMap) getArray() []int {
	....
}
```

Right. Let the try harding begin. because I just forgot that this array is already sorted in ascending order. Not gonna lie, I only got reminded of this after I glanced at a solution. 

```go
func removeDuplicates(nums []int) int {
	var count int
	for i := 0; i < len(nums)-1; i++ {
		if nums[i] < nums[i+1] {
			nums[count] = nums[i]
			count++
		}
	}
	return count
}
```
This solution was falling short of the last element. Here is the final working* solution which I think is the best I can do:

```go
func removeDuplicates(nums []int) int {
	var count int = 1 
	for i := 1; i < len(nums); i++ {
		if nums[i-1] < nums[i] {
			nums[count] = nums[i]
			count++
		}
	}
	return count
}
```
