# leetcode2022
### Add Two Numbers
#### Python
+ 注意反轉的用法
    def addTwoNumbers(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        l1n = 1
        l1r = 0
        while l1 != None:
            l1r = l1r + l1n * l1.val
            l1n = l1n * 10
            l1 = l1.next
        l2n = 1
        l2r = 0
        while l2 != None:
            l2r = l2r + l2n * l2.val
            l2n = l2n * 10
            l2 = l2.next
        result = str(l1r + l2r)[::-1]
        ls = ListNode(int(result[0]), None)
        lastN = ls
        for i in range(1, len(result), 1):
            lastN.next = ListNode(int(result[i]), None)
            lastN = lastN.next
        return ls
### Container With Most Water
#### Python
+ 類似maxsubarray
class Solution:
    def maxArea(self, height: List[int]) -> int:
        i = 0
        j = len(height) - 1
        result = 0
        cur = 0
        while i < j:
            if height[i] >= height[j]:
                cur =  height[j] * (j - i)
            else:
                cur =  height[i] * (j - i)
            if cur > result:
                result = cur
            if height[i] < height[j]:
                i = i + 1
            else:
                j = j - 1
        return result
### Letter Combinations of a Phone Number
#### Python
+ 直接寫過去
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        dic = [[], [], ["a", "b", "c"], ["d", "e", "f"], ["g", "h", "i"], ["j", "k", "l"], ["m", "n", "o"], ["p", "q", "r", "s"], ["t", "u", "v"], ["w", "x", "y", "z"]]
        result = []
        count = 0
        for s in digits:
            if count == 0:
                for i in dic[int(s)]:
                    result.append(i)
                    count = count + 1
            else:
                newR = []
                for a in result:
                    for b in dic[int(s)]:
                        newR.append(a + b)
                result = newR
            print(result)
        return result
### Generate Parentheses
#### Python
+ 注意括號的規律
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        if n == 1:
            return ["()"]
        record = []
        self.help("", record, 0, 0, n)
        return record
    def help(self, word, record, l, r, n):
        if l + r == n * 2:
            record.append(word)
            return
        print(l, r)
        if r < n and r < l:
            self.help(word + ")", record, l, r + 1, n)
        if l < n:
            self.help( word + "(", record, l + 1, r, n)
### Swap Nodes in Pairs
#### Python
+注意for的i可能超過邊界
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head == None:
            return None
        record = []
        self.help(head, record)
        for i in range(0, len(record), 2):
            print(i)
            if i <= len(record) - 2:
                tmp = record[i]
                record[i] = record[i + 1]
                record[i + 1] = tmp
        if len(record) == 1:
            return ListNode(record[0], None)
        nHead = ListNode(record[0], None)
        curNode = nHead
        for i in range(1, len(record), 1):
            tmp = ListNode(record[i], None)
            curNode.next = tmp
            curNode = tmp
        return nHead
        

    def help(self, head, record):
        if head == None:
            return
        else:
            record.append(head.val)
            self.help(head.next, record)
### Partition Labels
#### Python
+ 稍微釐清題目邏輯即可
class Solution:
    def partitionLabels(self, s: str) -> List[int]:
        record = []
        curdict ={}
        cur = ""
        count = 0
        for t in range(0, len(s), 1):
            cur = cur + s[t]
            count = count + 1
            curdict[s[t]] = 1
            if t + 1 <= len(s):
                w = 0
                for index in curdict:
                    if index not in s[t + 1:]:
                        w = w+1
                if w == len(curdict):
                    record.append(count)
                    cur = ""
                    curdict = {}
                    count = 0
        return record
### Permutations
#### Python
+ 注意每一個狀態的list都需要複製
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        i = 0
        record = []
        for n in nums:
            Nnums = nums[:]
            Array = []
            Array.append(n)
            Nnums.remove(n)
            self.help(Array, Nnums, record, len(nums))
        return record
    def help(self, Array, nums, record, olen):
        if len(Array) == olen:
            record.append(Array)
        for i in nums:
            NArray = Array[:]
            Nnums = nums[:]
            NArray.append(i)
            Nnums.remove(i)
            self.help(NArray, Nnums, record, olen)
### Rotate image
#### Python
class Solution:
    def rotate(self, matrix: List[List[int]]) -> None:
        for i in range(len(matrix)):
            for j in range(len(matrix)):
                if i <= j:
                    tmp = matrix[i][j]
                    matrix[i][j] = matrix[j][i]
                    matrix[j][i] = tmp
        for i in range(len(matrix)):
            matrix[i] = matrix[i][::-1]
### Group Anagrams
#### Python
+ 第一種方法將time limit exceed
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        record = []
        originStr = strs[:]
        for i in range(len(strs)):
            if strs[i] != "0":
                ar = [strs[i]]
                strs[i] = "0"
                for j in range(i+1, len(strs), 1):
                    if sorted(strs[j]) == sorted(originStr[i]):
                        ar.append(strs[j])
                        strs[j] = "0"
                record.append(ar)
            if strs[i] == "" and [""] not in record:
                record.append([""])
        return record
+ 用字典比較好
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        record = {}
        for i in strs:
            w = ''.join(sorted(i))
            if w in record:
                record[w].append(i)
            else:
                record[w] = [i]
        result = []
        for j in record:
            result.append(record[j])
        return result
### Unique Paths
#### Python
+ 用tuple比用str index來的好
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        record = {}
        return self.help(m, n, record)

    def help(self, m, n, record):
        if m == 1 or n == 1:
            return 1
        if (m, n) in record:
            return record[(m, n)]
        record[(m, n)] = self.help(m - 1, n, record) + self.help(m, n-1, record)
        return record[(m, n)]
### Minimum Path Sum
#### Python
+ 注意邊界條件是大於或者大於等於
class Solution:
    def minPathSum(self, grid: List[List[int]]) -> int:
        record = {}
        return self.help(0, 0, len(grid) - 1, len(grid[0]) -1, grid, record)
        
    def help(self, x, y, m, n, grid, record):
        # print(x, y, m, n)
        r = grid[x][y]
        d = grid[x][y]
        if (x, y) in record:
            return record[(x, y)]
        if x == m and y == n:
            record[(x, y)] = grid[x][y]
            return record[(x, y)]
        elif x + 1 <= m and y == n:
            r = grid[x][y] + self.help(x+ 1, y, m, n, grid, record)
            record[(x, y)] = r
            return record[(x, y)]
        elif x == m and y + 1 <= n:
            d = grid[x][y] + self.help(x, y+1 , m, n, grid, record)
            record[(x, y)] = d
            return record[(x, y)]
        elif x + 1 <= m and y + 1 <= n:
            r = grid[x][y] + self.help(x+ 1, y, m, n, grid, record)
            d = grid[x][y] + self.help(x, y+1 , m, n, grid, record)
            record[(x, y)] = min(r, d)
            return record[(x, y)]
### Combination Sum
#### Python ****
+ 回朔法的使用
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        record = []
        sortC = sorted(candidates)
        self.help(sortC, target, [], record)
        return record
    def help(self, cand, target, array , record):
        print(target)
        curA = array[:]
        for i in cand:
            if target - i == 0:
                curA.append(i)
                sortA = sorted(curA)
                if sortA not in record:
                    record.append(sortA)
            if target - i < cand[0]:
                pass
            if target - i >= cand[0]:
                curA.append(i)
                self.help(cand, target - i, curA, record)
                curA.pop()
### Binary Tree Inorder Traversal
#### Python ****
+ 複習inorder
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        record = []
        self.help(root, record)
        return record
    def help(self, root, record):
        if root == None:
            return
        if root.left != None:
            self.help(root.left, record)
        record.append(root.val)
        if root.right != None:
            self.help(root.right, record)
### Binary Tree Level Order Traversal
#### Python
+ 直接寫過去
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        record = {}
        self.help(root, 0, record)
        result = []
        for i in record:
            result.append(record[i])
        return result
    def help(self, root, layer, record):
        if root == None:
            return
        if str(layer) in record:
            tmp = record[str(layer)][:]
            tmp.append(root.val)
            record[str(layer)] = tmp
        else:
            record[str(layer)] = [root.val]
        if root.left != None:
            self.help(root.left, layer + 1, record)
        if root.right != None:
            self.help(root.right, layer + 1, record)