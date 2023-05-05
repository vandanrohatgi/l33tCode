Given two 0-indexed integer arrays nums1 and nums2, return a list answer of size 2 where:

    answer[0] is a list of all distinct integers in nums1 which are not present in nums2.
    answer[1] is a list of all distinct integers in nums2 which are not present in nums1.

Note that the integers in the lists may be returned in any order.

 

Example 1:

Input: nums1 = [1,2,3], nums2 = [2,4,6]
Output: [[1,3],[4,6]]
Explanation:
For nums1, nums1[1] = 2 is present at index 0 of nums2, whereas nums1[0] = 1 and nums1[2] = 3 are not present in nums2. Therefore, answer[0] = [1,3].
For nums2, nums2[0] = 2 is present at index 1 of nums1, whereas nums2[1] = 4 and nums2[2] = 6 are not present in nums2. Therefore, answer[1] = [4,6].

Example 2:

Input: nums1 = [1,2,3,3], nums2 = [1,1,2,2]
Output: [[3],[]]
Explanation:
For nums1, nums1[2] and nums1[3] are not present in nums2. Since nums1[2] == nums1[3], their value is only included once and answer[0] = [3].
Every integer in nums2 is present in nums1. Therefore, answer[1] = [].

 

Constraints:

    1 <= nums1.length, nums2.length <= 1000
    -1000 <= nums1[i], nums2[i] <= 1000

Well, on the first sight... God I miss python. Python has a built in functionality just for problems of this sort. There is not "SET" data structure in GO. well... this is the language I chose.

dammit, I cant find the clip of ben saying "This is the woman I chose to fall in love with" to the camera.

So here's my first brute force attempt where I messed up the boolean and had to finally look at the answer
```go
func findDifference(nums1 []int, nums2 []int) [][]int {
	distinct1 := make([]int, 0)
	distinct2 := make([]int, 0)
    found := false
	for _, i := range nums1 {
        found = false
		for _, x := range nums2 {
			//fmt.Println(i, x)
			if i == x {
                found = true
				break
			}
		}
        if !found{
            //fmt.Println(i)
		    distinct1 = append(distinct1, i)
        }
	}

	for _, i := range nums2 {
        found = false
		for _, x := range nums1 {
			fmt.Println(i, x)
			if i == x {
                found=true
				break
			}
		}
        if !found{
            fmt.Println(i)
		    distinct2 = append(distinct2, i)
        }
		
	}
	return [][]int{distinct1, distinct2}
}
```

but this does not handle duplicates in the array named "distinct" which I found hilarious. I used maps to to create our own version of sets:

```go
func findDifference(nums1 []int, nums2 []int) [][]int {
	distinct1Map := make(map[int]struct{})
	distinct2Map := make(map[int]struct{})
	for _, value := range nums1 {
		distinct1Map[value] = struct{}{}
	}
	for _, value := range nums2 {
		distinct2Map[value] = struct{}{}
	}
	distinct1 := make([]int, 0)
	distinct2 := make([]int, 0)
	found := false
	for _, i := range nums1 {
		found = false
		for _, x := range nums2 {
			if i == x {
				found = true
				break
			}
		}
		if found {
			delete(distinct1Map, i)
			delete(distinct2Map, i)
		}
	}
	for i := range distinct1Map {
		distinct1 = append(distinct1, i)
	}

	for i := range distinct2Map {
		distinct2 = append(distinct2, i)
	}
	return [][]int{distinct1, distinct2}
}
```

We can remove some of the complexity by just looping once over just one list:

```go
func findDifference(nums1 []int, nums2 []int) [][]int {
	distinct1Map := make(map[int]struct{})
	distinct2Map := make(map[int]struct{})
	for _, value := range nums1 {
		distinct1Map[value] = struct{}{}
	}
	for _, value := range nums2 {
		distinct2Map[value] = struct{}{}
	}
	distinct1 := make([]int, 0)
	distinct2 := make([]int, 0)

	for _, i := range nums1 {
		if _, ok := distinct2Map[i]; ok {
			delete(distinct1Map, i)
			delete(distinct2Map, i)
		}
	}
	for i := range distinct1Map {
		distinct1 = append(distinct1, i)
	}

	for i := range distinct2Map {
		distinct2 = append(distinct2, i)
	}
	return [][]int{distinct1, distinct2}
}
```

that is still uses 5 loops and I'm disgusted by myself. Going above and beyond now... PLUS ULTRA, (cue all for one theme).
I was able to bring it down to 3 loops at the cost of extra space:
```go
func findDifference(nums1 []int, nums2 []int) [][]int {
	genericMap := make(map[int]int)
	commonMap:=make(map[int]int)
	distinct1 := make([]int, 0)
	distinct2 := make([]int, 0)
	for _, value := range nums1 {
		genericMap[value] = 1
	}
	for _, value := range nums2 {
		if numList, _ := genericMap[value]; numList == 1 {
			commonMap[value] = 0
			continue
		}
		genericMap[value] = 2
	}

	for key, numList := range genericMap {
		if numList == 2 {
			distinct2 = append(distinct2, key)
		} else if _,ok:=commonMap[key]; !ok {
			distinct1 = append(distinct1, key)
		}
	}

	return [][]int{distinct1, distinct2}
}
```

