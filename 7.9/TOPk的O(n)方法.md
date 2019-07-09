在未排序的数组中找到第 k 个最大的元素。请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。

示例 1:

输入: [3,2,1,5,6,4] 和 k = 2
输出: 5
示例 2:

输入: [3,2,3,1,2,4,5,5,6] 和 k = 4
输出: 4
说明:

你可以假设 k 总是有效的，且 1 ≤ k ≤ 数组的长度。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/kth-largest-element-in-an-array
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

    //非海量数据下  使用快速排序的方法可以达到O(n)的时间复杂度，但空间复杂度是O(n)，比较消耗内存
    import java.util.*;
    class Solution {
        public void swap(int[] nums,int i,int j){
            if(i>=nums.length||nums[i]==nums[j])return;
            nums[i]=nums[i]^nums[j];
            nums[j]=nums[i]^nums[j];
            nums[i]=nums[i]^nums[j];
        }
        public int[] patition(int[] nums,int i,int j,int target){
            int cur=i;i--;j++;//cur=i!!!!不等于0！！！！
            while(cur<j){
                if(target==nums[cur]){
                    cur++;
                }else if(nums[cur]<target){
                    swap(nums,++i,cur++);
                }else{
                    swap(nums,--j,cur);
                }
            }
            return new int[]{i+1,j-1};
        }
        public int getMedian(int[] nums,int i,int j){
            if(j-i<5)return nums[i];
            int[] temp=new int[5];
            int[] medians=new int[(j-i+1)/5];
            for(int index=0;index<medians.length;index++){
                for(int zz=0;zz<5;zz++) temp[zz]=nums[i+zz+index*5];   
                Arrays.sort(temp);
                medians[index]=temp[2];
            }
            return helper(medians,0,medians.length-1,(medians.length-1)/2);
        }
        public int helper(int[] nums,int i,int j,int k){
            if(j-i<=4){
                int[] res=new int[j-i+1];
                int index=i;
                for(;i<=j;i++){
                    res[i-index]=nums[i];
                }
                Arrays.sort(res);
                return res[k-index]; //不是返回res[k]是res[k-index]
            }
            int[] be_en=patition(nums,i,j,getMedian(nums,i,j));
            if(k<be_en[0]) return helper(nums,i,be_en[0]-1,k);
            else if(be_en[1]<k) return helper(nums,be_en[1]+1,j,k);
            else return nums[be_en[0]];
        }
        public int findKthLargest(int[] nums, int k) {
            return helper(nums,0,nums.length-1,nums.length-k);
        }
    }
