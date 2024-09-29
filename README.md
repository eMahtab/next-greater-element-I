# Next greater element I
## https://leetcode.com/problems/next-greater-element-i

The next greater element of some element x in an array is the first greater element that is to the right of x in the same array.

You are given two distinct 0-indexed integer arrays nums1 and nums2, where nums1 is a subset of nums2.

For each 0 <= i < nums1.length, find the index j such that nums1[i] == nums2[j] and determine the next greater element of nums2[j] in nums2. If there is no next greater element, then the answer for this query is -1.

Return an array ans of length nums1.length such that ans[i] is the next greater element as described above.

 
```
Example 1:

Input: nums1 = [4,1,2], nums2 = [1,3,4,2]
Output: [-1,3,-1]
Explanation: The next greater element for each value of nums1 is as follows:
- 4 is underlined in nums2 = [1,3,4,2]. There is no next greater element, so the answer is -1.
- 1 is underlined in nums2 = [1,3,4,2]. The next greater element is 3.
- 2 is underlined in nums2 = [1,3,4,2]. There is no next greater element, so the answer is -1.

Example 2:

Input: nums1 = [2,4], nums2 = [1,2,3,4]
Output: [3,-1]
Explanation: The next greater element for each value of nums1 is as follows:
- 2 is underlined in nums2 = [1,2,3,4]. The next greater element is 3.
- 4 is underlined in nums2 = [1,2,3,4]. There is no next greater element, so the answer is -1.
``` 

### Constraints:

1. 1 <= nums1.length <= nums2.length <= 1000

2. 0 <= nums1[i], nums2[i] <= 104

3. All integers in nums1 and nums2 are unique.

4. All the integers of nums1 also appear in nums2.


## Implementation 1 : O(n^2)
```java
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
       if(nums1 == null)
           return null;
        int[] result = new int[nums1.length];
        Map<Integer,Integer> map = new HashMap<>();
        for(int i = 0; i < nums2.length; i++) {
            map.put(nums2[i], i);
        }
        int index = 0;
        for(int i = 0; i < nums1.length; i++) {
            int nums2Index = map.get(nums1[i]);
            boolean found = false;
            for(int j = nums2Index + 1; j < nums2.length; j++) {
                if(nums2[j] > nums1[i]) {
                    found = true;
                    result[index++] = nums2[j];
                    break;
                }
            }
            if(!found)
                result[index++] = -1;
        }
        return result;
    }
}
```

## Follow up: Could you find an O(nums1.length + nums2.length) solution?

## Implementation 2 : Using Stack
```java
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        if(nums2 == null || nums2.length == 0)
          return new int[0];
        int[] nextGreater = new int[nums2.length];
        if(nums2.length >= 1) {
            nextGreater[nums2.length-1] = -1;
        }
        Map<Integer,Integer> valueToIndex = new HashMap<>();
        valueToIndex.put(nums2[nums2.length-1], nums2.length-1);
        Stack<Integer> stack = new Stack<>();
        stack.push(nums2[nums2.length-1]);
        for(int i = nums2.length-2; i >= 0; i--) {
            valueToIndex.put(nums2[i], i);
            if(nums2[i] >= stack.peek()) {
                while(!stack.isEmpty() && nums2[i] >= stack.peek()) {
                    stack.pop();
                }
                if(stack.isEmpty()) {
                    nextGreater[i] = -1;
                } else{
                    nextGreater[i] = stack.peek();
                }
            } else {
                nextGreater[i] = stack.peek();
            }
            stack.push(nums2[i]);
        }
        
        int[] result = new int[nums1.length];
        for(int i = 0; i < nums1.length; i++) {
            result[i] = nextGreater[valueToIndex.get(nums1[i])];
        }
        return result;
    }
}
```

