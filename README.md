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
###  Invert Binary Tree
#### Python
+ 使用DFS時記得設一個root == None的邊界條件
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if root == None:
            return root
        result = TreeNode(None, None, None)
        self.traversal(root, result)
        return result
    def traversal(self, rootO, rootR):
        if rootO == None:
            return
        rootR.val = rootO.val
        if rootO.left != None and rootO.right != None:
            print(rootO.val)
            rootR.right = TreeNode(None, None, None)
            self.traversal(rootO.left, rootR.right)
            print(rootO.val)
            rootR.left = TreeNode(None, None, None)
            self.traversal(rootO.right, rootR.left)
        if rootO.right != None and rootO.left == None:
            print(rootO.val)
            rootR.left = TreeNode(None, None, None)
            self.traversal(rootO.right, rootR.left)
        if rootO.left != None and rootO.right == None:
            print(rootO.val)
            rootR.right = TreeNode(None, None, None)
            self.traversal(rootO.left, rootR.right)
        if rootO.left == None and rootO.right == None:
            print(rootO.val)
            rootR.right = None
            rootR.right = None
            return
        return
###  Valid Anagram
#### Python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        sortedS = sorted(s)
        sortedT = sorted(t)
        return sortedS == sortedT
###  Binary Search
#### Python
+ 記得雖然右指標是len(nums) - 1但在切片時要加回去
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        return self.binarySearch(nums, target, 0, len(nums) -1)

    def binarySearch(self, nums: List[int], target: int, pointerL: int, pointerR: int) -> int:
        half = floor(0.5 * (pointerL + pointerR))
        if pointerL == pointerR - 1:
            if nums[pointerL] == target:
                return pointerL
            if nums[pointerR] == target:
                return pointerR
        if nums[half] == target:
            return half
        elif target in nums[pointerL:half]:
            return self.binarySearch(nums, target, pointerL, half)
        elif target in nums[half:pointerR + 1]:
            return self.binarySearch(nums, target, half, pointerR)
        else:
            return -1
###  Flood Fill
#### Python
+ 走圖的問題記得要記錄哪裡有沒有走過
class Solution:
    def floodFill(self, image: List[List[int]], sr: int, sc: int, color: int) -> List[List[int]]:
        self.fill(image, sr, sc, color, image[sr][sc], [])
        print(image)
        return image

    def fill(self, image: List[List[int]], sr: int, sc: int, color: int, originColor: int, walked):
        if (str(sr) + str(sc) in walked):
            return
        else:
            walked.append(str(sr) + str(sc))
        image[sr][sc] = color
        # right
        if sc + 1 <= len(image[0]) - 1:
            if image[sr][sc + 1] == originColor:
                self.fill(image, sr, sc + 1, color,originColor, walked)
        # left
        if sc - 1 >= 0:
            if image[sr][sc - 1] == originColor:
                self.fill(image, sr, sc - 1, color,originColor, walked)
        # top
        if sr - 1 >= 0:
            if image[sr - 1][sc] == originColor:
                self.fill(image, sr - 1, sc, color,originColor, walked)
        # btm
        if sr + 1 <= len(image) - 1:
            if image[sr + 1][sc] == originColor:
                self.fill(image, sr + 1, sc, color,originColor, walked)
###  Lowest Common Ancestor of a Binary Search Tree
#### Python
+ 有一半測資未過
def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
       routeP = self.travelsal(root, p.val, [])
        routeQ = self.travelsal(root, q.val, [])
        print(routeP) 
        print(routeQ)
        pLen = len(routeP)
        qLen = len(routeQ)
        # return None
        if pLen <= qLen:
            for i in range(pLen):
                if routeP[i] != routeQ[i]:
                    return routeP[i - 1]
            return routeP[pLen - 1]
        if pLen > qLen:
            for i in range(qLen):
                if routeP[i] != routeQ[i]:
                    return routeQ[i - 1]
            return routeQ[qLen - 1]

    def travelsal(self, root, target, route):
        route.append(root)
        if root.val == target:
            return route
        if root.left != None:
            if root.left.val == target:
                route.append(root.left)
                return route
            
        if root.right != None:
            if root.right.val == target:
                route.append(root.right)
                return route
            
        if root.left != None:
            self.travelsal(root.left, target, route)
        if root.right != None:
            self.travelsal(root.right, target, route)
        return route
###  Balanced Binary Tree
#### Python
+ 有百分之十測資未過，查證表示有爭議
class Solution:
    def isBalanced(self, root: Optional[TreeNode]) -> bool:
        if root == None:
            return True
        if root.left == None and root.right == None:
            return True
        if root.right != None:
            disRight = self.travelsal(root.right, 0)
        else:
            disRight = 0
        if root.left != None:
            disLeft = self.travelsal(root.left, 0)
        else:
            disLeft = 0
        if (disRight - disLeft) * (disRight - disLeft) <= 1:
            return True
        else:
            return False

    def travelsal(self, root, dis):
        dis = dis + 1
        disLeft = 0
        disRight = 0
        if root.left != None:
            disLeft = self.travelsal(root.left, dis)
        if root.right != None:
            disRight = self.travelsal(root.right, dis)
        return max(disLeft, disRight, dis)
###  Linked List Cycle 
#### Python
+ python沒有記憶體但有obj id。什麼時候要return，什麼時候直接call下一個要注意，另外這一題可以用two pointer來最優解
class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        return self.travelsal(head, [])

    def travelsal(self, head, record):
        if head == None:
            return False
        if id(head) in record:
            return True
        else:
            record.append(id(head))
            return self.travelsal(head.next, record)
###  Implement Queue using Stacks
#### Python
+ queue是從頭push
class MyQueue:

    def __init__(self):
        self.stack = []

    def push(self, x):
        swap = []
        swap.append(x)
        for i in self.stack:
            swap.append(i)
        
        self.stack = swap

    def pop(self) -> int:
        return self.stack.pop()

    def peek(self) -> int:
        return self.stack[-1]

    def empty(self) -> bool:
        return not self.stack
###  First Bad Version
#### Python
+ 二分搜尋
class Solution:
    def firstBadVersion(self, n: int) -> int:
        leftptr = 0
        rightptr = n
        while rightptr >= leftptr:
            midptr = floor((leftptr + rightptr) / 2)
            print(leftptr, midptr, rightptr)
            if isBadVersion(midptr) == False and isBadVersion(midptr + 1) == True:
                return midptr + 1
            # elif isBadVersion(midptr) == True and isBadVersion(midptr + 1) == False:
            #     leftptr = midptr
            elif isBadVersion(midptr) == False and isBadVersion(midptr + 1) == False:
                leftptr = midptr
            elif isBadVersion(midptr) == True and isBadVersion(midptr + 1) == True:
                rightptr = midptr
###  Ransom note
#### Python
+ 寫過去就好了
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        sortR = sorted(ransomNote)
        sortM = sorted(magazine)
        if len(ransomNote) > len(magazine):
            return False
        i = 0
        j = 0
        record = []
        while i < len(sortR) and j < len(sortM):
            if sortR[i] == sortM[j]:
                record.append("0")
                i = i+1
            j = j + 1 
        return len(record) == len(ransomNote)