---
title: 剑指offer javascript part1(1-10题)
date: 2017-03-29 17:29:25
categories: 编程题
tags: 剑指offer
---

忒忒开始看算法了！

### 1. 二维数组中的查找
**描述**    
在一个二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

**代码**
-  暴力方式，全部遍历
<!--more-->
```javascript
function Find(target, array)
{
    for(var i=0;i<array.length;i++){
        for(var j=0;j<array[i].length;j++){
            if(array[i][j]==target)
               return true;
        }
    }
    return false;
}
```
- 利用二维数组由上到下，由左到右递增的规律，那么选取右上角或者左下角的元素a[row][col]与target进行比较，
当target小于元素a[row][col]时，那么target必定在元素a所在行的左边,即col--；
当target大于元素a[row][col]时，那么target必定在元素a所在列的下边,即row++；
```javascript
function Find(target, array){
    var row = 0;
    var col = array[0].length-1;
    while(row<array.length && col>=0){
        if(target == array[row][col]){
            return true;
        }else if(target > array[row][col]){
            row++;
        }else{
            col--;
        }
    }
    return false;
}```

### 2. 替换空格
**描述**    
请实现一个函数，将一个字符串中的空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。

**代码**
-使用replace（估计题目不是考这个）
```javascript
function replaceSpace(str)
{
    return  str.replace(/\s/g,'%20');
}
```
-使用split + join
```javascript
function replaceSpace(str)
{
    return str.split(' ').join('%20');
}
```

### 3. 从尾到头打印链表
**描述**    
输入一个链表，从尾到头打印链表每个节点的值。

**代码**
```javascript
/*function ListNode(x){
    this.val = x;
    this.next = null;
}*/
function printListFromTailToHead(head)
{
    // write code here
    var arr = [];
    var p = head;
    while(p){
        arr.push(p.val);
        p = p.next;
    }
    return arr.reverse();
}
```

### 4. 重建二叉树
**描述**    
输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。

**代码**
```javascript
/* function TreeNode(x) {
    this.val = x;
    this.left = null;
    this.right = null;
} */
function reConstructBinaryTree(pre, vin)
{
    if(pre.length === 0)return;
    var node = {
        val:pre[0]
    };
    for(var i=0;i<vin.length;i++){
        if(pre[0] == vin[i]){
            node.left = reConstructBinaryTree(pre.slice(1,i+1),vin.slice(0,i));
            node.right = reConstructBinaryTree(pre.slice(i+1),vin.slice(i+1));
        }
    }
    return node;
}
```

### 5. 用两个栈实现队列
**描述**    
用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。

[基本思路](http://www.cnblogs.com/wanghui9072229/archive/2011/11/22/2259391.html)：
入队时，将元素压入s1。
出队时，判断s2是否为空，如不为空，则直接弹出顶元素；如为空，则将s1的元素逐个“倒入”s2，把最后一个元素弹出并出队。

**代码**
```javascript
function Stack(){
    var arr = [];
    this.push = function(node){
        arr.push(node);
        return arr;
    };
    this.pop = function(){
        return arr.pop(); 
    };
    this.isEmpty = function(){
        return arr.length===0;
    }
}
//定义两个栈
var stack1 = new Stack();
var stack2 = new Stack();

function push(node)
{
    stack1.push(node);
}
function pop()
{
    if(stack2.isEmpty()){
        while(!stack1.isEmpty()){
            stack2.push(stack1.pop());
        }
    }
    return stack2.pop();
}
```

### 6. 旋转数组的最小数字
**描述**    
把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。
输入一个非递减排序的数组的一个旋转，输出旋转数组的最小元素。例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。
NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。

基本思路：
利用二分查找，以最小值为界，左右分为两部分，左右边均不小于于最小值。
中间元素若大于最后一个元素，则最小值位于右半边；
中间元素若小于最后一个元素，则最小值位于左半边（含中间元素）。

**代码**
```javascript
function minNumberInRotateArray(rotateArray)
{
    var low = 0;
    var high = rotateArray.length-1;
    
    while(low<high){
        var mid = low + Math.floor((high-low)/2);
        if(rotateArray[mid] > rotateArray[high]){
            low = mid + 1;
        }else if(rotateArray[mid] < rotateArray[high]){
            high = mid;
        }else{
            high = high-1;
        }
    }
    return rotateArray[low];
}
```

### 7. 斐波那契数列
**描述**    
大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项。
n<=39

分析：
经[测试](http://www.jianshu.com/p/0b32ce736c24)，如果用递归的写法会超出时间限制的

**代码** 
```javascript
function Fibonacci(n)
{
    var aaa = [];
    aaa[0] = 0;
    aaa[1] = 1;
    for(var i=2;i<=n;i++){
        aaa[i] = aaa[i-2]+aaa[i-1];
    }
    return aaa[n];
}
```

### 8. 跳台阶
**描述**    
一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法。

**分析**
对于本题,前提只有 一次 1阶或者2阶的跳法。
a. 如果两种跳法，1阶或者2阶，那么假定第一次跳的是一阶，那么剩下的是n-1个台阶，跳法是f(n-1);
b. 假定第一次跳的是2阶，那么剩下的是n-2个台阶，跳法是f(n-2)
c. 由a&b假设可以得出总跳法为: f(n) = f(n-1) + f(n-2) 
d. 然后通过实际的情况可以得出：只有一阶的时候 f(1) = 1 ,只有两阶的时候可以有 f(2) = 2
e. 可以发现最终得出的是一个斐波那契数列：
```         
           | 1, (n=1)
f(n) =     | 2, (n=2)
           | f(n-1)+f(n-2) ,(n>2,n为整数)
```

**代码** 
```javascript
function jumpFloor(number)
{   
    var arr = [];
    if(number === 0){
        return -1
    }
    arr.push(1);
    arr.push(2);
    for(var i = 2; i < number; i++){
        arr.push(arr[i - 1] + arr[i - 2])
    }
    return arr[number - 1]
}
```

### 9. 变态跳台阶
**描述**    
一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。

- **思路分析1**
因为n级台阶，
第一步有n种跳法：跳1级、跳2级、到跳n级
跳1级，剩下n-1级，则剩下跳法是f(n-1)
跳2级，剩下n-2级，则剩下跳法是f(n-2)
所以f(n)=f(n-1)+f(n-2)+...+f(1)，因为f(n-1)=f(n-2)+f(n-3)+...+f(1)，所以f(n)=2*f(n-1)

  **代码** 
  ```javascript
  function jumpFloorII(number)
  {
    var arr = [];
    if(number === 0){
        return -1
    }
    arr.push(1);
    arr.push(2);
    for(var i = 2; i < number; i++){
        arr.push(2*arr[i-1]);
    }
    return arr[number - 1]
    
  }
  ```
- **思路分析2**
每个台阶都有跳与不跳两种情况（除了最后一个台阶），最后一个台阶必须跳。所以共用2^(n-1)中情况（这个是看的大神解答，感觉略牛逼啊）
  ```
  function jumpFloorII(number)
  {
    return number < 0 ? -1 : Math.pow(2,number-1);
  }
  ```
  或者更简洁的写法
  ```
  function jumpFloorII(number)
  {
    return  1<<--number;
  }
  ```

### 10. 矩形覆盖
**描述**    
我们可以用2*1的小矩形横着或者竖着去覆盖更大的矩形。请问用n个2*1的小矩形无重叠地覆盖一个2*n的大矩形，总共有多少种方法？

**思路分析**
仍然是斐波那切数列，有以下几种情形：
a. number <= 0 直接return 0；
b. number = 1大矩形为2\*1，只有一种摆放方法，return 1；
c. number = 2 大矩形为2\*2，有两种摆放方法，return 2；
d. number = n 分为两步考虑：
        1). 第一次摆放一块 2\*1 (竖着)的小矩阵，则摆放方法总共为f(number - 1);
        2). 第一次摆放一块1\*2(横着)的小矩阵，则摆放方法总共为f(number-2)
              因为，摆放了一块1\*2的小矩阵，对应下方的1\*2摆放方法就确定了，所以为f(number-2)

  **代码** 
```javascript
function rectCover(number)
{
  var arr = [];
  if(number === 0){
      return 0
  }
  arr.push(1);
  arr.push(2);
  for(var i = 2; i < number; i++){
      arr.push(arr[i-1]+arr[i-2]);
  }
  return arr[number - 1]
}
```

<center>**未完待续...**</center>
