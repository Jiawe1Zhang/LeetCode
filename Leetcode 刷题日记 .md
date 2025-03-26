# Leetcode 刷题日记

## 3.5 (俩数之和)

俩数之和, 哈希表, 没想到的是, 哈希表在python里就是字典

这个B站的可视化动画讲的超级明白🫡

[散列表(哈希表) - 散列函数, 冲突处理, 平均查找长度(ASL)_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV13NwveLE1D?spm_id_from=333.788.videopod.sections&vd_source=fab1ef87b734b1a34847f798d42e2604)

```python
# 创建空字典, 我一度写出 haxi = dict{} 😂, 记得放在循环外 !!!
haxi = {} 

# 往字典里加入 键值对
haxi[键] = 值    (俩数之和, 键是数字本身, 值是对应下标)

# 只需要遍历一遍给定数组, 时间复杂度O(n)
# 空间复杂度：O(n)。哈希表需要 O(n) 的空间。

# python的字典查找应该底层就是哈希表实现的, 从而实现快速查找, 插入和删除
# (找一个数用哈希表 -> 查找需求)

if target - nums[i] not in haxi: 
# 这里字典可以直接支持查找是否在字典里是否有某个键(底层就是哈希表实现)
		haxi[nums[i]] = i
    continue # 跳出本次循环的遍历, 直接进入下一次)
```

> **哈希表的核心特性**：
> 
> - **键值对存储**：哈希表通过键（key）来存储和查找值（value）。
> - **哈希函数**：哈希函数将键映射到一个固定范围的索引，用于确定值的存储位置。
> - **冲突解决**：当不同的键映射到同一个索引时，需要通过冲突解决方法（如链地址法或开放寻址法）来处理。

---

## 3.6 (没刷)

- 太累了, 今日没刷

---

## 3.7 (字母异位词 & 最长连续序列)

- **`sorted()`** 是一个内置函数，它接受一个可迭代对象（如字符串、列表等），并返回一个包含该可迭代对象元素的排序后列表, sorted()
- **`join()`** 是字符串的一个方法，它将一个可迭代对象中的元素连接成一个字符串。`join()` 的语法是 `str.join(iterable)`，其中 `str` 是连接元素时使用的分隔符，`iterable` 是要连接的元素列表。在 `''.join(sorted(s))` 中，分隔符是空字符串 `''`，这意味着排序后的字符将被直接连接在一起，没有任何分隔符.
- 字母异位词
    
    ```python
    for i in haxi:
        print(i)# 输出的是字典的键
    
    # 返回字典的值，不使用 list() 转换
    values_view = haxi.values()
    print(values_view)  
    # 输出: dict_values([('aet', ['eat', 'tea', 'ate']), ('ant', ['tan', 'nat']), ('abt', ['bat'])])   
        
    for value in haxi.values():
        print(value)  # 输出的是字典的值
        
    # ['eat', 'tea', 'ate']
    # ['tan', 'nat']
    # ['bat'] 
    
    values_list = list(haxi.values())
    print(values_list)  # 输出: [['eat', 'tea', 'ate'], ['tan', 'nat'], ['bat']]
     
    for key, value in haxi.items():
        print(f"键: {key}, 值: {value}")  # 输出键和对应的值
        
        
    haxi = {
        'aet': ['eat', 'tea', 'ate'],
        'ant': ['tan', 'nat'],
        'abt': ['bat']
    }
    ```
    
- 最长连续序列
    
    ```python
        # 我个人认为非常优雅的解决最长连续序列问题的solution, very beautiful!
        # 另外一种标准解, 是遍历一次, 找当前数的数-1, 如果exist, 就马上下一个, 这里都是O(1)的时间复杂度, 说白了, 就是只有 数字-1 后面没有的, 那个才能作为max连续序列的start, 然后再开始一步步查找下一个, 得到一个长度, 存在max里, 然后可以做到类似O(1) + O(1) + ... + O(n), 决定不会是O(n^2)
        s=set(nums)
        ans = 0
        while s:
            t=1
            #随便从集合中拿出一个
            b=s.pop()
            c=b+1
            a=b-1
            #一直向右探测，有t 就加1 ，然后从集合中去除它，集合变小
            while c in s:
                t+=1
                s.remove(c)
                c+=1
            #向左探测
            while a in s:
                t+=1
                s.remove(a)
                a-=1
    
            if t>ans:
                ans=t
    
        return ans
    ```
    

---

## 3.8 (移动零+盛最多水容器)

- 移动零, 呃快慢指针, 交换零和非零, 差不多这样的思路
- 最多水
    - 最大存水, 思路是1. 面积是俩段的min高度* 宽度; 那么每次宽度的减少(指针的移动), 如果高度不能找到比之前更高的, 那么面积肯定比原先的小; 所以每次移动都是移动对应指针 高度小的, why? because, 你移动高度高的, 就算情况好, 高了, 你的面积还是受限于min边, 宽度减少, min高度不变, 面积铁变小, 所以必须移动俩段的min边, 找到高度增加的, 停下来, 算面积, 比较面积, 只有比原先高, 面积才有可能大; 毕竟宽度都不断减小了, 高度如果还比原先小是肯定不行的
    
    ```python
    class Solution:
        def maxArea(self, height: List[int]) -> int:
            i = 0 
            j = len(height) -1
            s =  (j-i) * min(height[i], height[j])
            tp = 0 
            while i < j:
                if height[i] <= height[j]:
                    while height[i] >= height[i+1] and i+1<j:
    		                 i+=1 
                     # 这里注意室i+1 <j, 不要写i< j
                     # 因为左边是 if i到了 i+1 就是j 的话, 说明宽度只有一格了, 
                     # i也不用去找比自己高的下一个高度了(就已经到j了, 下一个就是j,
                     # 所以跳出while, 然后i+=1,  
                     # 说明找不到更大的高度了来增加面积了; 
                     # 这样让外面大while, 也跳出去了,也就可以结束了      
                    i+=1
                else:
                    while height[j] >= height[j-1] and i+1<j:
                        j-=1   
                    j-=1
                    
    	          tp = (j-i) * min(height[i], height[j])
                s = max (s, tp)
                
            return s
    ```
    

---

## 3.9 (三数和)

- 起来就来一道力扣题, 没想到正好到三数和
    
    **三数之和**是一个经典的算法问题，通常描述为：给定一个包含 `n` 个整数的数组 `nums`，判断其中是否存在三个元素 `a, b, c`，使得 `a + b + c = 0`？找出所有满足条件且不重复的三元组。
    
    以下是解决该问题的详细思路和代码实现：
    
    ---
    
    ### 1. **问题分析**
    
    - **输入**：一个整数数组 `nums`。
    - **输出**：所有满足 `a + b + c = 0` 且不重复的三元组 `[a, b, c]`。
    - **要求**：
        - 三元组不能重复（例如 `[-1, 0, 1]` 和 `[0, 1, -1]` 是重复的）。
        - 时间复杂度尽量低。
    
    ---
    
    ### 2. **解题思路**
    
    ### (1) **暴力解法（不推荐）**
    
    - 使用三重循环枚举所有可能的三元组，检查是否满足 `a + b + c = 0`。
    - 时间复杂度：`O(n^3)`，效率太低。
    
    ### (2) **排序 + 双指针（推荐）**
    
    - **步骤**：
        1. **排序**：将数组排序，方便后续操作。
        2. **固定一个数**：遍历数组，固定一个数 `nums[i]` 作为三元组的第一个数。
        3. **双指针查找**：
            - 使用双指针 `left` 和 `right`，分别指向 `i+1` 和 `n-1`。
            - 计算 `sum = nums[i] + nums[left] + nums[right]`。
            - 如果 `sum == 0`，记录三元组，并移动指针去重。
            - 如果 `sum < 0`，说明需要更大的数，`left++`。
            - 如果 `sum > 0`，说明需要更小的数，`right--`。
        4. **去重**：
            - 在遍历过程中，跳过重复的 `nums[i]`、`nums[left]` 和 `nums[right]`。
    - **时间复杂度**：`O(n^2)`（排序 `O(nlogn)` + 双指针 `O(n^2)`）。
    
    ---
    
    ### 3. **代码实现**
    
    以下是 Python 实现：
    
    python
    
    复制
    
    ```python
    def threeSum(nums):
        # 结果列表
        result = []
        n = len(nums)
        # 先对数组排序
        nums.sort()
    
        # 遍历数组，固定第一个数
        for i in range(n - 2):
            # 跳过重复的 nums[i]
            if i > 0 and nums[i] == nums[i - 1]:
                continue
            # 双指针初始化
            left, right = i + 1, n - 1
            while left < right:
                total = nums[i] + nums[left] + nums[right]
                if total == 0:
                    # 找到满足条件的三元组
                    result.append([nums[i], nums[left], nums[right]])
                    # 跳过重复的 nums[left] 和 nums[right]
                    while left < right and nums[left] == nums[left + 1]:
                        left += 1
                    while left < right and nums[right] == nums[right - 1]:
                        right -= 1
                    # 移动指针
                    left += 1
                    right -= 1
                elif total < 0:
                    # 需要更大的数
                    left += 1
                else:
                    # 需要更小的数
                    right -= 1
        return result
    ```
    
    ```python
    # 我自己再力扣上刷到, 写的
    class Solution:
        def threeSum(self, nums: List[int]) -> List[List[int]]:
            nums = sorted(nums)
            pivot =0
            ans = []
            while nums[pivot] <=0 and pivot <= len(nums)-3:
                if nums[pivot] == nums[pivot-1] and pivot >0:   # 这里没想到
                    pivot+=1
                    continue
    
                left = pivot +1
                right = len(nums) -1
                while left < right:
                    total = nums[pivot] + nums[left] + nums[right]
                    if total < 0:
                        left +=1
                    if total > 0:
                        right -=1
                    if total == 0:
                        c = [nums[pivot], nums[left], nums[right]]
                        while nums[left+1] == nums[left] and left+1 < right:
                        # 这里的left+1 < right 
                            left +=1
                        while nums[right-1] == nums[right] and left+1 < right:
                            right -=1
    
                        left +=1
                        right -=1
                        ans.append(c)
                        
                pivot +=1 # 这里忘记写了
            return ans
    ```
    

---

## 3.10 (接雨水)

- 思路没什么问题, 还是python 的一些基本操作不熟悉, `for i in range(len(height)-2, 0,-1):` , 注意python 默认步长为1
    
    ```python
    class Solution:
        def trap(self, height: List[int]) -> int:
            tmp = 0
            #一开始我还不知道如何创建与 给定数组长度相同的零数组, 
            # 傻傻的写了一个Left = []
            # 后面继续Left[下标] -> 直接报错
            Left = [0] * len(height)
            
            for i in range(1, len(height)-1):
                tmp = max(tmp, height[i-1])
                Left[i] = tmp
            
            tmp = 0
            Right = [0] * len(height)
            # 一开始我写的是range(len(height)-2, 0), python 默认步长为1 ..., 没写-1 
            # 等于什么都没执行
            for i in range(len(height)-2, 0,-1):
                tmp = max(tmp, height[i+1])
                Right[i] = tmp   
    
            ans =0
            for i in range(1, len(height)-1):
                if min(Left[i], Right[i]) - height[i] >0: 
                    ans += min(Left[i], Right[i]) - height[i]
    
            return ans
    
    ```
    

---

## 3.11 (有效果的括号 + 最小栈)

- 使用数组实现的栈称为顺序栈。在顺序栈中，栈底通常固定在数组的一端，栈顶指针指示下一个可用位置。当栈顶指针达到数组的末尾时，栈满，无法再进行入栈操作
- 有效果的括号
    
    ```python
    class Solution:
        def isValid(self, s: str) -> bool:
            dic = {")":"(","]":"[","}":"{"}
            stack = []
            for i in s:
                if i in dic:
                    if stack:
                        if stack[-1] == dic[i]:
                        #敲掉最后一个元素, 相当于出栈
                            stack.pop()
                        else:
                            return False
                    else:
                        return False
                else:
                    stack.append(i)
            
            return not stack
            #首先明白在Python中，
            # None、False、空字符串""、0、空列表[]、空字典{}、空元组()都相当于False
            # 写return stack , 返回的是整个数组了
            
    ```
    
- 最小栈
    
    看到评论说, 字节一面要求用O(1)的辅助空间, 不愧是算法厂, 常用的解法是维护一条栈, 辅助空间复杂度是O(n), 如果能去维护一个栈, 每次push进一个数字, 另外一个栈同步push最新的min, 所以同步的有一个栈 每个位置对应记录原栈的在此位置是top 时候的整体min, 这样可以做到getMin O(1)时间复杂度
    
    有没有什么办法, 能将整个O(n)空间的栈, 去掉
    很巧妙的方法是, 栈里的每个元素记录的是,加进去的时候, 和min 的比较, 统一为 val - min
    
    如果是<0,  就把原来的min update成加入的元素, 每次出栈, if>0,直接出, 对min 不影响, if<0, 把min modify回 没加入前的(min - stack[-1]),
    
    这样就能做到只用额外O(1) 的辅助空间复杂度了! 因为栈里面存的不是自己, 而是和min 的关系, 省掉了另外开一个栈去维护n 个 最小值的空间
    
    ```python
    class MinStack:
    
        def __init__(self):
            self.stack = []  
            # if 要求辅助空间O(1), 要去维护
            # new - 原min = 要存的数
            self.min = None
    
        def push(self, val: int) -> None:
            if self.stack:
                self.stack.append(val - self.min)
                if val < self.min:
                    self.min = val
            else:
    	        # 注意对空栈的操作!!! 之前因为这个地方通过
                self.stack.append(0)
                self.min = val
            
        def pop(self) -> None:
            if self.stack[-1] <0:
                self.min = self.min - self.stack[-1]
            self.stack.pop()
            
    
        def top(self) -> int:
            if self.stack[-1] < 0:
                return self.min
            else:
                return self.stack[-1] + self.min
    
        def getMin(self) -> int:
            return self.min
    ```
    

---

## 3.12 (字符串解码)

- 列表 [1, 2, 3] → “123” , 不能用str(), 用str  的结果是 “[1, 2, 3]”  😂
- `str.isdigit()` 方法用于检查字符串中的字符是否都是数字字符。对于单个字符的字符串，它可以用来判断该字符是否是数字。
    
    ```python
    s = "a1b2c3"
    for char in s:
        if char.isdigit():
            print(f"'{char}' 是数字")
        else:
            print(f"'{char}' 不是数字")
    ```
    
    ```
    'a' 不是数字
    '1' 是数字
    'b' 不是数字
    '2' 是数字
    'c' 不是数字
    '3' 是数字
    ```
    
- 字符串解码, 看见括号有关的这种, 优先想到栈, 借鉴了题解, 还是参考了别人的code; 然后就是这里的一些list()的用法, + 直接拼接, 然后那个 `while stack and stack[-1].isdigit():` , 这个地方很耐人寻味, 你顺序反一下是会报 list index out of range 这个错的, 看起来还是有先后执行的顺序逻辑, 然后这个stack 很容易忘掉, 因为数字往前走很可能直接空了, 没东西了, 所以要加个stack 本身是否是空的判断
    
    ```python
    class Solution:
        def decodeString(self, s: str) -> str:
            stack = []
            for i in s:
                if i != "]":
                    stack.append(i)
                else:
                    word = []
                    while stack[-1]!="[":
                        word = list(stack[-1]) + word
                        stack.pop()
                    # 这里的这个stack.pop()很容易忘
                    stack.pop()
    
                    num = ""
                    while stack and stack[-1].isdigit():
                        num = stack[-1] + num
                        stack.pop()
    
                    
                    num = int(num)
    
                    stack = stack + num * word
    
            
            # 这里一度写出 return str(stack)
            return ''.join(stack)
    ```
    

---

## 3.13 (每日温度)

- `stack.pop()` 是一个同时具有返回值和副作用的操作：它返回栈顶元素的值，并且修改了原栈的结构
- 如果你想要同时遍历数组的下标和内容，可以使用 `enumerate()` 函数。`enumerate()` 函数会返回一个枚举对象，其中每个元素是一个元组，包含一个计数器（下标）和一个对应的数组元素
- 每日温度, 维护一个单调栈, 只递减, 有大的进来, 就一个个把比它小的出栈; 在单调栈基础题中，经常需要类似这种的解题思路：在 *O*(*n*) 的时间复杂度内求出数组中各个元素右侧第一个更大的元素及其下标，然后一并得到其他信息;   然后, 我疑惑了很久为什么这个内层循环的时间复杂度严格遵行, O(n)次, 对于每个元素, 
每次出栈的时候, 都一定是栈顶, 说白了不在栈顶不被比较, 就算在栈顶被比较没办法出栈, 这个时间是另外一个元素用来入栈的时间;
入栈, 我们知道入栈可能需要比较很多次, 才能找到入栈的位置, 确定入栈了, 就是算作自己的时间, 算1; 没找到入栈, 那就是当前栈顶需要出栈, 算作别人出栈的时间;

我之前想不通的就是, 对于每个元素它可能需要比较很多次才可以入栈, 入栈后, 比较很多次才可以出栈, 现在我理解了: 入不了栈时候的比较 → 别人出栈一次嘛, 不算我自己; 出不了栈 → 每个不同新人的入栈一次嘛

在这俩中时间情况下, 我们可以发现, 对于每个元素, 入栈一次常数时间, 出栈一次常数时间. (这就是单调栈 时间复杂度的魅力?)
    
    ```python
    class Solution:
        def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
            stack =[]
            day = [0] * len(temperatures) 
            # stack.append(temperatures[0])
            # day = [0]
            # 最开始可以用 while stack 来代替了增加的过程
            for i, value in enumerate(temperatures):
    		        # 还是 stack 要写在while 的最前面
                while stack and value > temperatures[stack[-1]]:
    		            # 比栈顶大, 栈顶出栈, 计算题目要求的天数
                    j = stack.pop()
                    # 第几天 - 第几天, 也就是在第几天后, 第一次出现温度升高
                    day[j] = i-j
                # 单调栈记录的是, 第几天(下标)
                stack.append(i)
        
            return day
    ```
    

---

## 3.14 (开始链表, 看了下相交链表的题解)

- 题解的思路确实牛逼, 俩条不同长的, 走 a+b+c 的长度一定能找到相交点; 没找到就是俩个指针都是None

## 3.15 (杭州happy, 没刷)

## 3.16 (玩完回学校太累了, 没刷)

---

## 3.17 (相交链表)

- 看leetcode, 一直没看懂为什么链表的定义到底具体是怎么样的; 直到看到评论区的一个comment: 这是题目初始化链表的方式，难点就是这个，别纠结初始化链表后的内存地址相等，题目的意思是初始化的时候就已经分配好了内存地址
    
    ```cpp
    package hot100;
    
    public class GetIntersectionNodeTest {
    
       public static class ListNode {
           int val;
           ListNode next;
    
           ListNode(int x) {
               val = x;
               next = null;
           }
       }
    
       public static void main(String[] args) {
           ListNode l1 = new ListNode(4);
           ListNode la = new ListNode(1);
           ListNode lb = new ListNode(8);
           ListNode lc = new ListNode(4);
           ListNode ld = new ListNode(5);
           l1.next = la;
           la.next = lb;
           lb.next = lc;
           lc.next = ld;
    
           ListNode l2 = new ListNode(5);
           ListNode le = new ListNode(6);
           ListNode lf = new ListNode(1);
           l2.next = le;
           le.next = lf;
           lf.next = lb;
    
           System.out.println(l1.next == l2.next.next); // 1  false
           System.out.println(l1.next.next == l2.next.next.next); // 8  true
       }
    }
    ```
    
- 类的实例化对象的比较是 通过地址来比较的, 说白了就是是不是一个实例! Python 中，任何对象都可以在布尔值上下文中被评估为 `True` 或 `False`。具体来说：如果 `A` 是 `None`（即空值），那么 `if A` 的条件判断结果为 `False`。如果 `A` 是一个有效的对象（即不是 `None`），那么 `if A` 的条件判断结果为 `True`。
    
    ```python
    # Definition for singly-linked list.
    # class ListNode:
    #     def __init__(self, x):
    #         self.val = x
    #         self.next = None
    
    class Solution:
        def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> Optional[ListNode]:
            A = headA
            B = headB
            while A != B:
    		        # 这些A.next= ListNode(3)这样的都是题目系统已经写好了
                A = A.next if A else headB # 跳到headB只会运行一次, 因为跑完
                # 俩个链都没相交就是俩个都是None的情况了
                B = B.next if B else headA
            return A
    ```
    

---

## 3.18 (反转链表, 迭代)

- 迭代法, 仔细想想也不难, 本质就是改变指针指向, 每次都是指上一个, 对于每个节点, 进行: 改变指针信息 (指向前一个, 这个信息来源于哪呢)  在改变指针信息前, 存储原本指针信息(知道下一个找谁, 去指自己)  由2得出, 需要存储自己的位置信息到给下一个节点, 让他指
    
    ```python
    # Definition for singly-linked list.
    # class ListNode:
    #     def __init__(self, val=0, next=None):
    #         self.val = val
    #         self.next = next
    class Solution:
        def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
            cur = head
            pre  = None
            while cur :
                # 我原本指向谁
                xiayige = cur.next
                # key step, 要反转就是改编指的谁, 改成指之前的
                cur.next = pre
                # 怎么想的呢, emmm, 一次次往后的节点走,
                # 既然是循环往后遍历, 肯定有个要用来存之前的节点位置
                # 使得下一个循环的cur.next = pre 有东西
                pre = cur
                # 下一个节点喽
                cur = xiayige 
            return pre # 这个return pre 确实精髓
    ```
    

---

## 3.19 (反转链表, 递归)

- 强行到C++, 别忘了给每行后面加 `;`  目前, 不知道如何想到这个递归的方法, 再说吧
    
    ```cpp
    class Solution {
    public:
        ListNode* reverseList(ListNode* head) {
            if (!head || !head->next) {
                return head;
            }
            ListNode* newHead = reverseList(head->next);
            head->next->next = head;
            head->next = nullptr;
            return newHead;
        }
    };
    ```
    

---

## 3.20 (毕设, 没刷)

- 刷视频, 开到了递归, 上面那个反转链表的递归和汉诺塔的递归

## 3.21 (毕设, 没刷)

---

## 3.22 (链表的中间结点 & 回文链表 & 环形链表)

- C++好像区分 大小写, 不能写True, false
- 逻辑与（`&&`）、逻辑或（`||`）和逻辑非（`!`）
- **指针的声明**：
    - 指针的声明格式为：`类型名 *指针变量名;`
    - 例如：`int* p;` 表示声明了一个指向 `int` 类型的指针变量 `p`。
    - 在声明指针时， 是指针的标识符，表示这是一个指针变量。
- C++函数的定义包括以下几个部分：
    
    ```cpp
    返回类型 函数名(参数类型1 参数名1, 参数类型2 参数名2, ...) {
    // 函数体// 可能包含返回语句
    }
    ```
    
    - **返回类型**：函数执行完成后返回的值的类型。如果函数不返回值，则使用`void`。
    - **函数名**：函数的名称，用于标识函数。
    - **参数列表**：函数接收的输入参数，参数列表可以为空。
    - **函数体**：函数的具体实现，包含一组语句。
- 链表的中间节点, 快慢指针, slow走一步, fast 走俩步; 如果链表长度是单数, 那么当slow走到正中间, fast 正好到最后一个Node; 长度是偶数, 此时再走一步,就是slow到, 后一半的第一个, fast到最后一个Node→ next, 说白了就是null;
    
    ```cpp
    class Solution {
    public:
        ListNode* middleNode(ListNode* head) {
            ListNode* slow = head;
            ListNode* fast = head;
            while( fast && fast->next) 
            {
                slow = slow->next;
                fast = fast->next->next;  
            };
            return slow;
        }
    };
    ```
    
- 掌握了中间节点和反转链表, 就可以写回文链表了; 写俩个函数, 然后按照灵神的题解, 写函数的时候记得, 传参数, 要注明类型, 还行手撕了一波反转链表的最基础的解法; 然后是回文链表的思路, 先探测中间结点, 要么单数个的正好中间作为head2, 要么偶数个的后半部分的第一个作为head2; 然后就是while(head2){}, 因为head2 长度是肯定小于head, 所以我只要比较head 和head2 的val 是不是一样, 然后一个个比下去就好了, if 只要出现不一样就false, 没有就是最外层的直接return true
    
    ```cpp
    /**
     * Definition for singly-linked list.
     * struct ListNode {
     *     int val;
     *     ListNode *next;
     *     ListNode() : val(0), next(nullptr) {}
     *     ListNode(int x) : val(x), next(nullptr) {}
     *     ListNode(int x, ListNode *next) : val(x), next(next) {}
     * };
     */
    class Solution {
        ListNode* findmiddle(ListNode* head)
        {
            ListNode* slow = head;
            ListNode* fast = head;
            while(fast && fast->next){
                slow = slow -> next;
                fast = fast -> next -> next;
    
            }
            return  slow;
        }
        
    
        ListNode* reverse(ListNode* head){
            ListNode *pre = NULL;
            ListNode *cur = head;
            ListNode *mid = nullptr;
            while(cur){
                mid = cur ->next;
                cur -> next = pre;
                pre = cur;
                cur = mid;
            }
            return pre;
        }
    public:
        bool isPalindrome(ListNode* head) {
            ListNode* head1 = findmiddle(head);
            ListNode* head2 = reverse(head1);
            while(head2){
                if (head -> val != head2->val){
                    return false;
                }
                head = head->next;
                head2 = head2->next;
            }
            return true;
        }
    };
    ```
    
- 环形链表, 思路就是快慢指针, 注意如果给的测试用例是空, 要单独判断, 第一次写的时候, 没想到这点
    
    ```cpp
    /**
     * Definition for singly-linked list.
     * struct ListNode {
     *     int val;
     *     ListNode *next;
     *     ListNode(int x) : val(x), next(NULL) {}
     * };
     */
    class Solution {
    public:
        bool hasCycle(ListNode *head) {
            if(head == NULL){
                return false;
            }
            ListNode* slow = head;
            ListNode* fast = head;
            while(fast->next && fast->next->next )
            {
                slow = slow->next;
                fast = fast->next->next;
                if (slow == fast)
                {
                    return true;
                }
            }
            return false;
        }
    };
    ```
    

---

## 3.23 (环形链表2 & 合并两个有序数组)

- O(1)的时间复杂度, 需要数学上的优化, 证明slow和fast相遇后, head和slow一起移动一定能在环形入口相遇; 正常思路是slow 边走边用哈希表记录遍历的节点, 出现重复的结点, return
其他没什么问题, 一个是空指针要判定, 空指针没有“成员”, 所以尝试访问违法…
另外有个是写fast -> next && fast -> next -> next, 不要写反了!!!
    
    ```cpp
    /**
     * Definition for singly-linked list.
     * struct ListNode {
     *     int val;
     *     ListNode *next;
     *     ListNode(int x) : val(x), next(NULL) {}
     * };
     */
    class Solution {
    public:
        ListNode *detectCycle(ListNode *head) {
            ListNode* slow = head;
            ListNode* fast = head;
            if(head == nullptr){
                return nullptr;
            }
            // while((fast -> next-> next) != nullptr){
            while(fast -> next && fast -> next -> next){
                slow = slow -> next;
                fast = fast -> next-> next;
                if (slow==fast){
                    while(head != slow){
                        head = head->next;
                        slow = slow-> next;
                    }
                    return slow;
                }
            }
            return nullptr;
        }
    };
    ```
    
- 88. 合并两个有序数组, 其实我是有点懵的; 有序数组二分查找和指针?  逆向双指针, 时间复杂度O(m+n); 然后是一个 `nums1[p--] = nums1[p1--];`  学了一个这种写法, 更新内容同时更新指针的索引
    
    ```cpp
    class Solution {
    public:
        void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
            int p = m+n-1;
            int p1 = m-1;
            int p2 = n-1;
            while(p2 >=0){
                if(p1>=0 && nums1[p1]>nums2[p2]){
                    nums1[p--] = nums1[p1--];
                }
                else{
                    nums1[p--] = nums2[p2--];
                }
            }
        }
    };
    ```
    
- 在C++中，逻辑运算符`&&`具有短路求值的特性。这意味着在表达式`A && B`中，如果`A`为`false`，则不会计算`B`，因为整个表达式的结果已经确定为`false`在你的条件判断`fast->next && fast->next->next != nullptr`中，`fast->next`会先被计算。
    
    如果`fast->next`为`false`（即`fast->next`是空指针），则整个表达式的结果为`false`，并且不会计算`fast->next->next != nullptr`部分。因此，`fast->next`的判断优先于`fast->next->next != nullptr`的判断。这样可以避免在`fast->next`为空指针时访问`fast->next->next`导致的空指针解引用错误。
    
- 如果`fast`本身是空指针，或者`fast->next`是空指针，那么`fast->next->next`就会尝试访问空指针的成员，导致运行时错误
- 在C++中，逻辑运算符`&&`的优先级低于比较运算符`!=`
- 呃 然后`while(fast -> next && fast -> next -> next != nullptr)` 可以直接写成 `while(fast -> next && fast -> next -> next)`  `!=nullptr`  就可以省略了
- **`cur = nums2[p2--];`**: 这部分表示将`nums2`数组中索引为`p2`的元素的值赋给变量`cur`，然后将`p2`的值减1

---

## 3.24 (合并两个有序链表)

- 这道题目, 起码写了俩个小时, debug也花了好半时间… 自己写的代码逻辑混乱啊,非常的不优雅 稍微总结下就是: 把另外一个链表塞进另一个链表, 要先弄一个`ListNode *tou = new ListNode();` 头的空指向List2, 后面有很多要判断的分支和细节处理, 思路很复杂, 要思考的点很多
    
    ```cpp
    class Solution {
    public:
        ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
            // ListNode* head1(nullptr, ListNode *list1);
            // ListNode* head2(nullptr, ListNode *list1);
            // pre1 = head1;
            // pre2 = head2;
            ListNode *tou = new ListNode();
            ListNode *cur;
            ListNode *ans;
            ListNode *a;
            tou ->next = list2;
            bu = tou;
            if (list2 == nullptr){
                return list1;
            }
            while(list1 && list2){
                if(list1->val >= list2-> val && list2->next == nullptr){
                    list2 -> next = list1;
                    return bu -> next;
                }else if (list1->val >= list2-> val && list1->val <= list2->next->val ){
                    // a = list1;
                    // list1 = list1->next;
                    // cur = list2->next;
                    // list2->next = a;
                    // a -> next = cur;
                    // list2 = list2 -> next;
                    // 少用一个中间变量
                    // 存要放进去的结点, list1先动完
                    // 先让存好的指向后续的, 在让前面指向存好的, list2再动
                    a = list1;
                    list1 = list1->next;
                    a -> next = list2->next;
                    list2->next = a;
                    list2 = list2 -> next;
                    
                }else if(list1 -> val < list2-> val){
                // 这里不直接写 tou = list1, 区别在于, tou 目前指的是空节点
                // 空节点, 同时也被最后要输出的ans指着, 能够找到新链的head
                // 不像下面这样写, 就会丢失head的位置
                    tou->next = list1;
                    tou = tou-> next;
                    list1 = list1-> next;
                    tou-> next = list2;
                }
                else{
                    list2 = list2-> next;
                }
            }
            return ans->next;
        }
    };
    ```
    
- 看下, 灵神的做法, 不是把一个链表插入另一个, 而是重开一个, 一个个插在第三个, 也不会用到多余的空间, 因为都是直接拿list1和list2 已有的链表结点位置直接用的;  直接开个哨兵结点, 然后一个个往后面加就完事了… 其实我上面的思路就是每次接完还有一定把list2拼进来, 就很多余, 没啥必要, 纯属给自己增加code量和思维量.
    
    讲完思路的问题, 来看看这个代码
    
    `ListNode dummy{}; 
     auto cur = &dummy;` auto 自动为cur 寻找类型, `&dummy` 传的是地址
    
    `ListNode *tou = new ListNode();` 然后就是这个new的语法, 返回的是这个对象的地址
    
    ```cpp
    class Solution {
    public:
        ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
            ListNode dummy{}; // 用哨兵节点简化代码逻辑
            auto cur = &dummy; // cur 指向新链表的末尾
            // 后面也会发现, 虽然这里是一个指向 栈内存, 但是实际return 的是dummy.next,
            // 是dummy指向的下一个, 也就是要么list1, 要么list2. 就不会有作用域的问题了 
            while (list1 && list2) {
                if (list1->val < list2->val) {
                    cur->next = list1; // 把 list1 加到新链表中
                    list1 = list1->next;
                } else { // 注：相等的情况加哪个节点都是可以的
                    cur->next = list2; // 把 list2 加到新链表中
                    list2 = list2->next;
                }
                cur = cur->next;
            }
            cur->next = list1 ? list1 : list2; // 拼接剩余链表
            return dummy.next;
        }
    };
    ```
    
- 三元运算符
`condition ? expr1 : expr2`
 eg. `cur->next = list1 ? list1 : list2;`  如果 `list1` 不为空（`list1` 为 `true`），则 `cur->next` 被赋值为 `list1`  这段代码通常出现在合并两个有序链表的算法中。在合并过程中，当其中一个链表已经完全被遍历完（即 `list1` 或 `list2` 为空），剩下的部分可以直接接到新链表的末尾。这段代码的作用就是将剩余的链表部分接到新链表的末尾。
- 在大多数情况下，`ListNode()` 和 `ListNode{}` 的行为是相同的，尤其是在有默认构造函数的情况下。然而，`ListNode{}` 的语法更现代，避免了函数调用的歧义，并且在聚合初始化时更灵活。因此，推荐使用 `ListNode{}` 的写法。
- **`ListNode dummy{}; auto cur = &dummy;` 能写成 `ListNode *cur = &dummy;` 吗?**
    
    **可以**，这两种写法在功能上是等价的。
    
    - `ListNode dummy{};` 定义了一个 `ListNode` 类型的对象 `dummy`，并使用默认初始化。
    - `auto cur = &dummy;` 中，`auto` 关键字会根据初始化表达式的类型来推断变量的类型。在这里，`&dummy` 是 `dummy` 对象的地址，其类型是 `ListNode*`（指向 `ListNode` 类型的指针）。因此，`cur` 被推断为 `ListNode*` 类型。
    - 直接写成 `ListNode *cur = &dummy;` 明确地声明了一个指向 `ListNode` 的指针变量 `cur`，并将其初始化为 `dummy` 的地址。这两种写法在生成的机器代码和功能上是完全相同的。
- **`ListNode *cur = &ListNode();` 可不可以，原因是什么?**
    
    **不可以**，这种写法会导致临时对象的生命周期问题。
    
    - `ListNode()` 会创建一个临时的 `ListNode` 对象。这个临时对象在表达式结束时（即分号 `;` 处）就会被销毁。
    - `&ListNode()` 取得的是这个临时对象的地址，将其赋值给指针 `cur`。然而，当这行代码执行完毕后，临时对象已经被销毁，`cur` 成为了一个悬空指针（dangling pointer），指向一个已经不存在的对象。
    - 后续如果使用这个悬空指针 `cur`（比如解引用它），会导致未定义行为（Undefined Behavior），可能会程序崩溃或读取到错误的数据。
    
    因此，`ListNode *cur = &ListNode();` 是不正确的写法，应该避免
    
- 栈内存和堆内存
    
    ![image.png](Leetcode%20%E5%88%B7%E9%A2%98%E6%97%A5%E8%AE%B0%201adca7bc3d8b802b8c04c13a85aa55d9/image.png)
    
- 在 `ListNode *tou = new ListNode();` 中，`new` 运算符用于动态分配内存，创建的对象不是临时对象。
    
    **关键区别:**
    
    - **临时对象（如 `ListNode()` 或 `ListNode{}`）**这些对象是在栈上分配的，它们的生命周期仅限于其所在的作用域。当代码执行到分号 `;` 时，临时对象会被销毁。
    - **用 `new` 创建的对象**这些对象是在堆上分配的，它们的生命周期不受作用域限制。只有当你显式调用 `delete` 时，对象才会被销毁。
- 栈中的基本类型
    
    ```cpp
    void example() {
        int a = 42;      // a 的值 42 直接存储在栈中
        float b = 3.14f;  // b 的值 3.14 直接存储在栈中
    } // a 和 b 在此处被自动释放
    ```
    
- **堆中的基本类型**
    
    ```cpp
    void example() {
        int* p = new int(42);    // 堆内存分配，存储 42
        float* q = new float(3.14f);  // 堆内存分配，存储 3.14
        // ...
        delete p;  // 必须手动释放
        delete q;
    }
    ```
    
- 在 `ListNode *cur = head;` 中，`cur` 是一个指针变量，它本身存储在栈内存中，但它指向的对象可能在堆内存或栈内存中，具体取决于 `head` 的定义。
    
    ### **1. `cur` 指针变量本身**
    
    - `cur` 是一个局部变量，存储在栈内存中。
    - 它的作用是保存一个地址（即它指向的对象的内存地址）。
    
    ### **2. `cur` 指向的对象**
    
    `cur` 指向的对象的内存性质取决于 `head` 的定义：
    
    ### **情况 1：`head` 指向堆内存**
    
    如果 `head` 是通过 `new` 动态分配的内存，那么 `cur` 指向的对象在堆内存中。例如：
    
    ```cpp
    ListNode *head = new ListNode(); *// head 指向堆内存*
    ListNode *cur = head;            *// cur 也指向堆内存*
    ```
    
    在这种情况下：
    
    - `head` 和 `cur` 都是栈变量（存储在栈内存中）。
    - 它们保存的地址指向堆内存中的 `ListNode` 对象。
    
    ### **情况 2：`head` 指向栈内存**
    
    如果 `head` 指向的是栈上的对象，那么 `cur` 指向的对象也在栈内存中。例如：
    
    ```cpp
    ListNode head;        *// head 是栈变量*
    ListNode *cur = &head; *// cur 指向栈内存*
    ```
    
    在这种情况下：
    
    - `head` 是一个栈变量。
    - `cur` 是一个栈变量，保存了 `head` 的地址。
    
    ### **总结**
    
    - `cur` 指针变量本身存储在栈内存中。
    - `cur` 指向的对象可能在堆内存或栈内存中，具体取决于 `head` 的定义。

---

## 3.25(俩数相加)

- 在C++中，整数型变量在布尔判断中会被隐式转换为布尔值，判断依据是值是否为零.
- 先来看看我们遇到的问题 (困扰了我很久的问题)
    
    ```cpp
    // 不行的
    ListNode end;
    end = ListNode(1); 
    l1 -> next = &end; 
    
    return head; 之后出了这个函数作用域, 外面拿过来, 指针指的就是悬空指针
    
    // 可行的, 推荐以后要创一个空节点, 并指向它的话, 就按照这样写
    ListNode* end = new ListNode(1);
    l1 -> next = end;
    // ok
    cur = cur->next = new ListNode(carry % 10);
    ```
    
- 来看下我的愚蠢代码, 只能说时间复杂度和空间复杂度都拉满了; 具体思路是: 一直看l1, 默认l1比l2 长, 如果发现 一样长,就看最后要不要进位置; 或者短, 就把l1 的next 指到 l2 的next 再执行 `l1 = l1->next;`   就是一定要注意的是, 因为我们最后返回的是一个链表, 这个链表是一个接一个的连起来的, 所以我们要确保, 每个listnode 本身的next 是连在一起的; 在修改之前我是 判断完 俩个链表的长度以及后面有没有nullptr, 直接就先`l1 = l1->next;`, 这个时候再交换他们所指的位置, 就完蛋了, 因为前面那个l1 链的点还是指着空啊!!!等于说没有把l1 链到l2 上, 虽然code 里有着操作, 但是想想就知道不对了
    
    ```cpp
    /**
     * Definition for singly-linked list.
     * struct ListNode {
     *     int val;
     *     ListNode *next;
     *     ListNode() : val(0), next(nullptr) {}
     *     ListNode(int x) : val(x), next(nullptr) {}
     *     ListNode(int x, ListNode *next) : val(x), next(next) {}
     * };
     */
    class Solution {
    public:
        ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
            int carry=0;
            ListNode* head = l1;
            ListNode* cur;
            // ListNode end; // 声明为静态变量
            //end = ListNode(1); 
            ListNode* end = new ListNode(1);
            while(l1){
                if(l2){
                    if(l1 -> val + l2 -> val +carry>=10){
                        l1 -> val = l1 -> val + l2 -> val +carry -10;
                        carry = 1;
                    }else{
                        l1 -> val = l1 -> val + l2 -> val+carry;
                        carry = 0;
                    }
                    if(l1->next == nullptr && l2 ->next){
                        cur = l1-> next;
                        l1-> next = l2->next;
                        l2->next = cur;
                    }
                    if(l1->next == nullptr && l2 ->next ==nullptr && carry==1){
                        l1 -> next = end;
                        return head;
                    }
                    l1 = l1->next;
                    l2 = l2-> next;
                }else{
                    if(l1->val+ carry == 10){
                        l1->val = 0;
                        carry=1;
    
                    }else{
                        l1->val = l1->val+carry;
                        carry = 0;
                    }
                    if(l1->next == nullptr && carry==1){
                        l1 -> next = end;
                        return head;
                    }
                    l1 =l1-> next;
                }
    
            }
            return head;
        }
    };
    ```
    
- 这个解决思路好, 再循环里单独看 l1 和l2 以及是否有进位, 这样就可以来判断是否需要下一个新开的节点 来存储了, `if >= 10` 的逻辑, 也可以用 `/= 10` 直接替代
    
    ```cpp
    class Solution {
    public:
        ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
            ListNode dummy; // 哨兵节点
            ListNode* cur = &dummy;
            int carry = 0; // 进位
            while (l1 || l2 || carry) { // 有一个不是空节点，或者还有进位，就继续迭代
                if (l1) {
                    carry += l1->val; // 节点值和进位加在一起
                    l1 = l1->next; // 下一个节点
                }
                if (l2) {
                    carry += l2->val; // 节点值和进位加在一起
                    l2 = l2->next; // 下一个节点
                }  
                cur = cur->next = new ListNode(carry % 10); // 每个节点保存一个数位
                carry /= 10; // 新的进位, 整除/
            }
            return dummy.next; // 哨兵节点的下一个节点就是头节点
        }
    };
    ```
    

---

## 3.26(删除链表的倒数第N个结点 & 两两交换链表的节点)

- 递归解决的思路, 有点吊
    
    ```cpp
    class Solution {
    public:
        int cur=0;
        ListNode* removeNthFromEnd(ListNode* head, int n) {
           if(!head) return NULL;
           head->next = removeNthFromEnd(head->next,n);
           cur++;
           if(n==cur) return head->next;
           return head;
        }
    };
    ```
    
- 然后是比较多的, 看了灵神的解法, 其他解法看到的有,反转链表之后再数n个.   太久没碰C语言了, 没想到for循环的 k 还有在外面提前定义好
值得注意的是, 因为有可能会删除头结点, 而删除某个节点又需要指针处在它的上个结点, 所以我们考虑可能会需要删除头结点, 所以需要引入dummy
    
    ```cpp
    /**
     * Definition for singly-linked list.
     * struct ListNode {
     *     int val;
     *     ListNode *next;
     *     ListNode() : val(0), next(nullptr) {}
     *     ListNode(int x) : val(x), next(nullptr) {}
     *     ListNode(int x, ListNode *next) : val(x), next(next) {}
     * };
     */
    class Solution {
    public:
        ListNode* removeNthFromEnd(ListNode* head, int n) {
            ListNode *dummy = new ListNode();
            dummy -> next = head;
            ListNode *i = dummy;
            ListNode *j = head;
            int k;
            for(k=2; k<=n; k++){
                j = j-> next;
            }
            while(j->next){
                i = i-> next;
                j = j-> next;
            }
            i -> next = i->next -> next;
            return dummy-> next;
            
    
        }
    };
    ```
    
- 交换节点, 一眼递归, 这次总算自己摸索出了一个递归答案, 一次过!
1. 找递归什么时候终止
2. 我们要先明确, 这个递归到底要返回一个什么东西
3. 我们只关注一次调用的具体操作, 而不去详细分析到底层层怎么呼应(我选择放在最后用案例手动跑一次)

明显, 当递归到只剩最后一个,或者直接指向空, 就要开始往回弹值了, 然后比较重要的point 是哪一行递归调用具体是怎么写的code, 说白了就是 xxx= 函数(yyy), 找到这个xxx 和yyy应该分别是什么, 在这行上面是退出条件, 这行下面是每层函数具体要干的事情(顺序写循环要干的事) 模糊的思路就是这个, 不要太抠细节, 先把大概的写出来, 细节的节点, next head 什么的可以用顺序再想
    
    ```cpp
    class Solution {
    public:
        ListNode* swapPairs(ListNode* head) {
            ListNode *cur;
            ListNode *newhead;
            if(head == nullptr){
                return nullptr;
            }else if(head -> next ==nullptr){
                return head;
            }
            // 这里还可以优化成
            // if(head == nullptr || head->next == nullptr){return head;}
            head -> next -> next = swapPairs(head->next -> next);
            newhead = head -> next;
            cur = head -> next -> next;
            head -> next -> next = head;
            head -> next = cur;
            return newhead;
        }
    };
    ```