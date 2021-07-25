# Problems on MCM Pattern

Identifying Pattern:
When we need to break a problem at different pos like palindrome partitioning, then that question follows MCM pattern.

General Steps:
1. Figure out pos for i and j where i moves from left to right and j moves from right to left.
2. Figure out base condition by thinking of a invalid input.
3. Run a loop for k from i to j and compute temp answer for every break
4. compute final answer and return it.


