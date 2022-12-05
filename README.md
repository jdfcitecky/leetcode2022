# leetcode2022

### Two Sum
#### Python
+ 直接找剩下的那一個是否有在裡面，不要兩層
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        for i in range(len(nums)):
            if target - nums[i] in nums:
                index = nums.index(target - nums[i])
                if index != i:
                    return [i, nums.index(target - nums[i])]
### Valid Parentheses.
#### Python
+ 注意python有pop這個用法
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []
        counter = [0, 0, 0]
        if len(s) <= 1:
            return False
        for i in range(len(s)):
            if s[i] == '(':
                stack.append(s[i])
                counter[0] = counter[0] + 1
            if s[i] == '{':
                stack.append(s[i])
                counter[1] = counter[1] + 1
            if s[i] == '[':
                stack.append(s[i])
                counter[2] = counter[2] + 1
            if s[i] == ')':
                if len(stack) == 0:
                    return False
                if stack[len(stack) -1] == '(':
                    stack.pop()
                    counter[0] = counter[0] - 1
                else:
                    return False
            if s[i] == ']':
                if len(stack) == 0:
                    return False
                if stack[len(stack) -1] == '[':
                    stack.pop()
                    counter[1] = counter[1] - 1
                else:
                    return False
            if s[i] == '}':
                if len(stack) == 0:
                    return False
                if stack[len(stack) -1] == '{':
                    stack.pop()
                    counter[2] = counter[2] - 1
                else:
                    return False
        if counter[0] + counter[1] + counter[2] == 0:
            return True
        else:
            return False
### Merge Two Sorted Lists.
#### Python
+ 遍歷link list的方法是將head直接指到下一個 list1 = list1.next
class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        result = ListNode(0, None)
        currentNode = result
        listone = []
        listtwo = []
        if list1 == None and list2 == None:
                return None
        if list1 != None:
            while True:
                listone.append(list1.val)
                if list1.next != None:
                    list1 = list1.next
                else:
                    break
        if list2 != None:
            while True:
                listtwo.append(list2.val)
                if list2.next != None:
                    list2 = list2.next
                else:
                    break
        listone = listone + listtwo
        listsorted = sorted(listone)
        print(listsorted)
        count = 0
        for i in listsorted:
            if count == 0:
                result.val = i
                count = count+1
            else:
                currentNode.next = ListNode(i, None)
                currentNode = currentNode.next
        return result
  
### Best Time to Buy and Sell Stock.
#### Python
+ 想法是只走一次，如果斜率是正的就繼續下一個，如果斜率是負的就重設起點
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        profit = 0
        buy = 0
        sell = 1
        if len(prices) < 2:
            return 0
        while sell < len(prices):
            if  prices[buy] < prices[sell]:
                profit = max(prices[sell] - prices[buy], profit)
            else:
                buy = sell
            sell = sell+1
        return profit
###  Valid Palindrome
#### Python
+ 字串處理 isalpha, isdigit, .lower之類的
class Solution:
    def isPalindrome(self, s: str) -> bool:
        strtidy = ""
        if len(s) == 0:
            return True
        if s == " ":
            return True
        for w in s:
            if w.isalpha() or w.isdigit():
                strtidy = strtidy + w.lower()
        if len(strtidy) == 0:
            return True
        
        print(strtidy)
        pt1 = 0
        pt2 = len(strtidy) -1
        while pt1 <= pt2:
            if strtidy[pt1] != strtidy[pt2]:
                return False
            else:
                pt1 = pt1 + 1
                pt2 = pt2 - 1
        return True
                