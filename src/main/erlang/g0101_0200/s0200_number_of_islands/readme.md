[![](https://img.shields.io/github/stars/LeetCode-in-Erlang/LeetCode-in-Erlang?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Erlang/LeetCode-in-Erlang)
[![](https://img.shields.io/github/forks/LeetCode-in-Erlang/LeetCode-in-Erlang?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Erlang/LeetCode-in-Erlang/fork)

## 200\. Number of Islands

Medium

Given an `m x n` 2D binary grid `grid` which represents a map of `'1'`s (land) and `'0'`s (water), return _the number of islands_.

An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example 1:**

**Input:** grid = [ 

["1","1","1","1","0"], 

["1","1","0","1","0"], 

["1","1","0","0","0"], 

["0","0","0","0","0"] 

]

**Output:** 1

**Example 2:**

**Input:** grid = [ 

["1","1","0","0","0"], 

["1","1","0","0","0"], 

["0","0","1","0","0"], 

["0","0","0","1","1"] 

]

**Output:** 3

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   `1 <= m, n <= 300`
*   `grid[i][j]` is `'0'` or `'1'`.

## Solution

```erlang
%% Definition for a binary tree node.
%%
%% -record(tree_node, {val = 0 :: integer(),
%%                     left = null  :: 'null' | #tree_node{},
%%                     right = null :: 'null' | #tree_node{}}).

%% @spec right_side_view(Root :: #tree_node{} | null) -> [integer()].
-spec right_side_view(Root :: #tree_node{} | null) -> [integer()].
right_side_view(null) ->
    [];
right_side_view(Root) ->
    right_side_view_level([Root], []).

%% Helper function to traverse the tree level by level
-spec right_side_view_level(Level :: [#tree_node{}], Acc :: [integer()]) -> [integer()].
right_side_view_level([], Acc) ->
    lists:reverse(Acc);
right_side_view_level(Level, Acc) ->
    % Get the value of the rightmost node at this level
    RightmostValue = lists:last([Node#tree_node.val || Node <- Level]),
    % Accumulate the rightmost value
    NewAcc = [RightmostValue | Acc],
    % Prepare the next level
    NextLevel = lists:foldl(fun(Node, AccNextLevel) ->
        case Node of
            #tree_node{left = null, right = null} -> AccNextLevel;
            #tree_node{left = Left, right = Right} ->
                AccNextLevel ++ lists:filter(fun(X) -> X =/= null end, [Left, Right])
        end
    end, [], Level),
    % Recursive call to process the next level
    right_side_view_level(NextLevel, NewAcc).
```