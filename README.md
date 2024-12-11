# Sliding_Window
### 1423. Maximum Points You Can Obtain from Cards
[Leetcode link](https://leetcode.com/problems/maximum-points-you-can-obtain-from-cards/)
<br>
There are several cards arranged in a row, and each card has an associated number of points. The points are given in the integer array cardPoints. In one step, you can take one card from the beginning or from the end of the row. You have to take exactly k cards. Your score is the sum of the points of the cards you have taken. Given the integer array cardPoints and the integer k, return the maximum score you can obtain.

Example 1:
Input: cardPoints = [1,2,3,4,5,6,1], k = 3
Output: 12
Explanation: After the first step, your score will always be 1. However, choosing the rightmost card first will maximize your total score. The optimal strategy is to take the three cards on the right, giving a final score of 1 + 6 + 5 = 12.

Example 2:
Input: cardPoints = [2,2,2], k = 2
Output: 4
Explanation: Regardless of which two cards you take, your score will always be 4.

Example 3:
Input: cardPoints = [9,7,7,9,7,7,9], k = 7
Output: 55
Explanation: You have to take all the cards. Your score is the sum of points of all cards.

Constraints:
1 <= cardPoints.length <= 105
1 <= cardPoints[i] <= 104
1 <= k <= cardPoints.length
![image](https://github.com/user-attachments/assets/049c5cec-4e64-4f8a-a33b-65f5485f2063)
<br>
```java
class Solution {
    public int maxScore(int[] cardPoints, int k) {
        int left = 0;
        int right = cardPoints.length-k;
        int max = Integer.MIN_VALUE;
        int leftsum = 0;
        int rightsum = 0;
        for(int i=cardPoints.length-k;i<cardPoints.length;i++) rightsum+=cardPoints[i];
        max = Math.max(max,rightsum);
        while(left<k)
        {
            leftsum+=cardPoints[left];
            rightsum-=cardPoints[right];
            max = Math.max(max,leftsum+rightsum);
            left++;
            right++;
        }
        return max;
    }
}
```
- In this problem we maximize the sum by choosing the k number of cards from the given array. The condition over here is we pick it up start and the end of the array by consiquitievely.
- so first we assume that we take all 'k' elements from the right side and hence find the right sum and also assign the left sum as 0.
- Also assign the left pointer to 0 and the right pointer to the k the element from the last that is length of the array-k.
- Then we check if the max is greater than th right sum and change it to max.
- Then what we do is decrease the right sum by one and to increase the left sum by one.
- then continously update the max value. We do the same thing until the left will less than the k finally return the max.
> [Reference](https://www.youtube.com/watch?v=pBWCOCS636U)
### 3. Longest Substring Without Repeating Characters
[Leetcode link](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
<br>
Given a string s, find the length of the longest substring without repeating characters.

Example 1:
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.

Example 2:
Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.

Example 3:
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.

Constraints:
0 <= s.length <= 5 * 104
s consists of English letters, digits, symbols and spaces.
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int left = 0;
        int max = 0;
        HashMap<Character,Integer> map = new HashMap<>();
        for(int i=0;i<s.length();i++)
        {
            if(map.containsKey(s.charAt(i)) && map.get(s.charAt(i))>=left)
            {
                left = map.get(s.charAt(i))+1;
                map.remove(s.charAt(i));
                map.put(s.charAt(i),i);
            }
            else
            {
                map.put(s.charAt(i),i);
                max = Math.max(max,(i-left+1));
            }
        }
        return max;
    }
}
```
- In this code we find the maximum subarray length which did not containe the repeated character.
- So first we fix the left pointer to the 0.
- And iterate through each character if the character is present in the hashmap as well as the characters value is greater than the left pointer means the current character already in the substring starting with index left.
- so we must remove that element also update left pointer value.
- So first we update the left pointer by the already seen characters index.
- then we remove the already in the hashmap character. Then we freshly put the current character and the current index.
- In the else part we put the character and update the max value that is i-left+1.
- Finally we return the max value.
> [Reference](https://www.youtube.com/watch?v=-zSxTJkcdAo)
### 1004. Max Consecutive Ones III
[Leetcode link](https://leetcode.com/problems/max-consecutive-ones-iii/description/)
<br>
Given a binary array nums and an integer k, return the maximum number of consecutive 1's in the array if you can flip at most k 0's.

Example 1:
Input: nums = [1,1,1,0,0,0,1,1,1,1,0], k = 2
Output: 6
Explanation: [1,1,1,0,0,1,1,1,1,1,1]
Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.

Example 2:
Input: nums = [0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1], k = 3
Output: 10
Explanation: [0,0,1,1,1,1,1,1,1,1,1,1,0,0,0,1,1,1,1]
Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.

Constraints:
1 <= nums.length <= 105
nums[i] is either 0 or 1.
0 <= k <= nums.length

Hints:
<br>
- One thing's for sure, we will only flip a zero if it extends an existing window of 1s. Otherwise, there's no point in doing it, right? Think Sliding Window!
- Since we know this problem can be solved using the sliding window construct, we might as well focus in that direction for hints. Basically, in a given window, we can never have > K zeros, right?
- We don't have a fixed size window in this case. The window size can grow and shrink depending upon the number of zeros we have (we don't actually have to flip the zeros here!).
- The way to shrink or expand a window would be based on the number of zeros that can still be flipped and so on.
```java
class Solution {
    public int longestOnes(int[] nums, int k) {
        int left = 0;
        int max = 0;
        for(int i=0;i<nums.length;i++)
        {
            if(nums[i]==1) max = Math.max(max,(i-left+1));
            else
            {
                if(k>0)
                {
                    max = Math.max(max,(i-left+1));
                    k--;
                }
                else
                {
                   while(nums[left]==1)
                   {
                      left++;
                   }
                   left+=1;
                }
            }
        }
        return max;
    }
}
```
- In this code we find the maximum consicutive ones with by flip atmost k zeroes.
- so first we fix the left pointer by 0 and to move over all the element if the element is one then we increse the count.
- Other wise it means zero if the k is greater than 0 then we increase the count because it is consider as the flipping zero.
- if the element is zero also the k value is 0 then we must move the left pointer to decrease the zeroes cout so we increase the left pointer until it reach the zero after increase by one it means that we remove the first occuring zero from the current window.
> [Reference](https://www.youtube.com/watch?v=3E4JBHSLpYk&list=TLPQMTIxMDIwMjQWPwI8DtNRUQ&index=2)
### 219. Contains Duplicate II
[Leetcode link](https://leetcode.com/problems/contains-duplicate-ii/?envType=problem-list-v2&envId=sliding-window&status=TO_DO&difficulty=EASY)
<br>
Given an integer array nums and an integer k, return true if there are two distinct indices i and j in the array such that nums[i] == nums[j] and abs(i - j) <= k.

Example 1:
Input: nums = [1,2,3,1], k = 3
Output: true

Example 2:
Input: nums = [1,0,1,1], k = 1
Output: true

Example 3:
Input: nums = [1,2,3,1,2,3], k = 2
Output: false

Constraints:
1 <= nums.length <= 105
-109 <= nums[i] <= 109
0 <= k <= 105

```java
class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        HashMap<Integer,Integer> map = new HashMap<>();
        for(int i=0;i<nums.length;i++)
        {
            if(!map.containsKey(nums[i])) map.put(nums[i],i);
            else
            {
                if(Math.abs(i-map.get(nums[i]))<=k) return true;
                map.remove(nums[i]);
                map.put(nums[i],i);
            }
        }
        return false;
    }
}
```
- In this code we return true if the i-j and nums[i] == nums[j] is less than the k so whenever we see the number in first time that time we add to the map.
- If we see the second time then we check if the difference between the current poistion and the previous poistion is less than or equal to the k if it is then we rrturn true.
- Else we remove the first occurence and add the current occurence.
### 1652. Defuse the Bomb
[Leetcode link](https://leetcode.com/problems/defuse-the-bomb/?envType=daily-question&envId=2024-11-18)
<br>
You have a bomb to defuse, and your time is running out! Your informer will provide you with a circular array code of length of n and a key k.

To decrypt the code, you must replace every number. All the numbers are replaced simultaneously.

If k > 0, replace the ith number with the sum of the next k numbers.
If k < 0, replace the ith number with the sum of the previous k numbers.
If k == 0, replace the ith number with 0.
As code is circular, the next element of code[n-1] is code[0], and the previous element of code[0] is code[n-1].

Given the circular array code and an integer key k, return the decrypted code to defuse the bomb!

 

Example 1:
Input: code = [5,7,1,4], k = 3
Output: [12,10,16,13]
Explanation: Each number is replaced by the sum of the next 3 numbers. The decrypted code is [7+1+4, 1+4+5, 4+5+7, 5+7+1]. Notice that the numbers wrap around.

Example 2:
Input: code = [1,2,3,4], k = 0
Output: [0,0,0,0]
Explanation: When k is zero, the numbers are replaced by 0. 

Example 3:
Input: code = [2,4,9,3], k = -2
Output: [12,5,6,13]
Explanation: The decrypted code is [3+9, 2+3, 4+2, 9+4]. Notice that the numbers wrap around again. If k is negative, the sum is of the previous numbers.
 
Constraints:
n == code.length
1 <= n <= 100
1 <= code[i] <= 100
-(n - 1) <= k <= n - 1

```java
class Solution {
    public int[] decrypt(int[] code, int k) {
        if(code.length==1)
        {
            int[] ans = new int[1];
            ans[0]=0;
            return ans;
        }
        int[] ans = new int[code.length];
        int sum = 0;
        int left = 0,right = 0;
        if(k>=0) 
        {
            left = 1;
            right = k;
        }
        else if(k<0) 
        {
            right = code.length-1;
            left = code.length-(-k);
        }
        for(int i=left;i<=right;i++) sum+=code[i];
        for(int i=0;i<code.length;i++)
        {
            ans[i]=sum;
            sum-=code[left];
            left=(left+1)%code.length;
            right=(right+1)%code.length;
            sum+=code[right];
        }
        return ans;
    }
}
```
- Int his code we find the ans array if we are in the it element and the the k is positive we find the next k integers sum if the k is negativ we find previous k integers sum if the k value is 0 then we put zero.
- So what we do is first find the first set of sum and set the left and right pointer at correct position after do that for each time we decrease the left pointer value and add the right pointer value.
- if the k is positive left =1 right = k if the k is negative right = last element and the left = length-k th element.
- after set the left and right then find the sum between left and right.
- Traverse through the array for the first element in ans will be sum.
- Then subtract the left pointer value from the sum and increase the left pointer by one and take modulo by size because to prevent overflow.
- Same for the right pointer then add the right pointer value.
- After doing all the things return the ans array.
### 2779. Maximum Beauty of an Array After Applying Operation
[Leetcode link](https://leetcode.com/problems/maximum-beauty-of-an-array-after-applying-operation/?envType=daily-question&envId=2024-12-11)
<br>
You are given a 0-indexed array nums and a non-negative integer k. In one operation, you can do the following: Choose an index i that hasn't been chosen before from the range [0, nums.length - 1]. Replace nums[i] with any integer from the range [nums[i] - k, nums[i] + k]. The beauty of the array is the length of the longest subsequence consisting of equal elements. Return the maximum possible beauty of the array nums after applying the operation any number of times.
Note that you can apply the operation to each index only once. A subsequence of an array is a new array generated from the original array by deleting some elements (possibly none) without changing the order of the remaining elements.
 
Example 1:
Input: nums = [4,6,1,2], k = 2
Output: 3
Explanation: In this example, we apply the following operations:
- Choose index 1, replace it with 4 (from range [4,8]), nums = [4,4,1,2].
- Choose index 3, replace it with 4 (from range [0,4]), nums = [4,4,1,4].
After the applied operations, the beauty of the array nums is 3 (subsequence consisting of indices 0, 1, and 3).
It can be proven that 3 is the maximum possible length we can achieve.

Example 2:
Input: nums = [1,1,1,1], k = 10
Output: 4
Explanation: In this example we don't have to apply any operations.
The beauty of the array nums is 4 (whole array).
 
Constraints:
1 <= nums.length <= 105
0 <= nums[i], k <= 105

```java
class Solution {
    public int maximumBeauty(int[] nums, int k) {
        Arrays.sort(nums);
        int max = 0;
        int left = 0,r=1;
        while(left<nums.length)
        {
            while(r<nums.length && nums[r]-nums[left]<=2*k) 
            {
                r++;
            }
            max = Math.max(max,(r-left));
            left++;
        }
        return max;
    }    
}
```
- In this method we will use the sliding window techniques that is already they given the hints.
- If we sort the array and from the left to right when will the arr[r] - arr[l] is less then equal to 2*k then that is valid subsequence then update the max value finally return max value.
> [Reference](https://www.youtube.com/watch?v=x29QnzSBFVI)
