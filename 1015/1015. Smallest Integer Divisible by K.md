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
