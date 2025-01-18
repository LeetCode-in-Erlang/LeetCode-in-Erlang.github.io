[![](https://img.shields.io/github/stars/LeetCode-in-Erlang/LeetCode-in-Erlang?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Erlang/LeetCode-in-Erlang)
[![](https://img.shields.io/github/forks/LeetCode-in-Erlang/LeetCode-in-Erlang?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Erlang/LeetCode-in-Erlang/fork)

## 137\. Single Number II

Medium

Given an integer array `nums` where every element appears **three times** except for one, which appears **exactly once**. _Find the single element and return it_.

You must implement a solution with a linear runtime complexity and use only constant extra space.

**Example 1:**

**Input:** nums = [2,2,3,2]

**Output:** 3

**Example 2:**

**Input:** nums = [0,1,0,1,0,1,99]

**Output:** 99

**Constraints:**

*   <code>1 <= nums.length <= 3 * 10<sup>4</sup></code>
*   <code>-2<sup>31</sup> <= nums[i] <= 2<sup>31</sup> - 1</code>
*   Each element in `nums` appears exactly **three times** except for one element which appears **once**.

## Solution

```erlang
-spec single_number(Nums :: [integer()]) -> integer().
single_number(Nums) ->
    Frequencies = lists:foldl(fun count_frequency/2, #{}, Nums),
    {Key, _} = lists:keyfind(1, 2, maps:to_list(Frequencies)),
    Key.

-spec count_frequency(Num :: integer(), Map :: map()) -> map().
count_frequency(Num, Map) ->
    maps:update_with(Num, fun(V) -> V + 1 end, 1, Map).
```