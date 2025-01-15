[![](https://img.shields.io/github/stars/LeetCode-in-Erlang/LeetCode-in-Erlang?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Erlang/LeetCode-in-Erlang)
[![](https://img.shields.io/github/forks/LeetCode-in-Erlang/LeetCode-in-Erlang?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Erlang/LeetCode-in-Erlang/fork)

## 49\. Group Anagrams

Medium

Given an array of strings `strs`, group **the anagrams** together. You can return the answer in **any order**.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

**Example 1:**

**Input:** strs = ["eat","tea","tan","ate","nat","bat"]

**Output:** [["bat"],["nat","tan"],["ate","eat","tea"]]

**Example 2:**

**Input:** strs = [""]

**Output:** [[""]]

**Example 3:**

**Input:** strs = ["a"]

**Output:** [["a"]]

**Constraints:**

*   <code>1 <= strs.length <= 10<sup>4</sup></code>
*   `0 <= strs[i].length <= 100`
*   `strs[i]` consists of lowercase English letters.

## Solution

```erlang
-spec group_anagrams(Strs :: [unicode:unicode_binary()]) -> [[unicode:unicode_binary()]].
group_anagrams(Strs) ->
    group_anagrams(Strs, dict:new()).

-spec group_anagrams(Strs :: [unicode:unicode_binary()], Acc :: dict:dict()) -> [[unicode:unicode_binary()]].
group_anagrams([], Acc) ->
    dict:fold(fun(_, Val, AccAcc) -> [Val | AccAcc] end, [], Acc);
group_anagrams([Word | Rest], Acc) ->
    Key = create_key(Word),
    NewAcc = case dict:find(Key, Acc) of
                {ok, List} -> dict:store(Key, [Word | List], Acc);
                error -> dict:store(Key, [Word], Acc)
              end,
    group_anagrams(Rest, NewAcc).

-spec create_key(Word :: unicode:unicode_binary()) -> unicode:unicode_binary().
create_key(Word) ->
    CharList = unicode:characters_to_list(Word),  % Convert the binary string to a list of characters
    SortedList = lists:sort(CharList),            % Sort the characters to create a canonical key
    list_to_binary(SortedList).                   % Convert back to binary for the dictionary key
```