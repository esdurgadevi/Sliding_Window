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
