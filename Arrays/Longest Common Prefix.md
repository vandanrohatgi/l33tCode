Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string "".

 

Example 1:

Input: strs = ["flower","flow","flight"]
Output: "fl"

Example 2:

Input: strs = ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.

 

Constraints:

    1 <= strs.length <= 200
    0 <= strs[i].length <= 200
    strs[i] consists of only lowercase English letters.


So I don't know why but here's what came to my mind as the first solution:

```go
func longestCommonPrefix(strs []string) string {
	var count int
	sort.SliceStable(strs, func(i, j int) bool {
		return len(strs[i]) < len(strs[j])
	})
	for i := range strs[0] {
		if strs[0][i] == strs[len(strs)-1][i] {
			count++
		} else {
			return strs[0][:count]
		}
	}
	fmt.Println(strs)
	return strs[0]
}
```

For some reason I skipped brute force and this got accepted too! I passed the initial test cases and 114/124 test cases. pretty good for someone like me. 

And here's my solution after handling a case I skipped:

```go

func longestCommonPrefix(strs []string) string {
	var count int
	sort.SliceStable(strs, func(i, j int) bool {
		return len(strs[i]) < len(strs[j])
	})
	smallest := strs[0]
	if len(smallest) == 0 {
		return ""
	}

	count = match(smallest, strs[len(strs)-1])
	for i := range strs {
		if smallest[:count] != strs[i][:count] {
			count = match(smallest[:count], strs[i][:count])
		}
	}
	fmt.Println(strs)
	return smallest[:count]
}

func match(a, b string) int {
	count := 0

	for i := 0; i < len(a); i++ {
		if a[i] == b[i] {
			count++
		} else {
			return count
		}
	}
	return count
}
```
Runtime3 ms
Beats
56.66%
Memory2.5 MB
Beats
15.56%

quite proud of that. 
