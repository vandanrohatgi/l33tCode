Given a string s and an integer k, return the maximum number of vowel letters in any substring of s with length k.

Vowel letters in English are 'a', 'e', 'i', 'o', and 'u'.

 

Example 1:

Input: s = "abciiidef", k = 3
Output: 3
Explanation: The substring "iii" contains 3 vowel letters.

Example 2:

Input: s = "aeiou", k = 2
Output: 2
Explanation: Any substring of length 2 contains 2 vowels.

Example 3:

Input: s = "leetcode", k = 3
Output: 2
Explanation: "lee", "eet" and "ode" contain 2 vowels.

 

Constraints:

    1 <= s.length <= 105
    s consists of lowercase English letters.
    1 <= k <= s.length


Welp, here's my first brute force:
```go
func maxVowels(s string, k int) int {
	var longest int
	var max int
	vowels := map[string]struct{}{"a": {}, "e": {}, "i": {}, "o": {}, "u": {}}

	for indexOuter := 0; indexOuter < len(s)-k+1; indexOuter++ {
		///fmt.Println(string(s[indexOuter]))
		for indexInner := indexOuter; indexInner < indexOuter+k; indexInner++ {
			fmt.Println(string(s[indexInner]))
			if _, ok := vowels[string(s[indexInner])]; ok {
				longest++
			}
		}
		if longest > max {
			max = longest
		}
		if max == k {
			return k
		}
		longest = 0
		fmt.Println("------------------------")
	}
	return max
}
```
This one basically times out. Afraid this might use Dyanmic programming because I read the word "maximum" I had to go to solutions which suggests using a window. Here's my interpretation of that concept:

```go
func maxVowels(s string, k int) int {
	var count int = countVowels(s[0:k])
	var max int = count
	vowels := map[string]struct{}{"a": {}, "e": {}, "i": {}, "o": {}, "u": {}}

	for index := 1; index < len(s)-k+1 && max != k; index++ {
		fmt.Println(index, index+k)
		if _, ok := vowels[string(s[index-1])]; ok {
			count--
		}
		if _, ok := vowels[string(s[index+k-1])]; ok {
			count++
		}
		if count > max {
			max = count
		}
	}
	return max
}
func countVowels(s string) int {
	count := 0
	vowels := map[string]struct{}{"a": {}, "e": {}, "i": {}, "o": {}, "u": {}}
	for _, i := range s {
		if _, ok := vowels[string(i)]; ok {
			count++
		}
	}
	return count
}
```

As you can see I'm still having problems understanding indexs and hence the random +1 and -1 s. Apart from that, this solution isn't too hot either. At this point my goal is just to be able to at least implment the solution. I'm not trying to become a DS algo wizard. I'm just learning GO by solving problems.
