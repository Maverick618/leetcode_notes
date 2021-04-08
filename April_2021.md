# My Leetcode Notes  __  April 2021

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

1. 我能不能直接找最小值啊，vector的方法QAQ。
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