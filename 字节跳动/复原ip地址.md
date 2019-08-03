给定一个只包含数字的字符串，复原它并返回所有可能的 IP 地址格式。

示例:

输入: "25525511135"
输出: ["255.255.11.135", "255.255.111.35"]

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/restore-ip-addresses
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```java
class Solution {
    public void helper(String s,int groups,String temp,List<String> res){
        if(groups==4){
            if(s.length()==0)res.add(temp);
            return;
        }
        for(int i=0;i<Math.min(3,s.length());i++){
            String cur=s.substring(0,i+1);
            if(Integer.valueOf(cur)<=255){
                if(groups==0)helper(s.substring(i+1),groups+1,temp+cur,res);
                else helper(s.substring(i+1),groups+1,temp+'.'+cur,res);
            }
            if(cur.equals("0"))break;
        }
    }
    public List<String> restoreIpAddresses(String s) {
        List<String> res=new ArrayList<>();
        helper(s,0,"",res);
        return res;
    }
}
```
