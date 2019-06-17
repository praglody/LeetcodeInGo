# LeetcodeInGo
Leetcode In Go

**1. 两数之和**

给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

示例：

```
给定 nums = [2, 7, 11, 15], target = 9
因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```

解题：
```
func twoSum(nums []int, target int) []int {
    var tmp = map[int]int{}
    
    for i := range nums{
        if s,ok := tmp[target-nums[i]];ok {
            return []int{s,i}
        }
        tmp[nums[i]] = i
    }
    return nil
}

```

**2. 两数相加**

给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

示例：
```
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807
```

解题：
```
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func addTwoNumbers(l1 *ListNode, l2 *ListNode) *ListNode {
    var carry int = 0
    var tmp,res,pre *ListNode
    tmp = &ListNode{}
    res = tmp
    for {
        if l1 == nil && l2 == nil {
            break
        }
        if l1 != nil {
            carry += l1.Val
        }
        if l2 != nil {
            carry += l2.Val
        }

        if carry > 9 {
            tmp.Val = carry - 10
            carry = 1
        }else{
            tmp.Val = carry
            carry = 0
        }
        tmp.Next = &ListNode{}
        pre = tmp
        tmp = tmp.Next
        
        if l1 != nil && l1.Next != nil{
            l1 = l1.Next
        }else{
            l1 = nil
        }
        
        if l2 != nil && l2.Next != nil{
            l2 = l2.Next
        }else{
            l2 = nil
        }
    }
    if carry > 0 {
        tmp.Val = 1
    }else{
        pre.Next = nil
        tmp = nil
    }
    
    return res
}
```

**3. 无重复字符的最长子串**

给定一个字符串，请你找出其中不含有重复字符的`最长子串`的长度。

示例：
```
输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。

输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。请注意，你的答案必须是`子串`的长度，"pwke" 是一个子序列，不是子串。
```

解题：
```
func lengthOfLongestSubstring(s string) int {
	var left, right, max int
	var sLen = len(s)
	var flag bool

	for right < sLen {
		flag = false
		for i := left; i < right; i++ {
			if s[i] == s[right] {
				left++
				flag = true
				break
			}
		}
		if !flag {
			right++
			if right-left > max {
				max = right - left
			}

		}
	}

	return max
}
```

**5. 最长回文子串**

给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。

示例 1：
输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。

示例 2：
输入: "cbbd"
输出: "bb"

解题：
```
// 中心扩展算法
func longestPalindrome(s string) string {
	var start, end = 0, -1
    
	for i := 0; i < len(s); i++ {
		max := expandAroundCenter(&s, i, i)
		max2 := expandAroundCenter(&s, i, i+1)

		if max < max2 {
			max = max2
		}

		if max > end-start {
			start = i - (max-1)/2
			end = i + max/2
		}

	}
	return s[start : end+1]
}

func expandAroundCenter(s *string, left, right int) int {
	for left >= 0 && right < len(*s) {
		if (*s)[left] != (*s)[right] {
			break
		}
		left--
		right++
	}

	return right - left - 1
}
```

**6. Z 字形变换(zigzag-conversion)**

将一个给定字符串根据给定的行数，以从上往下、从左到右进行 Z 字形排列。

比如输入字符串为 "LEETCODEISHIRING" 行数为 3 时，排列如下：

```
L   C   I   R
E T O E S I I G
E   D   H   N
```

之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："LCIRETOESIIGEDHN"。

请你实现这个将字符串进行指定行数变换的函数：

`string convert(string s, int numRows);`

示例 1:  
输入: s = "LEETCODEISHIRING", numRows = 3  
输出: "LCIRETOESIIGEDHN"

示例 2:  
输入: s = "LEETCODEISHIRING", numRows = 4  
输出: "LDREOEIIECIHNTSG"  
解释:

```
L     D     R
E   O E   I I
E C   I H   N
T     S     G
```

解题：

```
func convert(s string, numRows int) string {
	if s == "" || len(s) <= numRows || numRows == 1 {
		return s
	}

	var rows = make([]string, numRows)
	var curRow int
	var direction bool

	for i := 0; i < len(s); i++ {
		if curRow == 0 || curRow == numRows-1 {
			direction = !direction
		}
		rows[curRow] = rows[curRow] + string(s[i])
		if direction {
			curRow++
		} else {
			curRow--
		}
	}
	for i := 1; i < numRows; i++ {
		rows[0] += rows[i]
	}
	return rows[0]
}
```

**7. 整数反转**

给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

示例 1:  
输入: 123  
输出: 321  

示例 2:  
输入: -123  
输出: -321  

示例 3:  
输入: 120  
输出: 21  

注意:  
假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 `[−2^31,2^31−1]`。请根据这个假设，如果反转后整数溢出那么就返回 0。

解题：

```
func reverse(x int) int {
	var n = int64(x)
	var t int64
	var flag bool

	if n > 0 {
		flag = true
	}else if n < 0{
		flag = false
		n *= -1
	}else{
		return x
	}

	for n > 0{
		t = t * 10 + n % 10
		n = n / 10
	}

	if !flag {
		t *= -1
	}

	if t > 2147483647 || t < -2147483648 {
		return 0
	}

	return int(t)
}
```

**8. 字符串转换整数 (atoi)**

请你来实现一个 atoi 函数，使其能将字符串转换成整数。

首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。

当我们寻找到的第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字组合起来，作为该整数的正负号；假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成整数。

该字符串除了有效的整数部分之后也可能会存在多余的字符，这些字符可以被忽略，它们对于函数不应该造成影响。

注意：假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换。

在任何情况下，若函数不能进行有效的转换时，请返回 0。

说明：假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 [-2^21, 2^31-1]。如果数值超过这个范围，请返回 INT_MAX(2^31−1) 或 INT_MIN (−2^31)。

解题：
```
func myAtoi(str string) int {
	if str == "" {
		return 0
	}

	var t, n int64
	var s string
	var f int8

	for _, v := range str {
		if v >= 48 && v <= 57 {
			if v == 48 && s == "0" {
				continue
			} else {
				s += string(v)
			}
		} else if v == 43 && len(s) == 0 {
			// + 号
			if f == 0 {
				f = 1
			} else {
				return 0
			}
		} else if v == 45 && len(s) == 0 {
			// - 号
			if f == 0 && len(s) == 0 {
				f = -1
			} else {
				return 0
			}
		} else if v == 32 && len(s) == 0 && f == 0 {
			// 空格
			continue
		} else {
			break
		}

		if len(s) == 11 {
			break
		}
	}

	if len(s) == 0 {
		return 0
	}

	for _, v := range s {
		n, _ = strconv.ParseInt(string(v), 10, 64)
		t = t*10 + n
	}

	if f == -1 {
		t *= -1
	}

	if t > 2<<30-1 {
		t = 2<<30 - 1
	} else if t < -(2 << 30) {
		t = -(2 << 30)
	}

	return int(t)
}
```

**9. 回文数（palindrome-number）**

判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

示例 1:  
输入: 121  
输出: true

示例 2:  
输入: -121  
输出: false  
解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。

示例 3:  
输入: 10  
输出: false  
解释: 从右向左读, 为 01 。因此它不是一个回文数。

解题：

```
func isPalindrome(x int) bool {
    if x < 0 || (x%10 == 0 && x != 0) {
        return false
    }
    if x < 10 {
        return true
    }
    
    var t int
    for t < x{
        t = t * 10 + x % 10
        x /= 10
    }
    
    return x == t || x == t / 10
}
```

**11. 盛最多水的容器 (container with most water)**

给定 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

说明：你不能倾斜容器，且 n 的值至少为 2。

![container with most water](https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/07/25/question_11.jpg)

图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。

示例:  
输入: [1,8,6,2,5,4,8,3,7]  
输出: 49

解题：
```
// 暴力法
func maxArea(height []int) int {
	var maxArea int
	var h int
	for i, v := range height {
		for j := len(height) - 1; j > i; j-- {
			if v > height[j] {
				h = height[j] * (j - i)
			} else {
				h = v * (j - i)
			}
			if h > maxArea {
				maxArea = h
			}
		}
	}

	return maxArea
}

// 双指针法
func maxArea(height []int) int {
	var maxArea int
	var i, j = 0, len(height)-1
	
	for i < j {
		if height[i] < height[j] {
			if height[i]*(j-i) > maxArea {
				maxArea = height[i] * (j - i)
			}
			i++
		} else {
			if height[j]*(j-i) > maxArea {
				maxArea = height[j] * (j - i)
			}
			j--
		}
	}
	return maxArea
}
```
