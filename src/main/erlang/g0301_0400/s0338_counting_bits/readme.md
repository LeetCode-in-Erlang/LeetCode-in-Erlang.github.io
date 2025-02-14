[![](https://img.shields.io/github/stars/LeetCode-in-Erlang/LeetCode-in-Erlang?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Erlang/LeetCode-in-Erlang)
[![](https://img.shields.io/github/forks/LeetCode-in-Erlang/LeetCode-in-Erlang?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Erlang/LeetCode-in-Erlang/fork)

## 338\. Counting Bits

Easy

Given an integer `n`, return _an array_ `ans` _of length_ `n + 1` _such that for each_ `i` (`0 <= i <= n`)_,_ `ans[i]` _is the **number of**_ `1`_**'s** in the binary representation of_ `i`.

**Example 1:**

**Input:** n = 2

**Output:** [0,1,1]

**Explanation:**

    0 --> 0
    1 --> 1
    2 --> 10 

**Example 2:**

**Input:** n = 5

**Output:** [0,1,1,2,1,2]

**Explanation:**

    0 --> 0
    1 --> 1
    2 --> 10
    3 --> 11
    4 --> 100
    5 --> 101 

**Constraints:**

*   <code>0 <= n <= 10<sup>5</sup></code>

**Follow up:**

*   It is very easy to come up with a solution with a runtime of `O(n log n)`. Can you do it in linear time `O(n)` and possibly in a single pass?
*   Can you do it without using any built-in function (i.e., like `__builtin_popcount` in C++)?

## Solution

```erlang
-spec count_bits(N :: integer()) -> [integer()].
count_bits(N) ->
    InitMap = maps:from_list([{X, 0} || X <- lists:seq(0, N div 2)]),
    lists:reverse(do_count_bits(1, N, InitMap, [0])).

do_count_bits(I, N, _Map, Acc) when I > N ->
    Acc;
do_count_bits(I, N, Map, Acc) ->
    IBits = maps:get(I div 2, Map) + (I rem 2),
    NewMap = case I > (N div 2) of
        true -> Map;
        false -> maps:put(I, IBits, Map)
    end,
    do_count_bits(I + 1, N, NewMap, [IBits | Acc]).
```