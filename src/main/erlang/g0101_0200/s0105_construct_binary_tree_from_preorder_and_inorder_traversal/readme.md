[![](https://img.shields.io/github/stars/LeetCode-in-Erlang/LeetCode-in-Erlang?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Erlang/LeetCode-in-Erlang)
[![](https://img.shields.io/github/forks/LeetCode-in-Erlang/LeetCode-in-Erlang?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Erlang/LeetCode-in-Erlang/fork)

## 105\. Construct Binary Tree from Preorder and Inorder Traversal

Medium

Given two integer arrays `preorder` and `inorder` where `preorder` is the preorder traversal of a binary tree and `inorder` is the inorder traversal of the same tree, construct and return _the binary tree_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

**Input:** preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]

**Output:** [3,9,20,null,null,15,7]

**Example 2:**

**Input:** preorder = [-1], inorder = [-1]

**Output:** [-1]

**Constraints:**

*   `1 <= preorder.length <= 3000`
*   `inorder.length == preorder.length`
*   `-3000 <= preorder[i], inorder[i] <= 3000`
*   `preorder` and `inorder` consist of **unique** values.
*   Each value of `inorder` also appears in `preorder`.
*   `preorder` is **guaranteed** to be the preorder traversal of the tree.
*   `inorder` is **guaranteed** to be the inorder traversal of the tree.

## Solution

```erlang
%% Definition for a binary tree node.
%%
%% -record(tree_node, {val = 0 :: integer(),
%%                     left = null  :: 'null' | #tree_node{},
%%                     right = null :: 'null' | #tree_node{}}).

-spec build_tree(Preorder :: [integer()], Inorder :: [integer()]) -> #tree_node{} | null.
build_tree([], []) ->
    null;
build_tree([Head | Preorder], Inorder) ->
    {LeftInorder, [_ | RightInorder]} = split_while(fun(X) -> X =/= Head end, Inorder),
    {LeftPreorder, RightPreorder} = take_and_drop(Preorder, length(LeftInorder)),
    #tree_node{
        val = Head,
        left = build_tree(LeftPreorder, LeftInorder),
        right = build_tree(RightPreorder, RightInorder)
    }.

-spec take_and_drop(List :: [integer()], N :: integer()) -> {[integer()], [integer()]}.
take_and_drop(List, N) ->
    take_and_drop(List, N, []).

-spec take_and_drop(List :: [integer()], N :: integer(), Acc :: [integer()]) -> {[integer()], [integer()]}.
take_and_drop(List, 0, Acc) ->
    {lists:reverse(Acc), List};
take_and_drop([Head | Tail], N, Acc) ->
    take_and_drop(Tail, N - 1, [Head | Acc]).

-spec split_while(Fun :: fun((integer()) -> boolean()), List :: [integer()]) -> {[integer()], [integer()]}.
split_while(_, []) ->
    {[], []};
split_while(Fun, [H | T]) ->
    case Fun(H) of
        true ->
            {Left, Right} = split_while(Fun, T),
            {[H | Left], Right};
        false ->
            {[], [H | T]}
    end.
```