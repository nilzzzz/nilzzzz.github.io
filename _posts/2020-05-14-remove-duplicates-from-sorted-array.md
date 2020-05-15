---
layout: post
title: 26. 删除排序数组中的重复项及相似题目解析
tag: Java
---

### 26.删除排序数组中的重复项

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

题目描述：

给定一个排序数组，你需要在 原地 删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。

> 示例 1:
>
> 给定数组 nums = [1,1,2], 
>
> 函数应该返回新的长度 2, 并且原数组 nums 的前两个元素被修改为 1, 2。 
>
> 你不需要考虑数组中超出新长度后面的元素。



> 示例 2:
>
> 给定 nums = [0,0,1,1,1,2,2,3,3,4],
>
> 函数应该返回新的长度 5, 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4。
>
> 你不需要考虑数组中超出新长度后面的元素。
>

>
> 说明:
>
> 为什么返回数值是整数，但输出的答案是数组呢?
>
> 请注意，输入数组是以「引用」方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。
>
> 你可以想象内部操作如下:
>
> // nums 是以“引用”方式传递的。也就是说，不对实参做任何拷贝
> int len = removeDuplicates(nums);
>
> // 在函数里修改输入数组对于调用者是可见的。
> // 根据你的函数返回的长度, 它会打印出数组中该长度范围内的所有元素。
> for (int i = 0; i < len; i++) {
>     print(nums[i]);
> }

#### 方法：双指针法

假设输入数组为 [1、2、2、6]  那么图像演示过程如下:

![image-20200515092125060](https://gitee.com/XiaoShenKeHeBen/Static/raw/master/image/202005/15/092126-539479.png)

以上我们可以得到j及之前为非重复元素

整个过程纯语言表述如下: 数组完成排序后（这道题是已排序数组），我们可以放置两个指针 i 和 j，其中 i 是慢指针，而 j 是快指针。

其中

1. i及之前的元素为非重复项
2. j及之后为待重复检测的项

用j遍历数组, 如果 nums[i] = nums[j]，当前j不会搬移到非重复项区域

当我们遇到 nums[j] != nums[i]时，我们需要

1. 递增 i (扩充非重复项数组区域) 
2. 并将nums[j]的值复制到 nums[i] 

然后，接着我们将再次重复相同的过程，直到 j 到达数组的末尾为止。

```
public int removeDuplicates(int[] nums) {
    if (nums.length == 0) return 0;
    int i = 0;
    for (int j = 1; j < nums.length; j++) {
        if (nums[j] != nums[i]) {
            //i++;
            //nums[i] = nums[j];
            nums[++i]=nums[j];
        }
    }
    return i + 1;
}
```

类似题目：

#### [80. 删除排序数组中的重复项 II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array-ii/)

题目描述：

给定一个排序数组，你需要在原地删除重复出现的元素，使得每个元素最多出现两次，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。

示例 :

> 给定 nums = [1,1,1,2,2,3],
>
> 函数应返回新长度 length = 5, 并且原数组的前五个元素被修改为 1, 1, 2, 2, 3 。
>
> 你不需要考虑数组中超出新长度后面的元素。
>

在‘26. 删除排序数组中的重复项’的基础上，同样采用双指针法

```
class Solution {
    public int removeDuplicates(int[] nums) {
        if(nums.length<=1) return nums.length;
        int index=2;
        for(int i=2;i<nums.length;i++){
            if(nums[i]!=nums[index-2]){
                nums[index++]=nums[i];
            }
        }
        return index;
    }
}
```

再进一步，如果最多出现k次那？

![image-20200515095100326](https://gitee.com/XiaoShenKeHeBen/Static/raw/master/image/202005/15/095102-347454.png)

相似题目：

#### [11. 盛最多水的容器](https://leetcode-cn.com/problems/container-with-most-water/)

题目描述：

给你 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0). 找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

说明：你不能倾斜容器，且 n 的值至少为 2。

![image-20200515095534679](https://gitee.com/XiaoShenKeHeBen/Static/raw/master/image/202005/15/095534-347428.png)

图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。

 **示例：**

```
输入：[1,8,6,2,5,4,8,3,7]
输出：49
```

#### 双指针法

##### 思路：

使用第一列和最后一列, 可以构建宽度最宽的容器, 它的水位是第一行和最后一行中较小的一个的高度。

所有其他容器的宽度会更小, 又因为需要较高的水位才能容纳更多的水, 只能通过提高水位线来获得, 一开始两个指针一个指向开头一个指向结尾，此时容器的底是最大的，接下来随着指针向内移动，会造成容器的底变小，在这种情况下想要让容器盛水变多，就只有在容器的高上下功夫。 那我们该如何决策哪个指针移动呢？我们能够发现不管是左指针向右移动一位，还是右指针向左移动一位，容器的底都是一样的，都比原来减少了 1。这种情况下我们想要让指针移动后的容器面积增大，就要使移动后的容器的高尽量大，所以我们选择指针所指的高较小的那个指针进行移动，这样我们就保留了容器较高的那条边，放弃了较小的那条边，以获得有更高的边的机会。

1. 左右指针初始化为数组的左右边界
2. 计算左右指针对应的面积：最小高度 * 两指针距离, 并更新最大面积
3. 移动较小指针
4. 重复2,3 

最终可以得到最大面积maxArea;

```
public int maxArea(int[] height) {
    int left = 0, right = height.length - 1;
	int maxArea = 0;

	while (left < right) {
		maxArea = Math.max(maxArea, Math.min(height[left], height[right]) * (right - left));
		if (height[left] < height[right])
			left++;
		else
			right--;
	}

	return maxArea;
}
```

