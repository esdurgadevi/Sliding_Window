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
### 345. Reverse Vowels of a String
[Leetcode link](https://leetcode.com/problems/reverse-vowels-of-a-string/description/)
<br>
Given a string s, reverse only all the vowels in the string and return it.

The vowels are 'a', 'e', 'i', 'o', and 'u', and they can appear in both lower and upper cases, more than once.

 

Example 1:

Input: s = "IceCreAm"

Output: "AceCreIm"

Explanation:

The vowels in s are ['I', 'e', 'e', 'A']. On reversing the vowels, s becomes "AceCreIm".

Example 2:

Input: s = "leetcode"

Output: "leotcede"

 

Constraints:

1 <= s.length <= 3 * 105
s consist of printable ASCII characters.

```c
 int iscom(char ch)
{
    if(ch=='a' || ch=='e' || ch=='i' || ch=='o' || ch=='u' || ch=='A' || ch=='E' || ch=='I' || ch=='O' || ch=='U')
    {
        return false;
    }
    return true;
}
char* reverseVowels(char* s) {
    int l=0;
    int r=strlen(s)-1;
    while(l<r)
    {
        while(l<r && iscom(s[l])) l++;
        while(r>l && iscom(s[r])) r--;
        char ch = s[l];
        s[l] = s[r];
        s[r] = ch;
        l++;
        r--;
    }
    return s;
}
```

### 3306. Count of Substrings Containing Every Vowel and K Consonants II
[Leetcode link](https://leetcode.com/problems/count-of-substrings-containing-every-vowel-and-k-consonants-ii/?envType=daily-question&envId=2025-03-10)
<br>
You are given a string word and a non-negative integer k.

Return the total number of substrings of word that contain every vowel ('a', 'e', 'i', 'o', and 'u') at least once and exactly k consonants.

 

Example 1:

Input: word = "aeioqq", k = 1

Output: 0

Explanation:

There is no substring with every vowel.

Example 2:

Input: word = "aeiou", k = 0

Output: 1

Explanation:

The only substring with every vowel and zero consonants is word[0..4], which is "aeiou".

Example 3:

Input: word = "ieaouqqieaouqq", k = 1

Output: 3

Explanation:

The substrings with every vowel and one consonant are:

word[0..5], which is "ieaouq".
word[6..11], which is "qieaou".
word[7..12], which is "ieaouq".
 

Constraints:

5 <= word.length <= 2 * 105
word consists only of lowercase English letters.
0 <= k <= word.length - 5

```java
class Solution {
    public boolean is_vow(char ch)
    {
        if(ch=='a' || ch=='e' || ch=='i' || ch=='o' || ch=='u') return true;
        return false;
    }
    public long find(String word,int k)
    {
        long ans = 0;
        int c = 0;
        int left = 0;
        HashMap<Character,Integer> map = new HashMap<>();
        for(int right = 0;right<word.length();right++)
        {
            if(is_vow(word.charAt(right)))
            {
                map.put( word.charAt(right), map.getOrDefault(word.charAt(right),0)+1 );
            }
            else c++;
            while(map.size()>=5 && c>=k)
            {
                ans += word.length()-right;
                if(!is_vow(word.charAt(left))) c--;
                else 
                {
                    map.put(word.charAt(left),map.get(word.charAt(left))-1);
                    if(map.get(word.charAt(left))==0) map.remove(word.charAt(left));
                }
                left++;
            }
        }
        return ans;
    }
    public long countOfSubstrings(String word, int k) {
        return find(word,k) - find(word,k+1);
    }
}
```
> [Reference](https://www.youtube.com/watch?v=2wANakxRZNo)

### 1358. Number of Substrings Containing All Three Characters
[Leetcod link](https://leetcode.com/problems/number-of-substrings-containing-all-three-characters/?envType=daily-question&envId=2025-03-14)
<br>
Given a string s consisting only of characters a, b and c.
Return the number of substrings containing at least one occurrence of all these characters a, b and c.

Example 1:
Input: s = "abcabc"
Output: 10
Explanation: The substrings containing at least one occurrence of the characters a, b and c are "abc", "abca", "abcab", "abcabc", "bca", "bcab", "bcabc", "cab", "cabc" and "abc" (again). 

Example 2:
Input: s = "aaacb"
Output: 3
Explanation: The substrings containing at least one occurrence of the characters a, b and c are "aaacb", "aacb" and "acb". 

Example 3:
Input: s = "abc"
Output: 1
 

Constraints:
3 <= s.length <= 5 x 10^4
s only consists of a, b or c characters.

```java
class Solution {
    public int numberOfSubstrings(String s) {
        HashMap<Character,Integer> map = new HashMap<>();
        int left = 0;
        int right = 0;
        int ans = 0;
        int n = s.length();
        while(right<n)
        {
            map.put(s.charAt(right),map.getOrDefault(s.charAt(right),0)+1);
            while(map.getOrDefault('a',0)>=1 && map.getOrDefault('b',0)>=1 && map.getOrDefault('c',0)>=1)
            {
                ans += (n-right);
                int temp = map.get(s.charAt(left));
                map.remove(s.charAt(left));
                if(temp-1>0) map.put(s.charAt(left),temp-1);
                left++;
            }
            right++;
        }
        return ans;
    }
}
```
### 2563. Count the Number of Fair Pairs
[Leetcode link](https://leetcode.com/problems/count-the-number-of-fair-pairs/description/?envType=daily-question&envId=2025-04-19)
<br>
Given a 0-indexed integer array nums of size n and two integers lower and upper, return the number of fair pairs.

A pair (i, j) is fair if:

0 <= i < j < n, and
lower <= nums[i] + nums[j] <= upper
 

Example 1:

Input: nums = [0,1,7,4,4,5], lower = 3, upper = 6
Output: 6
Explanation: There are 6 fair pairs: (0,3), (0,4), (0,5), (1,3), (1,4), and (1,5).
Example 2:

Input: nums = [1,7,9,2,5], lower = 11, upper = 11
Output: 1
Explanation: There is a single fair pair: (2,3).
 

Constraints:

1 <= nums.length <= 105
nums.length == n
-109 <= nums[i] <= 109
-109 <= lower <= upper <= 109
#### Brute Force Method
- Two nested Loops
- Time Complexity : O(n<sup>2</sup>)
#### Another Method
```java
class Solution {
    public long countFairPairs(int[] nums, int lower, int upper) {
        int c = 0;
        Arrays.sort(nums);
        for(int i=0;i<nums.length-1;i++)
        {
            int lp = find1(nums,i,lower);
            int up = find2(nums,i,upper);
            if(lp<=up) c=c+(up-lp+1);
        }
        return c;
    }
    public int find1(int[] nums,int i,int lower)
    {
        int j=i+1;
        while(j<nums.length && nums[i]+nums[j]<lower) j++;
        return j;
    }
    public int find2(int[] nums,int i,int upper)
    {
        int j=nums.length-1;
        while(j>=0 && nums[i]+nums[j]>upper) j--;
        return j;
    }
}
```
> Time Complexity : O(n logn)
#### Binary Search Method
```java
```
### 560. Subarray Sum Equals K
[Leetcode link](https://leetcode.com/problems/count-of-interesting-subarrays/?envType=daily-question&envId=2025-04-26)
<br>
Given an array of integers nums and an integer k, return the total number of subarrays whose sum equals to k.

A subarray is a contiguous non-empty sequence of elements within an array.

 

Example 1:

Input: nums = [1,1,1], k = 2
Output: 2
Example 2:

Input: nums = [1,2,3], k = 3
Output: 2
 

Constraints:

1 <= nums.length <= 2 * 104
-1000 <= nums[i] <= 1000
-107 <= k <= 107

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int prefix_sum = 0;
        int count = 0;
        HashMap<Integer,Integer> map = new HashMap<>();
        map.put(0,1);
        for(int i=0;i<nums.length;i++)
        {
            prefix_sum += nums[i];
            if(map.containsKey(prefix_sum-k)) count += map.get(prefix_sum-k);
            map.put(prefix_sum,map.getOrDefault(prefix_sum,0)+1);
        }
        return count;
    }
}
```
- For each time we update the prefix sum by add the current element.
- And we check if the prefixsum-k  will there in the previous calculation or not.
- Because we want to calculate the number of sub arrays with sum k  we calculate the prefix sum till the current index and we search if the prefix sum-k  is exist or not.
- If exist then add the number of time that will exist.

### 2444. Count Subarrays With Fixed Bounds
[Leetcode link](https://leetcode.com/problems/count-subarrays-with-fixed-bounds/description/?envType=daily-question&envId=2025-04-26)
<br>
You are given an integer array nums and two integers minK and maxK.

A fixed-bound subarray of nums is a subarray that satisfies the following conditions:

The minimum value in the subarray is equal to minK.
The maximum value in the subarray is equal to maxK.
Return the number of fixed-bound subarrays.

A subarray is a contiguous part of an array.

 

Example 1:

Input: nums = [1,3,5,2,7,5], minK = 1, maxK = 5
Output: 2
Explanation: The fixed-bound subarrays are [1,3,5] and [1,3,5,2].
Example 2:

Input: nums = [1,1,1,1], minK = 1, maxK = 1
Output: 10
Explanation: Every subarray of nums is a fixed-bound subarray. There are 10 possible subarrays.
 

Constraints:

2 <= nums.length <= 105
1 <= nums[i], minK, maxK <= 106

```java
class Solution {
    public long countSubarrays(int[] nums, int minK, int maxK) {
        long ans = 0;
        int badi = -1,maxi = -1,mini = -1;
        for(int i=0;i<nums.length;i++)
        {
            if(nums[i]<minK || nums[i]>maxK) badi = i;
            if(nums[i] == minK) mini = i;
            if(nums[i] == maxK) maxi = i;
            ans += Math.max(0,Math.min(mini,maxi)-badi);
        }
        return ans;
    }
}
```
> [Reference](https://www.youtube.com/watch?v=Bk-HxzaooqM)

### 560. Subarray Sum Equals K
[Leetcode link](https://leetcode.com/problems/subarray-sum-equals-k/)
<br>
Given an array of integers nums and an integer k, return the total number of subarrays whose sum equals to k.

A subarray is a contiguous non-empty sequence of elements within an array.

 

Example 1:

Input: nums = [1,1,1], k = 2
Output: 2
Example 2:

Input: nums = [1,2,3], k = 3
Output: 2
 

Constraints:

1 <= nums.length <= 2 * 104
-1000 <= nums[i] <= 1000
-107 <= k <= 107

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int prefix_sum = 0;
        int count = 0;
        HashMap<Integer,Integer> map = new HashMap<>();
        map.put(0,1);
        for(int i=0;i<nums.length;i++)
        {
            prefix_sum += nums[i];
            if(map.containsKey(prefix_sum-k)) count += map.get(prefix_sum-k);
            map.put(prefix_sum,map.getOrDefault(prefix_sum,0)+1);
        }
        return count;
    }
}
```
### 2302. Count Subarrays With Score Less Than K
[Leetcode link](https://leetcode.com/problems/count-subarrays-with-score-less-than-k/description/?envType=daily-question&envId=2025-04-28)
<br>
The score of an array is defined as the product of its sum and its length.

For example, the score of [1, 2, 3, 4, 5] is (1 + 2 + 3 + 4 + 5) * 5 = 75.
Given a positive integer array nums and an integer k, return the number of non-empty subarrays of nums whose score is strictly less than k.

A subarray is a contiguous sequence of elements within an array.

 

Example 1:

Input: nums = [2,1,4,3,5], k = 10
Output: 6
Explanation:
The 6 subarrays having scores less than 10 are:
- [2] with score 2 * 1 = 2.
- [1] with score 1 * 1 = 1.
- [4] with score 4 * 1 = 4.
- [3] with score 3 * 1 = 3. 
- [5] with score 5 * 1 = 5.
- [2,1] with score (2 + 1) * 2 = 6.
Note that subarrays such as [1,4] and [4,3,5] are not considered because their scores are 10 and 36 respectively, while we need scores strictly less than 10.
Example 2:

Input: nums = [1,1,1], k = 5
Output: 5
Explanation:
Every subarray except [1,1,1] has a score less than 5.
[1,1,1] has a score (1 + 1 + 1) * 3 = 9, which is greater than 5.
Thus, there are 5 subarrays having scores less than 5.
 

Constraints:

1 <= nums.length <= 105
1 <= nums[i] <= 105
1 <= k <= 1015

```java
class Solution {
    public long countSubarrays(int[] nums, long k) {
        int l=0;
        long sum = 0,count=0;
        for(int r=0;r<nums.length;r++)
        {
            sum += nums[r];
            while(l<r && sum*(r-l+1)>=k)
            {
                sum -= nums[l];
                l++;
            }
            if(sum*(r-l+1)<k) count += (r - l + 1);
        }
        return count;
    }
}
```
