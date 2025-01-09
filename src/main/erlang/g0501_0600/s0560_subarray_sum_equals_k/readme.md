[![](https://img.shields.io/github/stars/LeetCode-in-Erlang/LeetCode-in-Erlang?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Erlang/LeetCode-in-Erlang)
[![](https://img.shields.io/github/forks/LeetCode-in-Erlang/LeetCode-in-Erlang?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Erlang/LeetCode-in-Erlang/fork)

## 560\. Subarray Sum Equals K

Medium

Given an array of integers `nums` and an integer `k`, return _the total number of subarrays whose sum equals to_ `k`.

A subarray is a contiguous **non-empty** sequence of elements within an array.

**Example 1:**

**Input:** nums = [1,1,1], k = 2

**Output:** 2

**Example 2:**

**Input:** nums = [1,2,3], k = 3

**Output:** 2

**Constraints:**

*   <code>1 <= nums.length <= 2 * 10<sup>4</sup></code>
*   `-1000 <= nums[i] <= 1000`
*   <code>-10<sup>7</sup> <= k <= 10<sup>7</sup></code>

## Solution

```erlang
% #Medium #Top_100_Liked_Questions #Array #Hash_Table #Prefix_Sum #Data_Structure_II_Day_5_Array
% #Big_O_Time_O(n)_Space_O(n) #2025_01_08_Time_47_(100.00%)_Space_79.35_(_%)

-spec subarray_sum(Nums :: [integer()], K :: integer()) -> integer().
subarray_sum(Nums, K) ->
    subarray_sum(Nums, K, 0, 0, #{0 => 1}).

-spec subarray_sum([integer()], integer(), integer(), integer(), #{integer() => integer()}) -> integer().
subarray_sum([], _K, _TempSum, Ret, _SumCount) -> 
    Ret;
subarray_sum([Num | Rest], K, TempSum, Ret, SumCount) ->
    NewTempSum = TempSum + Num,
    Count = maps:get(NewTempSum - K, SumCount, 0),
    NewRet = Ret + Count,
    UpdatedCount = maps:get(NewTempSum, SumCount, 0) + 1,
    NewSumCount = maps:put(NewTempSum, UpdatedCount, SumCount),
    subarray_sum(Rest, K, NewTempSum, NewRet, NewSumCount).
```