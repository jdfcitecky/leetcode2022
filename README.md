# leetcode2022

### Two Sum
#### Python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        for i in range(len(nums)):
            for j in range(len(nums)):
                if nums[i] + nums[j] == target:
                    return [i, j]
#### Go
func twoSum(nums []int, target int) []int {
    for i:=0;i<len(nums); i++{
        for j:=0;j<len(nums); j++{
            if (nums[i] + nums[j] == target){
                result := []int{i, j}
                return result
            }
        }
    }
    return []int{0}
}
      