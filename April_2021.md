#  My Leetcode Notes  __  April 2021

## 01 [笨阶乘](https://leetcode-cn.com/problems/clumsy-factorial/)

【笨阶乘】

1. 把我笨死了。

   用VScode，写c++ 都不知道要用名字空间std；

   using namespace std;

【笨思路】
一如既往的switch计算器思路！
在运算减法的情况下误用了递归！导致运算出错！

```c++
输入：10
输出：12
解释：12 = 10 * 9 / 8 + 7 - 6 * 5 / 4 + 3 - 2 * 1
    					(如果减法用递归， +3 就会变成 -3 ) 	
    
```

```c++
class Solution {
public:
    int clumsy(int N){
        int flag = 0; //标识符，用来更改运算符，第一次为乘法运算
        int ans = N; //返回值
        N--;
        while(N){
            switch(flag % 4){
                case 0:
                    ans *= N;
                    break;
                case 1:
                    ans /= N;
                    break;
                case 2:
                    ans += N;
                    break;
                case 3:
                    if(N >= 3){
                        ans -= N * (N - 1) / (N - 2);
                        N -= 2;
                        flag = 1;
                    }else if( N == 2){
                        ans -= 2;
                        N = 1;
                    }else{
                        ans--;
                    }
                    break;
                default:
                    break;
            }
            flag++;
            N--;
        }
        return ans; 
    }
};
```

【评论区的神奇思路】

```c++
// 分成4的递归思路
class Solution {
public:
 int clumsy(int N) {
        if(N<=2)return N;
        if(N==3)return 6;
        int sum=N*(N-1)/(N-2)+N-3;
        N-=4;
        while(N>=4){
         sum+=(-N*(N-1)/(N-2)+N-3);
         N-=4;
        }
        return sum-clumsy(N);
    }
};

//找规律
class Solution {
public:
    int clumsy(int N){
        if(N>4)
        {
            switch(N%4)
            {
                case 0:return N+1;break;
                case 1:return N+2;break;
                case 2:return N+2;break;
                case 3:return N-1;break;
                default:break;
            }
        }
        switch(N)
        {
            case 0:return 0;
            case 1:return 1;
            case 2:return 2;
            case 3:return 6;
            case 4:return 7;
            default:break;
        }
        return 0;
    }
};
```

## 02 [直方图的水量](https://leetcode-cn.com/problems/volume-of-histogram-lcci/)

## 03 [最长公共子序列](https://leetcode-cn.com/problems/longest-common-subsequence/)

## 04 [森林中的兔子](https://leetcode-cn.com/problems/rabbits-in-forest/)

## 05 [合并两个有序数组](https://leetcode-cn.com/problems/merge-sorted-array/)

## 06 [删除有序数组中的重复项 II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array-ii/)

【初始思路】

1. 类似只留一个重复项的思路，添加了第二个判断条件，让其能保有两个重复的元素。
2. 数组长度小于等于2时可以不用处理。
3. 因为要处理 +2 的情况，有数据越界的情况发生，干脆在后面加了两个元素，后面在把它删掉。

【不足】

1. 没有充分利用数组递增这个条件，只在插入数据的时候用了一下。

2. 代码很笨拙：

   执行用时：8 ms, 在所有 C++ 提交中击败了57.79%的用户

   内存消耗：10.7 MB, 在所有 C++ 提交中击败了38.78%的用户

【代码】

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if(nums.size() < 3)
            return nums.size();

        int i = 0, j = 0;
        nums.push_back(nums[0]-1);
        nums.push_back(nums[0]-1);
        for (; i < nums.size() - 2; ++i ){
            if(nums[i] != nums[i + 1])
                nums[j++] = nums[i];
            else if(nums[i + 1] != nums[i + 2])
                nums[j++] = nums[i];
        }
        i += 2;
        while(i - j){
            nums.pop_back();
            j++;
        }
        return nums.size();
    }
};
```

【评论区】[许同学不会认输的](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array-ii/comments/95337)

1. 处理数组越界问题上采用相反的思路，既然数据越界的情况必然是在数组长度大于2的情况下才会发生，故下标index - 2 > 0  一定成立。

2. 思路也不太一样，Ta的思路(我所理解的)，应该是，index作为目标数组的下标，一定会有，

   nums[index] != nums[index - 2] 

   因此只需找到原数组中符合 nums[i] != nums[index - 2] 的 nums[i] 并将其赋值给nums[index] 即可，同时index需要自动加一，即能够寻找下一元素。

   考虑这种情况，

   nums  = [1,2,2,2,3,3];

   index = 2, i = 2;

   显然，

   nums[index] = 2, nums[index - 2] = 0, nums[i] = 2,

   故 line a 语句会被执行，执行后有，

   index = 3, nums[index] = 2, nums[index - 2] = 2;

   循环一次后(即上述line a执行后 i 会加1)，

   ​	i = 3, nums[i] = 2,

   不满足条件，而后,

    	i = 4, nums[i] = 3,

   满足条件，line a被执行，

   index = 4,  nums[index] = 3, nums [index - 2] = 2, nums  = [1,2,2,3,3,3]

   可以发现，元素2只保留了两项，同样的，后续的元素3也会只保留两项（index只会记到第二个元素3，也就是说最后index的值位为5而不是6）。

3. 同样的，对于数组长度小于等于2的数组可以不作处理。

```c++
// 代码参考后略有修改，评论者似乎是用的C#代码
class Solution {
public:
    int removeDuplicates(vector<int>& nums){
        if(nums.size() <= 2) return nums.size();
        
        int index = 2;
        for(int i = 2; i < nums.size(); i++){
            if(nums[i] != nums[index-2])
                nums[index++] = nums[i];// line a
        }
        
        return index;
    }
};
```

【其他参考】

1. [宝宝可乖了](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array-ii/comments/26655)
2. 将前面的长度判断取消的方法: [Passion](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array-ii/comments/870726)
3. [官方题解](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array-ii/solution/shan-chu-pai-xu-shu-zu-zhong-de-zhong-fu-yec2/)

【总结】

1. 在比较当中，采用+1，+2的方式访问数组总要考虑数组越界问题时，可以考虑用减法的方式访问比较。

## 07 [搜索旋转排序数组 II](https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii/)

## 08 [寻找旋转排序数组中的最小值](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/)

【初始思路】

1. 我能不能直接找最小值啊，find方法QAQ。
2. 从两边逼近，找到最小值

【代码】（有误）

```c++
class Solution {
public:
    int findMin(vector<int>& nums) {
        if(nums[0] <= nums[nums.size()-1]) // equals for nums.length() == 1
            return nums[0];
        int len = nums.size();
        int index = 0;

        while(nums[index] > nums[len - index]) //from sides to center to find the minimum
            index++;
        // two situations
        return (nums[index] < nums[len - index + 1]) ? nums[index] : nums[len - index + 1]; 
    }
};  

```

【错误】

>  执行出错
>
> 最后执行的输入：[2,1]

【分析原因】

1. 数组下标问题，越界了(又是越界)，应该是len忘记减1了。

【改动】

```c++
class Solution {
public:
    int findMin(vector<int>& nums) {
        if(nums[0] <= nums[nums.size()-1]) // equals for nums.length() == 1
            return nums[0];
        int len = nums.size() - 1;
        int index = 0;

        while(nums[index] > nums[len - index]) //from sides to center to find the minimum
            index++;
        // two situations
        return (nums[index] < nums[len - index + 1]) ? nums[index] : nums[len - index + 1]; 
    }
};  

// or 
class Solution {
public:
    int findMin(vector<int>& nums) {
        if(nums[0] <= nums[nums.size()-1]) // equals for nums.length() == 1
            return nums[0];
        int len = nums.size();
        int index = 0;

        while(nums[index] > nums[len - index - 1]) //from sides to center to find the minimum
            index++;
        // two situations
        return (nums[index] < nums[len - index]) ? nums[index] : nums[len - index]; 
    }
};  
```

【评价】

> **150 / 150** 个通过测试用例
>
> 状态：*通过*
>
> 执行用时: **0 ms** 
>
> 内存消耗: **9.8 MB** 

[**官方题解**](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/solution/xun-zhao-xuan-zhuan-pai-xu-shu-zu-zhong-5irwp/)

> 二分搜索
>
> 我们考虑数组中的最后一个元素 x：在最小值右侧的元素（不包括最后一个元素本身），它们的值一定都严格小于 x；而在最小值左侧的元素，它们的值一定都严格大于 x。因此，我们可以根据这一条性质，通过二分查找的方法找出最小值。
>
> ```c++
> class Solution {
> public:
>     int findMin(vector<int>& nums) {
>         int low = 0;
>         int high = nums.size() - 1;
>         while (low < high) {
>             int pivot = low + (high - low) / 2;
>             if (nums[pivot] < nums[high]) {
>                 high = pivot;
>             }
>             else {
>                 low = pivot + 1;
>             }
>         }
>         return nums[low];
>     }
> };
> 
> 作者：LeetCode-Solution
> 链接：https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/solution/xun-zhao-xuan-zhuan-pai-xu-shu-zu-zhong-5irwp/
> 来源：力扣（LeetCode）
> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
> ```

【总结】

关于有序搜索查找的问题，多多考虑利用二分法。从不同角度对二分法进行使用，可以加深对二分法的理解。

## 09 [寻找旋转排序数组中的最小值 II](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii/)

【思路】

1. 和昨天的题类似，那么应该只需要对二分法稍作修改就行了。

2. 刚开始想的多个条件就行了。

   ```c++
   class Solution {
   public:
       int findMin(vector<int>& nums) {
           int low = 0, high = nums.size() - 1;
           int mid = 0;
           while(low < high) {
               mid = (low + high) / 2;
               if(nums[mid] < nums[high])
                   high = mid;
               else if(nums[mid] > nums[high])
                   low = mid + 1;
               else
                   high--; //新加的语句
           }
           return nums[low];
       }
   };
   
   //执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户
   //内存消耗：11.9 MB, 在所有 C++ 提交中击败了78.69%的用户
   ```

3. 后来想着能不能递归，结果特别慢。。。

   ```c++
   class Solution {
   public:
       int findMin(vector<int>& nums) {
           int low = 0, high = nums.size() - 1;
           int mid = 0;
           while(low < high) {
               mid = (low + high) / 2;
               if(nums[mid] < nums[high])
                   high = mid;
               else if(nums[mid] > nums[high])
                   low = mid + 1;
               else{
                   if(low == mid)
                       return nums[low];
                   vector<int> pre(nums.begin() + low, nums.begin() + mid);
                   vector<int> last(nums.begin() + mid, nums.begin() + high + 1);
                   return (findMin(pre) < findMin(last)) ? findMin(pre) : findMin(last);
               }    
           }
           return nums[low];
       }
   };
   
   //执行用时：96 ms, 在所有 C++ 提交中击败了22.48%的用户
   //内存消耗：37.8 MB, 在所有 C++ 提交中击败了5.12%的用户
   ```

[官方题解](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii/solution/xun-zhao-xuan-zhuan-pai-xu-shu-zu-zhong-de-zui--16/)与思路一最开始的想法一致。

【总结】

​	有时没必要苛求特别的算法。

## 12 [最大数](https://leetcode-cn.com/problems/largest-number/)

【初始思路】

1. 基数排序？
2. 太难了，没做出来，花了时间都没做出来

[官方题解](https://leetcode-cn.com/problems/largest-number/solution/zui-da-shu-by-leetcode-solution-sid5/)

[宫水三叶の相信科学系列](https://leetcode-cn.com/problems/largest-number/solution/gong-shui-san-xie-noxiang-xin-ke-xue-xi-vn86e/)

## 13 [ 二叉搜索树节点最小距离](https://leetcode-cn.com/problems/minimum-distance-between-bst-nodes/)

【初始思路】

1. 只需比较root->right - root 和 root - root->left的值的大小即可。
2. 需要使用递归。
3. 要考虑没有左右子树或只有一个子树的情况。

```c++
class Solution {
public:
    int minDiffInBST(TreeNode* root) {
        if(root->left){ 
            //有左子树
            if(root->right){ 
                // 左右子树都有
                int min = ((root->val - root->left->val) < (root->right->val - root->val)) ?  (root->val - root->left->val) : (root->right->val - root->val);
                min = (min < minDiffInBST(root->left)) ? min : minDiffInBST(root->left);
                return (min < minDiffInBST(root->right)) ? min : minDiffInBST(root->right);
            }
            //只有左子树
            return ((root->val - root->left->val) < minDiffInBST(root->left)) ? (root->val - root->left->val) : minDiffInBST(root->left);
        }else if(root->right) // 只有右子树
            return ((root->right->val - root->val) < minDiffInBST(root->right)) ? (root->right->val - root->val) : minDiffInBST(root->right);
        else //叶子节点，由题可知，树不会是单个的叶子结点
            return root->val;
    }
};

/*
执行结果：解答错误
显示详情
输入：
[1,0,48,null,null,12,49]
输出：
0
预期结果：
1
*/
```

【解答出错】

1. 叶子结点的解决办法出现错误。

【解决办法】

1. 叶子节点情况下应返还最大值（100000）。

2. >**提示：**
   >
   >- 树中节点数目在范围 `[2, 100]` 内
   >- `0 <= Node.val <= 100000`

```c++
class Solution {
public:
    int minDiffInBST(TreeNode* root) {
        if(root->left){ 
            //有左子树
            if(root->right){ 
                // 左右子树都有
                int min = ((root->val - root->left->val) < (root->right->val - root->val)) ?  (root->val - root->left->val) : (root->right->val - root->val);
                min = (min < minDiffInBST(root->left)) ? min : minDiffInBST(root->left);
                return (min < minDiffInBST(root->right)) ? min : minDiffInBST(root->right);
            }
            //只有左子树
            return ((root->val - root->left->val) < minDiffInBST(root->left)) ? (root->val - root->left->val) : minDiffInBST(root->left);
        }else if(root->right) // 只有右子树
            return ((root->right->val - root->val) < minDiffInBST(root->right)) ? (root->right->val - root->val) : minDiffInBST(root->right);
        else //叶子节点，由题可知，树不会是单个的叶子结点
            return 100000;
    }
};

/*
执行结果：解答错误
显示详情
输入：
[90,69,null,49,89,null,52]
输出：
3
预期结果：
1
*/
```

【解答出错】

1. “任意”结点，而不是“相邻”结点（感谢评论区[摘星](https://leetcode-cn.com/problems/minimum-distance-between-bst-nodes/comments/644436)）。

【解决办法】

1. 类似中序遍历寻找间距最小（感谢评论区[Coding_Gengjiabo](https://leetcode-cn.com/problems/minimum-distance-between-bst-nodes/comments/102121)）。

```

```

【[官方题解](https://leetcode-cn.com/problems/minimum-distance-between-bst-nodes/solution/er-cha-sou-suo-shu-jie-dian-zui-xiao-ju-8u87w/)】

## 14 [实现 Trie (前缀树)](https://leetcode-cn.com/problems/implement-trie-prefix-tree/)

【[官方题解](https://leetcode-cn.com/problems/implement-trie-prefix-tree/solution/shi-xian-trie-qian-zhui-shu-by-leetcode-ti500/)】

## 15 [打家劫舍 II](https://leetcode-cn.com/problems/house-robber-ii/)

【初始思路】

1. **贪心算法**。

   > 举出反例后发现不行
   >
   > 反例: {1, 3, 4, 3, 0} 
   >
   > 最优解应该是 3+3， 而不是 4 + 1 + 0, 不能优先选最大的？那怎么办呢？

2. 不能排序，排序会打乱原有相邻关系。

3. 还是考虑**贪心算法**，对于序列(x, y, z)， 如果满足 ：

   1. y >= x + z， 则可以选择y而抛弃x、z。
   2. y < x + z, 且 x 和 z 不相邻， 则抛弃y，继续贪心算法。
   3. x 和 z 相邻，只能选择y，此时可以直接输出y。

4. 如何抛弃？

   1. 置零。将抛弃的选择在数组中的值置为零。

5. 如何处理首尾相邻问题：

   1. 添加判断语句，每次访问时考虑是否为首部或尾部。
   2. 模长除余法(自己瞎起的名字)，对每个下标 x, nums[x]存在，则nums[(x+length-1) % length] 和nums[(i+1)%length]必然存在（就是不会有数组越界问题）。

6. 如何判断贪心算法的结束？

   1. 对每个已选择和已经抛弃的数据，都置为0，数组全零则输出最终结果

7. 代码如下

   ```c++
   class Solution {
   public:
       int rob(vector<int>& nums) {
           int len = nums.size();
           int i = 0; // a Loop control variable
           int max_index = 0;
           int answer = 0;
           bool flag = false; // whether this solution gets the answer
           while(!flag) {
               max_index = 0;
               for(i = 1; i < len; i++) { // find the maximum
                   max_index = (nums[max_index] > nums[i]) ? max_index : i;
               }
               // three situation
               // for (x, y, z)
               if (nums[max_index] >= (nums[(max_index + len - 1) % len] + nums[(max_index + 1) % len])){ // y >= x + z
                   answer += nums[max_index];
                   nums[max_index] = 0;
                   nums[(max_index + len - 1) % len] = 0;
                   nums[(max_index + 1) % len] = 0;
               }else if(3 == len)
                   return nums[1];
               else // y < x + z && x and z are not adjacent
                   nums[max_index] = 0;
   
               // whether gets the answer
               for (i = 0; i < len && !nums[i]; i++)
                   ;
               flag = (i == len) ? 1 : 0;
           }
           //return answer
           return answer;
       }
   };
   
   
   //部分优化
   
   class Solution {
   public:
       int rob(vector<int>& nums) {
           int len = nums.size();
           int i = 0; // a Loop control variable
           int max_index = 0;
           int answer = 0;
           bool flag = false; // whether this solution gets the answer
           while(!flag) {
               max_index = 0;
               for(i = 1; i < len; i++) { // find the maximum
                   max_index = (nums[max_index] > nums[i]) ? max_index : i;
               }
               // three situation
               // for (x, y, z)
               if (nums[max_index] >= (nums[(max_index + len - 1) % len] + nums[(max_index + 1) % len])){ // y >= x + z
                   answer += nums[max_index];
                   nums[max_index] = 0;
                   nums[(max_index + len - 1) % len] = 0;
                   nums[(max_index + 1) % len] = 0;
               }else if(len < 4)
                   return nums[max_index];
               else // y < x + z && x and z are not adjacent
                   nums[max_index] = 0;
   
               // whether gets the answer
               for (i = 0; i < len && !nums[i]; i++)
                   ;
               flag = i == len;
           }
           //return answer
           return answer;
       }
   };
   ```

【自己调试阶段找到的错误】

> 测试样例：nums = {1, 3, 4, 3, 3};
>
> 答案应该是 7 但是输出为6，还是在情况判断上失策了，怎么办呢？
>
> 又感觉贪心走不通了。。。

8. 一种递归的解决方式

   ```c++
   class Solution {
   public:
       int rob(vector<int> nums) { 
           int len = nums.size();
           int max_index = 0;
           int i = 0;
   
           for (i = 0; i < len && !nums[i]; i++)
               ;
           if(i == len)
               return 0;
   
           max_index = 0;
           for(i = 1; i < len; i++) { // find the maximum
               max_index = (nums[max_index] > nums[i]) ? max_index : i;
           }
           if(len < 4)
               return nums[max_index];
           
           int temp = nums[max_index];
   
           nums[max_index] = 0;
           int temp_a = rob(nums);
   
           nums[(max_index + len - 1) % len] = 0;
           nums[(max_index + 1) % len] = 0;
           int temp_b = rob(nums) + temp;
           //return answer
           return temp_a > temp_b ? temp_a : temp_b;
       }
   };
   
   /*
   可惜的是
   这种方法超时了。。
   当数组变长时，对每个最大值都要两次递归
   时间复杂度成指数增长，费时费资
   */
   ```

9. 考虑可能要用动态规划，可是我不会啊。。。
10. 太难了，我是垃圾，写的都是垃圾。

【[**官方题解**](https://leetcode-cn.com/problems/house-robber-ii/solution/da-jia-jie-she-ii-by-leetcode-solution-bwja/)】

## 16-1 [扰乱字符串](https://leetcode-cn.com/problems/scramble-string/)

【[**官方题解**](https://leetcode-cn.com/problems/scramble-string/solution/rao-luan-zi-fu-chuan-by-leetcode-solutio-8r9t/)】

## 16-2 [除自身以外数组的乘积](https://leetcode-cn.com/leetbook/read/top-interview-questions-hard/xw8dz6/)

【初始思路】

1. 不用除法还要再O(n)时间内完成(题目要求)？？

2. 初始化输出数组，其值为输入数组向左移动后的值。

   例：

   输入：[1, 2, 3, 4]

   输出：[2, 3, 4, 1]

3. 然后，左移，数组相乘？
4. 不行，首先，没法左移右移的，这不是队列，其次，也不支持两个数组直接乘。

【有效思路】

1. [yauldmar](https://leetcode-cn.com/leetbook/read/top-interview-questions-hard/xw8dz6/?discussion=2x2zaf)：最直接的想法应该是：初始化结果数组，使得每个位置都保存所有数的乘积，然后每个位置除以nums数组当前位置的值，但这样最大的问题是如果数组存在0，就有问题了，而且题目中明确不允许使用除法，这个思路排除。

2. 那么我考虑将输出数组初始化为全1数组，然后设置一个临时的积temp，初始值为一，遍历数组过程中，每访问一个nums[i]，temp就乘以nums[i]，然后out[i+1]乘以temp，注意到不能越界，因此循环条件是  i < nums.size() - 1。

   [a, b, c, d], [1, 1, 1, 1]

   [1, 1, 1, 1] ==> [1, a, ab, abc]

3. 然后，再将 (2.) 反过来执行就可以得到答案啦！

4. 时间复杂度:O(2n) = O(n)

5. 空间复杂度:O(1)

6. 代码

   ```c++
   // 1.0
   class Solution {
   public:
       vector<int> productExceptSelf(vector<int>& nums) {
           vector<int> ans(nums.size(),1);
           int temp = 1, len = nums.size();
   
           for(int i = 0; i < len - 1; i++){
               temp *= nums[i];
               ans[i + 1] *= temp;
           }
           temp = 1;
           for(int i = len - 1; i > 0 ; i--){
               temp *= nums[i];
               ans[i - 1] *= temp;
           }
           
           return ans;
       }
   };
   
   /*
   执行结果：通过
   执行用时：24 ms, 在所有 C++ 提交中击败了83.25%的用户
   内存消耗：23.3 MB, 在所有 C++ 提交中击败了85.40%的用户
   */
   
   ```
   
   感谢[yauldmar](https://leetcode-cn.com/leetbook/read/top-interview-questions-hard/xw8dz6/?discussion=2x2zaf), 让我发现C、两个for循环语句其实能整合在一起。
   
   ```c++
   //1.1
   class Solution {
   public:
       vector<int> productExceptSelf(vector<int>& nums) {
           vector<int> ans(nums.size(),1);
           int left = 1, right = 1, len = nums.size();
   
           for(int i = 0; i < len - 1; i++){
               left *= nums[i];
               right *= nums[len - 1 - i];
               ans[i + 1] *= left;
               ans[len - i - 2] *= right;
           }
           
           return ans;
       }
   };
   ```

7. 自我思路最终想法

   > 易证，若数组中存在两个为零的元素，则输出数组一定是全零数组。

   ```c++
   //2.0
   #include<algorithm>
   class Solution {
   public:
       vector<int> productExceptSelf(vector<int>& nums) {
           if(count(nums.begin(), nums.end(), 0) > 1)
               return vector<int>(nums.size(), 0);
           
           vector<int> ans(nums.size(),1);
           int left = 1, right = 1, len = nums.size();
   
           for(int i = 0; i < len - 1; i++){
               left *= nums[i];
               right *= nums[len - 1 - i];
               ans[i + 1] *= left;
               ans[len - i - 2] *= right;
           }
           
           return ans;
       }
   };
   /*
   参考评价
   执行结果：通过
   执行用时：20 ms, 在所有 C++ 提交中击败了94.43%的用户
   内存消耗：23.3 MB, 在所有 C++ 提交中击败了90.47%
   的用户
   */
   ```

【[**官方题解**](https://leetcode-cn.com/problems/product-of-array-except-self/solution/chu-zi-shen-yi-wai-shu-zu-de-cheng-ji-by-leetcode-/)】【[**我的题解**](https://leetcode-cn.com/problems/product-of-array-except-self/solution/li-qiu-xiao-zhu-chu-zi-shen-yi-wai-shu-z-n6l5/)】

## 17 [存在重复元素 III](https://leetcode-cn.com/problems/contains-duplicate-iii/)

【初始思路】

1. 遍历数组加个约束条件？时间复杂度为O(n^2)。

   ```c++
   class Solution {
   public:
       bool containsNearbyAlmostDuplicate(vector<int>& nums, int k, int t) {
   
           for(int i = 0; i < nums.size() ;i++)
               for(int j = i + 1; j < i + k + 1 && j < nums.size(); j++){
                   if(abs(nums[i] - nums[j]) <= t)
                       return true;
               }
           return false;
       }
   };
   ```

2. > 第一，没注意分析数据范围，int不够长
   >
   > [-2147483648,2147483647]
   > 1
   > 1
   >
   > 测试样例不通过
   >
   > 改版
   >
   > ```c++
   > class Solution {
   > public:
   >     bool containsNearbyAlmostDuplicate(vector<int>& nums, int k, int t) {
   > 
   >         for(int i = 0; i < nums.size() ;i++)
   >             for(int j = i + 1; j < i + k + 1 && j < nums.size(); j++){
   >                 if(abs((long long int)nums[i] - (long long int)nums[j]) <= t)
   >                     return true;
   >             }
   >         return false;
   >     }
   > };
   > ```
   >
   > 还是错误的，没注意数组长度限制，O(n^2) 超时。

3. 考虑另外一种算法，先排序，然后看前两个元素就行了。
4. 不行！绝对不行，排序会打乱原有的数组序，不符合题意要求。


【[**官方题解**](https://leetcode-cn.com/problems/contains-duplicate-iii/solution/cun-zai-zhong-fu-yuan-su-iii-by-leetcode-bbkt/)】

## 18 [删除有序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)

【思路】

1. 此题为06日做的题的前身版本，我的思路还是和删除字符串中重复的一样。

【双指针】

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if(nums.size() < 2) // 题面有说数组可能为空
            return nums.size(); 

        int j = 0;
        nums.push_back(10001); // 插入一个数据范围之外的数据，用作最后的 i + 1
        for(int i = 0; i < nums.size() - 1; i++)
            if(nums[i] != nums[i+1])
                nums[j++] = nums[i];
        return j;
    }
};
```

【[**官方题解**](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/solution/shan-chu-pai-xu-shu-zu-zhong-de-zhong-fu-tudo/)】

【总结心得】

- 多多注意题面信息，题目往往都不会错，好好理解题目意思，观察数据范围。

## 19 [移除元素](https://leetcode-cn.com/problems/remove-element/)

【双指针】

【代码】

```c++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int index = 0;
        if(nums.size()){ // 数组有可能为空因此要判断一下
            for(int i = 0; i < nums.size(); ++i)
                if(nums[i] != val)
                    nums[index++] = nums[i];
        }else
            return 0;
        return index;
    }
};

//改进
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int index = 0;
        //数组空不满足for的条件语句
        for(int i = 0; i < nums.size(); ++i)
            if(nums[i] != val)
                nums[index++] = nums[i];
        
        return index;
    }
};

/* 参考评价
执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户
内存消耗：8.3 MB, 在所有 C++ 提交中击败了99.26%的用户
*/
```

【[**官方题解**](https://leetcode-cn.com/problems/remove-element/solution/yi-chu-yuan-su-by-leetcode-solution-svxi/)】