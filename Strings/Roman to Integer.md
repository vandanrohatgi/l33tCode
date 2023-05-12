Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.

Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000

For example, 2 is written as II in Roman numeral, just two ones added together. 12 is written as XII, which is simply X + II. The number 27 is written as XXVII, which is XX + V + II.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used:

    I can be placed before V (5) and X (10) to make 4 and 9. 
    X can be placed before L (50) and C (100) to make 40 and 90. 
    C can be placed before D (500) and M (1000) to make 400 and 900.

Given a roman numeral, convert it to an integer.

 

Example 1:

Input: s = "III"
Output: 3
Explanation: III = 3.

Example 2:

Input: s = "LVIII"
Output: 58
Explanation: L = 50, V= 5, III = 3.

Example 3:

Input: s = "MCMXCIV"
Output: 1994
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.

 

Constraints:

    1 <= s.length <= 15
    s contains only the characters ('I', 'V', 'X', 'L', 'C', 'D', 'M').
    It is guaranteed that s is a valid roman numeral in the range [1, 3999].

So this is an annoying one. Welp, here's my first try without handlng the cases of  subtraction:

```go
func romanToInt(s string) int {
	roman := map[string]int{"I": 1, "V": 5, "X": 10, "L": 50, "C": 100, "D": 500, "M": 1000}
	number := roman[string(s[0])]
	for i := 1; i < len(s); i++ {
		numeral := string(s[i])
		number += roman[numeral]
	}
	return number
}
```

After brute forcing my way through life:
```go
func romanToInt(s string) int {
	roman := map[string]int{"I": 1, "V": 5, "X": 10, "L": 50, "C": 100, "D": 500, "M": 1000}
	number := roman[string(s[0])]
	for i := 1; i < len(s); i++ {
		previousNumeral := string(s[i-1])
		numeral := string(s[i])
		fmt.Println(numeral)
		if numeral == "V" && previousNumeral == "I" {
			number += 3
		} else if numeral == "X" && previousNumeral == "I" {
			number += 8
		} else if numeral == "L" && previousNumeral == "X" {
			number += 30
		} else if numeral == "C" && previousNumeral == "X" {
			number += 80
		} else if numeral == "D" && previousNumeral == "C" {
			number += 300
		} else if numeral == "M" && previousNumeral == "C" {
			number += 800
		} else {
			number += roman[numeral]
		}
		fmt.Println(number)
	}

	return number
}
```
Don't ask about the absolute shit I've posted here. The results are as depressing as this solution:

Runtime35 ms
Beats
5.69%
Memory3.6 MB
Beats
8.22%

Dear diary, today I found out that switch case is not very fast. Replacing all the if/else with switch just made the code a bit more readable.

```go
	for i := 1; i < len(s); i++ {
		previousNumeral := string(s[i-1])
		numeral := string(s[i])
		fmt.Println(previousNumeral + numeral)
		switch romanBs := previousNumeral + numeral; romanBs {
		case "IV":
			number += 3
		case "IX":
			number += 8
		case "XL":
			number += 30
		case "XC":
			number += 80
		case "CD":
			number += 300
		case "CM":
			number += 800
		default:
			number += roman[numeral]
		}
```

Runtime32 ms
Beats
5.69%
Memory3.6 MB
Beats
8.22%

well, I thought hashmaps were always a good way to speed up ;,)

```go
func romanToInt(s string) int {
	roman := map[string]int{"I": 1, "V": 5, "X": 10, "L": 50, "C": 100, "D": 500, "M": 1000, "IV": 3, "IX": 8, "XL": 30, "XC": 80, "CD": 300, "CM": 800}
	number := roman[string(s[0])]
	for i := 1; i < len(s); i++ {
		previousNumeral := string(s[i-1])
		numeral := string(s[i])
		combined := previousNumeral + numeral
		fmt.Println(combined)
		if value, ok := roman[combined]; ok {
			number += value
		} else {
			number += roman[numeral]
		}
		fmt.Println(number)
	}

	return number
}
```

Right time to finally see the solution I guess.
