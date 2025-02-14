[![](https://img.shields.io/github/stars/LeetCode-in-Erlang/LeetCode-in-Erlang?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Erlang/LeetCode-in-Erlang)
[![](https://img.shields.io/github/forks/LeetCode-in-Erlang/LeetCode-in-Erlang?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Erlang/LeetCode-in-Erlang/fork)

## 221\. Maximal Square

Medium

Given an `m x n` binary `matrix` filled with `0`'s and `1`'s, _find the largest square containing only_ `1`'s _and return its area_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/26/max1grid.jpg)

**Input:** matrix = \[\["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]

**Output:** 4

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/26/max2grid.jpg)

**Input:** matrix = \[\["0","1"],["1","0"]]

**Output:** 1

**Example 3:**

**Input:** matrix = \[\["0"]]

**Output:** 0

**Constraints:**

*   `m == matrix.length`
*   `n == matrix[i].length`
*   `1 <= m, n <= 300`
*   `matrix[i][j]` is `'0'` or `'1'`.

## Solution

```erlang
-spec maximal_square(Matrix :: [[char()]]) -> integer().
maximal_square([]) -> 0;
maximal_square([[]|_]) -> 0;
maximal_square(Matrix) ->
    M = length(Matrix),
    N = length(hd(Matrix)),
    
    % Initialize DP array with all zeros
    DP = lists:duplicate(M + 1, lists:duplicate(N + 1, 0)),
    
    % Iterate through matrix and build DP table
    {MaxSquare, _} = lists:foldl(
        fun(I, {MaxSoFar, DPCurrent}) ->
            process_row(I, Matrix, N, MaxSoFar, DPCurrent)
        end,
        {0, DP},
        lists:seq(1, M)
    ),
    
    MaxSquare * MaxSquare.

process_row(I, Matrix, N, MaxSoFar, DP) ->
    lists:foldl(
        fun(J, {CurrentMax, CurrentDP}) ->
            case lists:nth(J, lists:nth(I, Matrix)) of
                $1 ->
                    Val1 = get_dp_value(CurrentDP, I-1, J),
                    Val2 = get_dp_value(CurrentDP, I, J-1),
                    Val3 = get_dp_value(CurrentDP, I-1, J-1),
                    NewVal = 1 + min3(Val1, Val2, Val3),
                    NewDP = set_dp_value(CurrentDP, I, J, NewVal),
                    {max(CurrentMax, NewVal), NewDP};
                _ ->
                    {CurrentMax, CurrentDP}
            end
        end,
        {MaxSoFar, DP},
        lists:seq(1, N)
    ).

get_dp_value(DP, I, J) when I > 0, J > 0 ->
    lists:nth(J + 1, lists:nth(I + 1, DP));
get_dp_value(_, _, _) -> 0.

set_dp_value(DP, I, J, Value) ->
    Row = lists:nth(I + 1, DP),
    NewRow = list_set(Row, J + 1, Value),
    list_set(DP, I + 1, NewRow).

list_set(List, Pos, Value) ->
    {Head, [_|Tail]} = lists:split(Pos - 1, List),
    Head ++ [Value] ++ Tail.

min3(A, B, C) ->
    min(A, min(B, C)).

max(A, B) when A > B -> A;
max(_, B) -> B.
```