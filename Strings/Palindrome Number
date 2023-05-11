Given an integer x, return true if x is a
palindrome
, and false otherwise.

 

Example 1:

Input: x = 121
Output: true
Explanation: 121 reads as 121 from left to right and from right to left.

Example 2:

Input: x = -121
Output: false
Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.

Example 3:

Input: x = 10
Output: false
Explanation: Reads 01 from right to left. Therefore it is not a palindrome.

 

Constraints:

    -231 <= x <= 231 - 1


So here's my first solution:

```go
func isPalindrome(x int) bool {
	s := strconv.Itoa(x)
	sLength := len(s)-1
	for i := range s {
		if s[i] != s[sLength-i] {
			return false
		}
	}
	return true

}
```

I may be acting cool but I'm giggling inside because it beats 78% in runtime and 90% in memory! We can do some optimization and stop the loop midway at the cost of some memory:

```go
func isPalindrome(x int) bool {
	s := strconv.Itoa(x)
	sLength := len(s)
	if sLength == 1 {
		return true
	}
	loopRange := math.Ceil(float64(sLength) / 2.0)
	for i := 0; i <= int(loopRange); i++ {
		if s[i] != s[sLength-1-i] {
			return false
		}
	}
	return true
}
```
which brings us to:

Runtime15 ms
Beats
89.55%
Memory4.5 MB
Beats
84.74%

And here is one without converting anything to string (I had to lookup a mathematical way to reverse a number). surprisingly this looks a lot more simple yet it is a lot slower:

```go
func isPalindrome(x int) bool {
	reverse := 0
	original := x
	if x < 0 {
		return false
	}
	for x != 0 {
		remainder := x % 10
		reverse = reverse*10 + remainder
		x /= 10
	}
	return original == reverse
}
```
Runtime23 ms
Beats
60.89%
Memory4.3 MB
Beats
99.96%
