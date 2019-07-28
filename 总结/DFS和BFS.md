# DFS与BFS
## DFS
碰到找所有⽅案的题，基本可以确定是 DFS。除了⼆叉树以外的90%DFS的题，要么是排列，要么是组合
### 排列组合问题
#### 组合问题
问题模型：求出所有满⾜条件的“组合”。 判断条件：组合中的元素是顺序⽆关的。 时间复杂度：与 2^n相关
#### 排列问题
问题模型：求出所有满⾜条件的“排列”。 判断条件：组合中的元素是顺序“相关”的。 时间复杂度：与 n!相关
####通⽤的DFS时间复杂度计算公式
$O(答案个数 * 构造每个答案的时间)$
####基本⽅案
找N个数组成的全排列
+ 案例⼀：找N个数组成的全排列
点：每个数为⼀个点
边：任意两两点之间都有连边，且为⽆向边
路径：= 排列 = 从任意点出发到任意点结束经过每个点⼀次且仅⼀次的路径
找所有满⾜某个条件的⽅案：
1. 找到图中的所有满⾜条件的路径
2. 路径 = ⽅案 = 图中节点的排列组合
3. 点、边、路径需要⾃⼰分析
+ 案例⼆：找出⼀个集合的所有⼦集
点：集合中的元素
边：元素与元素之间⽤有向边连接，⼩的点指向⼤的点（为了避免选出12和21折后在哪个重复集
合）
路径： = ⼦集 = 图中任意点出发到任意点结束的⼀条路径

### DFS+拓扑排序+回溯+排列的一道题
有M个任务，任务编号分别是1~M,每个任务有一个消耗时间，有些任务有前置任务，必须完成前置任务才能完成后续任务。求任务的平均完成时间最小时的路径，
若有多个最优路径，选择最按照字典序的那一个。
```
输入样例
5 6
1 2 1 1 1
1 2
1 3
1 4
2 5
3 5
4 5
答案
1 3 4 2 5
```
```java
import java.util.*;
public class Main{
    static int[] res;
    static long time;
    public static void helper(int[][]yilai,int[] times,int[] record,int[] temp,int sum,int index){
        if(index==record.length){

            if(sum<time){
                for(int i=0;i<record.length;i++)res[i]=temp[i];
                time=sum;
            }
            return;
        }
        for(int i=0;i<yilai.length;i++){
            if(record[i]==0){
                record[i]=1;
                for(int j=0;j<record.length;j++){
                    if(yilai[i][j]==1)
                    record[j]+=1;
                }
                temp[index]=i;
                helper(yilai,times,record,temp,sum*2+times[i],index+1);
                record[i]=0;
                for(int j=0;j<record.length;j++){
                    if(yilai[i][j]==1)
                        record[j]-=1;
                }
            }
        }
        return;
    }
    public static void main(String[] args){
        Scanner sc=new Scanner(System.in);
        int N=sc.nextInt(),M=sc.nextInt();
        time=Long.MAX_VALUE;
        res=new int[N];
        int[] times=new int[N];
        for(int i=0;i<N;i++)times[i]=sc.nextInt();
        int[][] yilai=new int[N][N];//j依赖i
        int[] record=new int[N];//-1是不可访问 0是可访问 1是已访问
        for(int i=0;i<M;i++){
            int a=sc.nextInt()-1,b=sc.nextInt()-1;
            yilai[a][b]=1;
            record[b]-=1;
        }
        helper(yilai,times,record,new int[N],0,0);
        for(int i=0;i<record.length-1;i++){
            System.out.print(res[i]+1);
            System.out.print(" ");
        }
        System.out.println(res[record.length-1]+1);
    }
}
```
## BFS
### 什么时候⽤
图的遍历 Traversal in Graph
1. 层级遍历 Level Order Traversal
2. 由点及⾯ Connected Component
3. 拓扑排序 Topological Sorting
4. 最短路径 Shortest Path in Simple Graph
5. ⾮递归的⽅式找所有⽅案 Iteration solution for all possible results
6. ⼆叉树上的宽度优先搜索
### 使⽤队列作为主要的数据结构 
Queue
### 是否需要实现分层？ 
需要分层的算法⽐不需要分层的算法多⼀个循环 Java / C++:
size=queue.size() 如果直接 for (int i = 0; i < queue.size(); i++) 会怎么样？ 问：为什么 Python 可
以直接写 for i in range(len(queue)) ？ --(这里我在java中的处理方法是，每层结束后加一个null进队列，这样每次遇到null就可以知道该分层了，可以一层循环完成)--
#### 图上的宽度优先搜索
哈希表
图中存在环 存在环意味着，同⼀个节点可能重复进⼊队列
#### BFS的时间复杂度
O(N + M)

其中 N 为点数，M 为边数
#### 拓扑排序
能够⽤ BFS 解决的问题，⼀定不要⽤ DFS 去做，因为⽤ Recursion 实现的 DFS 可能造成StackOverflow

拓扑排序并不是传统的排序算法 ⼀个图可能存在多个拓扑序（Topological Order），也可能不存在任何拓扑序

--⼊度（In-degree）： 有向图（Directed Graph）中指向当前节点的点的个数（或指向当前节点的边的条数）,所以拓扑排序中可以建立一个record数组
索引对应着对应的点，若数组的值大于0，则该点的入度为0，若数组的值等于0则代表可以访问,如数组的值小于0则代表已经加入队列过--

算法描述：
1. 统计每个点的⼊度
2. 将每个⼊度为 0 的点放⼊队列（Queue）中作为起始节点
3. 不断从队列中拿出⼀个点，去掉这个点的所有连边（指向其他点的边），其他点的相应的⼊度 - 1
4. ⼀旦发现新的⼊度为 0 的点，丢回队列中
