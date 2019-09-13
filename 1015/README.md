#Problem:
[Smallest Integer Divisible by K](https://leetcode.com/problems/smallest-integer-divisible-by-k/)

#Pre-Requisites and Tags:
Pigeonhole Principle, Modular arithmatic

#Quick Explanation:
Realize that due to Pigeonhole Principle, after trying at most $K$ valid numbers, we are guaranteed to have a repetition of remainder. Lets say the repeated remainder is $X$. It is *repeated*, which means we arrived at $X$ before, and after doing the operation a few times we arrive at it again. Since our operation is constant, it means we will be arriving at same set of remainders again and again. Hence, a cycle is formed. If $0$ is not obtained so far, its impossible to obtain it, and we return -1. 

If we get $0$ within first $K$ tries, we can easily return the answer for that case.

#Explanation:

We will discuss the following things in the editorial-

1. Prove that we will be arriving at same set of remainders again and again, i.e. if we once arrived at $X$ and the next remainder was $Y$, then whenever we arrive at $X$, our next remainder will be $Y$ irrespective of number of times we have done the operation.
2. Integrate above observation with Pigeonhole Principle and arrive at final solution.

### 1. Proof that if we arrive at $X$ and the next remainder is $Y$, then no matter how many times we do the operation the next remainder after $X$ will always be $Y$.

To start with the proof, we must first see what our operation does. Our operation appends $1$ at the end of the number. This is equivalent to multiplying the number by $10$ and adding $1$ to it!

Hence, lets say our current remainder is $X$. Since the next remainder is $Y$, this means that-

$Y=(X*10+1)%K$


#Solution
```
class Solution {
public:
    int smallestRepunitDivByK(int K) {
        int curr=0;//curr will store the current remainder
        int visited[100001];//Will tell if we visited a particular remainder already or not.
        
        memset(visited,0,sizeof(visited));//Initialize the array to value 0.
                
        //Note that by pigeonhole principle, since at max K remaidners are possible, we
        //only have to iterate K+1 times at max to determine the answer (if possible).        
        for(int i=1;i<=K+1;i++)
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
