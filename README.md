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
	var start, end int
	var max = 0

	if s == "" {
		return s
	}
	for i := 0; i < len(s); i++ {
		m1 := expandAroundCenter(&s, i, i)
		m2 := expandAroundCenter(&s, i, i+1)

		if m1 > m2 {
			max = m1
		} else {
			max = m2
		}

		if max > end-start {
			start = i - (max-1)/2
			end = i + max/2
		}
	}

	return string(s[start : end+1])
}

func expandAroundCenter(s *string, left, right int) int {
	for left >= 0 && right < len(*s) {
		if (*s)[left] != (*s)[right] {
			break
		} else {
			left--
			right++
		}
	}

	return right - left - 1
}
```
