给定两个字符串 A 和 B, 寻找重复叠加字符串A的最小次数，使得字符串B成为叠加后的字符串A的子串，如果不存在则返回 -1。

举个例子，A = "abcd"，B = "cdabcdab"。

答案为 3， 因为 A 重复叠加三遍后为 “abcdabcdabcd”，此时 B 是其子串；A 重复叠加两遍后为"abcdabcd"，B 并不是其子串。

注意:

 A 与 B 字符串的长度在1和10000区间范围内。
 ```java
 class Solution {
    public int helper(String A,String B){
        if(B.length()==0)return 0;
        if(A.startsWith(B))return 1;
        if(B.startsWith(A)){
            int res=helper(A,B.substring(A.length()));
            if(res==-1)return -1;    
            return res+1;
        }
        return -1;
    }
    public int repeatedStringMatch(String A, String B) {
        int res=-1;
        for(int i=0;i<A.length();i++){
            String zz=A.substring(i);
            if(zz.startsWith(B))return 1; //重点就是这里，判断是否A只重复一次B就是A的字串。
            if(B.startsWith(zz)){
                int zres=helper(A,B.substring(zz.length()));
                if(zres!=-1){
                    if(res==-1)res=zres+1;
                    else res=Math.min(res,zres+1);
                }
            }
        }
        return res;
    }
}
 ```
