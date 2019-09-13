# Problem:
[Smallest Integer Divisible by K](https://leetcode.com/problems/smallest-integer-divisible-by-k/)

# Pre-Requisites and Tags:
Pigeonhole Principle, Modular arithmatic

# Quick Explanation:
Realize that due to Pigeonhole Principle, after trying at most `K` valid numbers, we are guaranteed to have a repetition of remainder. Lets say the repeated remainder is `X`. It is *repeated*, which means we arrived at `X` before, and after doing the operation a few times we arrive at it again. Since our operation is constant, it means we will be arriving at same set of remainders again and again. Hence, a cycle is formed. If `0` is not obtained so far, its impossible to obtain it, and we return -1. 

If we get `0` within first `K` tries, we can easily return the answer for that case.

# Explanation:

We will discuss the following things in the editorial-

1. Prove that we will be arriving at same set of remainders again and again, i.e. if we once arrived at `X` and the next remainder was `Y`, then whenever we arrive at `X`, our next remainder will be `Y` irrespective of number of times we have done the operation.
2. Integrate above observation with Pigeonhole Principle and arrive at final solution.
3. Discuss an Implementation.

### 1. Proof that if we arrive at `X` and the next remainder is `Y`, then no matter how many times we do the operation the next remainder after `X` will always be `Y`.

To start with the proof, we must first see what our operation does. Our operation appends `1` at the end of the number. This is equivalent to multiplying the number by `10` and adding `1` to it!

Hence, lets say our current remainder is `X`. Since the next remainder is `Y`, this means that-

`Y=(X*10+1)%K`

Note that `K` is fixed for a test case. `10` and `1` are constants. The only variable here is `X`. And the above equation directly interprets to, that, if we get a remainder of `X`, then the next remainder will always be `Y` since there is only `1` operation and as we said, `K`, `10` and `1` are constants, fixed for each test case.

### 2. Integrate above Observation wiht Pigeonhole:

Now, recall the Pigeonhole principle. According to it, I have `K` slots of remainders, from `[0,K-1]`. But if I get `0`, the question ends there and I have already found an answer! 

Hence, after considering at most `K` numbers, we will either get a `0`, or a remainder `X` which is getting repeated. This means that, we can consider only first `K` numbers made by the operation, at most `K-1` of them can give a distinct remainder such that none of the remainder is `0`. If we get a repeated remainder or `0`, then we are actually done!

But the question is, how and why? What is the significance of this repeated remainder?

Lets say that our repeated remainder is `X`, the remainder after `X` is `Y`, after `Y` is `Z`etc. Lets denote is by `[X,Y,Z,...]`. I have previously arrived at `X`, and now I again arrive at `X`. I claim that since I did not find a remainder of `0` earlier, no solution is possible (because if we did find a remainder of `0`, we would have terminated looking for more numbers and given answer immediately).

Previously after `X`, my next remainder was `Y`. By proof in earlier section, we know that it is again going to be `Y`. Similarly, after `Y` we are bound to arrive at `Z`. **We will again arrive at same set of remainders we arrived at between visiting `X` first time, and re-visiting it again.** And `0` is not among them, else we'd have found an answer and terminated already! Hence, this forms an infinite cycle and no answer is possible for this case.

The above has a very beautiful implication. The smallest answer, if it exists, must exist within first `K` numbers made by the operation, else we get a cycle and no answer is possible! 

### 3. Implementation Ideas:

Note that we do not need to print the final number which is divisible by $K$ (although that could be easily done in $O(N)$ by maintaining it in form of string). Hence, we declare a variable `curr` to keep track of my current remainder, and an array `visited` to keep track of whether a remainder is visited already or not. Initially entire array of `visited` is set to `false`.

We repeat the operation `K` times, i.e. check for first `K` smallest numbers. At each iteration, we check if remainder is `0` or visited already, both cases leading to termination of our algorithm as a definite conclusion is reached. Else, we simply mark the current remainder `curr` as visited and move on to next iteration.

You can find a detailed, commented solution below.


# Solution
```
class Solution {
public:
    int smallestRepunitDivByK(int K) {
        int curr=0;//curr will store the current remainder
        int visited[100001];//Will tell if we visited a particular remainder already or not.
        
        memset(visited,0,sizeof(visited));//Initialize the array to value 0.
                
        //Note that by pigeonhole principle, since at max K remaidners are possible, Hence,
        //we only have to iterate K times at max to determine the answer (if possible).        
        for(int i=1;i<=K;i++)
        {
            curr=curr*10+1;//Appending 1 at end of a number num is same as multiplying
            //num by 10 and then adding 1 to it.
            curr%=K;//find current remainder.
            
            if(curr==0)
                return i;//Number has i digits
            else if(visited[curr])//if we already visited this remainder, we found a cycle.
                return -1;
            else
                visited[curr]=1; //Mark this remainder as visited.
            
        }
        return -1; //If remainder=0 not found so far, answer is not possible.
    }
};
```

# Analysis

#### Time

We do `K` iterations to determine the answer, and hence our time complexity is `O(K)`.

#### Memory
Our `visited` array needs to have `K` elements to keep track of visited status of all remainders in worst case. Everything else is `O(1)`. Hence, our memory used is `O(K)`.

