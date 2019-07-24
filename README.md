# exercise-day
This file records my daily exercises.

##算法练习：

####**7.22 算法题：数组去重**  
目前一共有两种方法来去重：  
**1.**先创建一个新的数组，使用for循环语句和indexOf方法来测试传入的方法中的元素是否已经在新数组中了，如果有就不需要push进去，如果没有就push进去。  

**2.**先创建一个新的数组，并且，把传入来的数组先排序，先把传入的数组的第一个元素值push进去新数组中，再使用for循环，对比arr[i]是不是和新数组中的最后一个元素相同（因为传入的元素是已经排序过的，所以是让arr[i]和新数组中的最后一个元素进行比较）


####**7.23 算法题：实现阶乘** （使用递归）  
阶乘即所有小于及等于该数的正整数的积，并且0的阶乘为1。自然数n的阶乘写作n!。  
判断传入的数值是否小于2，若小于2，则返回1，若不是，则返回再次调用该函数，但是是num-1作为参数传入再乘以num  
*注意！！！！！！*  
*在写阶乘的函数过程中，要把num*函数名（num-1）改成——>arguments.callee（num-1）是为了降低耦合性*  

####**7.24 算法题：判断一个字符串是否是回文**  
回文即在中文文当中是指倒着念和顺着念都是相同的，前后对称，例如“上海自来水来自海上”；在英文文当中是指正着看和反着看都相同的单词，例如“madam”  （该算法题中不把有数字和有符号的情况算进去）
该算法题使用了三种方法解决：    

**1.**  把字符串反转即前后顺序颠倒，把新的字符串和传入的字符串做对比；  
**2.**  获取字符串的长度，使用for循环，分别比对字符串的第一个和最后一个字符，第二个和倒数第二个字符，第三个和倒数第三个字符...以此类推；  
**3.**  第三个方法和第二个方法差不多，把字符串转化成数组，使用for循环，分别比对数组的第一个和最后一个元素，第二个和倒数第二个元素，第三个和倒数第三个元素...以此类推  
  



