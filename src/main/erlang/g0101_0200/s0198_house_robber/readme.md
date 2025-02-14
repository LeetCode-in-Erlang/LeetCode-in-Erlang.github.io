[![](https://img.shields.io/github/stars/LeetCode-in-Erlang/LeetCode-in-Erlang?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Erlang/LeetCode-in-Erlang)
[![](https://img.shields.io/github/forks/LeetCode-in-Erlang/LeetCode-in-Erlang?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Erlang/LeetCode-in-Erlang/fork)

## 198\. House Robber

Medium

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given an integer array `nums` representing the amount of money of each house, return _the maximum amount of money you can rob tonight **without alerting the police**_.

**Example 1:**

**Input:** nums = [1,2,3,1]

**Output:** 4

**Explanation:** Rob house 1 (money = 1) and then rob house 3 (money = 3). Total amount you can rob = 1 + 3 = 4.

**Example 2:**

**Input:** nums = [2,7,9,3,1]

**Output:** 12

**Explanation:** Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1). Total amount you can rob = 2 + 9 + 1 = 12.

**Constraints:**

*   `1 <= nums.length <= 100`
*   `0 <= nums[i] <= 400`

## Solution

```erlang
-spec rob(Nums :: [integer()]) -> integer().
rob(Nums) ->
    Results = lists:foldr(
        fun(E, Results) ->
            Result = max(nth(2, Results), nth(3, Results)),
            [Result + E| Results]
        end,
        [],
        Nums
    ),
    max(nth(1, Results), nth(2, Results)).

nth(Pos, Results) ->
    case length(Results) of
        N when N < Pos ->
            0;
        _ ->
            lists:nth(Pos, Results)
    end.
```