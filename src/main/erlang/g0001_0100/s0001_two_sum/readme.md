[![](https://img.shields.io/github/stars/LeetCode-in-Erlang/LeetCode-in-Erlang?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Erlang/LeetCode-in-Erlang)
[![](https://img.shields.io/github/forks/LeetCode-in-Erlang/LeetCode-in-Erlang?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Erlang/LeetCode-in-Erlang/fork)

## 1\. Two Sum

Easy

Given an array of integers `nums` and an integer `target`, return _indices of the two numbers such that they add up to `target`_.

You may assume that each input would have **_exactly_ one solution**, and you may not use the _same_ element twice.

You can return the answer in any order.

**Example 1:**

**Input:** nums = [2,7,11,15], target = 9

**Output:** [0,1]

**Output:** Because nums[0] + nums[1] == 9, we return [0, 1]. 

**Example 2:**

**Input:** nums = [3,2,4], target = 6

**Output:** [1,2] 

**Example 3:**

**Input:** nums = [3,3], target = 6

**Output:** [0,1] 

**Constraints:**

*   <code>2 <= nums.length <= 10<sup>4</sup></code>
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>
*   <code>-10<sup>9</sup> <= target <= 10<sup>9</sup></code>
*   **Only one valid answer exists.**

**Follow-up:** Can you come up with an algorithm that is less than <code>O(n<sup>2</sup>) </code>time complexity?

## Solution

```erlang
﻿% #Easy #Top_100_Liked_Questions #Top_Interview_Questions #Array #Hash_Table
% #Data_Structure_I_Day_2_Array #Level_1_Day_13_Hashmap #Udemy_Arrays #Big_O_Time_O(n)_Space_O(n)
% #AI_can_be_used_to_solve_the_task #2025_01_05_Time_3_(97.50%)_Space_65.32_(7.50%)

-spec two_sum(Nums :: [integer()], Target :: integer()) -> [integer()].
two_sum(Nums, Target) ->
    two_sum(Nums, Target, #{}, 0).

two_sum([], _, _, _) -> undefined;
two_sum([H|T], Target, M, Index) ->
    case M of
        #{ Target-H := Pair } -> [Pair, Index];
        _ -> two_sum(T, Target, M#{ H => Index }, Index + 1)
    end.
```