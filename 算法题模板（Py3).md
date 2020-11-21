## 写在写代码前/写完检查时
测试用例的选取
 1. 功能测试（一般性）
 2. 边界值
 3. 特殊值（空）

## 特殊函数

 

 1. List item
 2. 返回a和b的最大公约数
 	math.gcd(a,b)
 3. Python的计数器
	cnt = Collections.counter(str)
 4. 双端队列
 	【构造】queue = collections.deque()
 	【方法】左端弹出：queue.popleft()
 	【方法】右端弹出：queue.pop()
 	【方法】左端加入：queue.appendleft()
 	【方法】右端弹出：queue.append()
 5. lambda函数
 	a = lambda x,y,z:(x+y)*y-z
 	要点：
（1）lambda 函数不能包含命令，

	（2）包含的表达式不能超过一个。

	说明：一定非要使用lambda函数；任何能够使用它们的地方，都可以定义一个单独的普通函数来进行替换。我将它们用在需要封装特殊的、非重用代码上，避免令我的代码充斥着大量单行函数。

	lambda匿名函数的格式：冒号前是参数，可以有多个，用逗号隔开，冒号右边的为表达式。其实lambda返回值是一个函数的地址，也就是函数对象。
 	例： intervals.sort(key=lambda x:x[0])
 	可以进行对多维数组按某一行或某一列排序
6. set()函数用来构造无重复的元素合集
	a = set()
	增删元素操作： a.remove()， a.add()
7. 正负无限规避最大（小）值判断，float('inf'), float('-inf')
8.                   模块heapq中一些重要的函数
  函 数                                                  描 述
heappush(heap, x)                                      将x压入堆中
heappop(heap)                                      从堆中弹出最小的元素
heapify(heap)                                        让列表具备堆特征
heapreplace(heap, x)                            弹出最小的元素，并将x压入堆中
nlargest(n, iter)                                  返回iter中n个最大的元素
nsmallest(n, iter)                                返回iter中n个最小的元素
## **1. BFS**
 **- 类型**

 - 1. 二维多源（单->多）
 - 2. 多层输出（二叉树按层）
 ** **

 **- 例题**

 - [LC 542. 01矩阵](https://leetcode-cn.com/problems/01-matrix/)
 -  [LC 1162. 地图分析](https://leetcode-cn.com/problems/as-far-from-land-as-possible/)
 - [LC 199. 二叉树的右视图](https://leetcode-cn.com/problems/binary-tree-right-side-view/)
 - 

** **
**- 核心思想**
BFS核心就在于按层遍历，也就是所有能分步按层去解决的问题都适用这种搜索方法。以下列举一些BFS的特点：

1.一定是用到deque的结构来模拟队列
2.队列有一个初始节点
3.内层循环，每次处理从队列中出队一个元素
4.对元素进行扩张（新入列）
5.对于扩张后满足条件的点再进行处理
6.外层循环，继续循环处理deque中的元素，直到deque为空，则代表所有点都已经完成了扩张。
（完成扩张的点也进行标记）

** **
**- 具体技巧及代码模板**
    对于无向图搜索来说，必须得标志是否访问过，如果要求在原图（矩阵）中进行计算，一个技巧是将访问过的节点设为负值。(或者灵活地先将所有未访问的值设为-1，访问后设为正值)
    首先，BFS使用队列，把每个还没有搜索到的点依次放入队列，然后再弹出队列头部元素当做当前的遍历点。
    
 	（1）不需要确定当前遍历到了哪一层
 		
```python
while queue not None:
	cur = queue.pop()
	for 节点 in cur 的所有相邻节点：
		if 该节点有效且未被访问过：
			queue.push(当前节点) 
```

 	（2）需要确定当前遍历到了哪一层，增加参数level表示当前遍历的层数，size 表示当前遍历层有多少元素（或者说当前层源点的个数，用来构造遍历循环）。
 	

```python
queue = collections.deque()
level = 0
while queue not None:
	size = queue.size()
	while (size -= 1 ):
		for 节点 in cur 的所有相邻节点：
		if 该节点有效且未被访问过：
			queue.push(当前节点)
			#queue.append((nx,ny))用来记录下一层将要被访问的邻节点
	levle += 1
```

 ## **2. DFS**
 **- 类型**

 - 1. 遍历整个无向图网络
****
 **- 例题**

 - [LC 200. 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)
 - [LC 695. 岛屿的最大面积](https://leetcode-cn.com/problems/max-area-of-island/)
****
 **- 核心思想**
 遍历整个网络，当遇到符合条件的点时，从该点开始进行深度优先搜索，将dfs搜索过的点置-1或0。继续搜索网络下一个符合条件的点直至网络中所有的点。
 ** **
**- 具体技巧及代码模板**
    

 - 定义dfs方法：寻找点（i, j）附近所有符合条件的点（相邻）
 	从 (i, j) 向此点的上下左右 (i+1,j),(i-1,j),(i,j+1),(i,j-1) 做深度搜索
 	终止条件：（i, j ）越界或者下一个相邻节点不符合条件		
 - 	主循环：
   遍历整个矩阵
以下代码以dfs搜索最大的以“1”为陆地的最大岛的面积为例
```python
def dfs(self, grid, cur_i, cur_j):
        if cur_i < 0 or cur_j < 0 or cur_i == len(grid) or cur_j == len(grid[0]) or grid[cur_i][cur_j]!=1:
            return 0
        area = 1
        grid[cur_i][cur_j] = 0
        for di, dj in [[1,0], [-1,0], [0,1], [0,-1]]:
            next_i, next_j = cur_i + di, cur_j + dj
            area = self.dfs(grid, next_i, next_j)+area
        return area
```
主循环

```python
def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
    ans = 0
    for i in range(len(grid)):
        for j in range(len(grid[0])):
            ans = max(ans, self.dfs(grid, i, j))
    return ans
```

 ## **3.二分查找**


 ## **4.滑动窗口技巧**
  **- 例题**

 - [LC209. 长度最小的子数组](https://leetcode-cn.com/problems/minimum-size-subarray-sum/)
 - [LC3. 无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

****
 **- 核心思想**
 暴力法遍历时时间复杂度会过高。则使用滑动窗口（类似于双指针）思想。以LC3题为例，定义“”不重复子串” 的起始和结束位置分别为start 和 end, 随着找到符合条件的子串出现，或者当前子串不再符合条件，则将start的值增加（指针向右移动），重复该项操作直至再次符合（不符合）题目条件。
 ** **
**- 代码模板**
以题LC209为例，去找到长度最小的子数组
```python
min_len = n
cur_len = 0
cur_sum = 0
left = 0 #指向子数组的第一个元素
#主循环的i相当于右指针，指向了查询子数组的最后一个元素
for i in range(len(n)):
	cur_len += 1
	cur_sum += nums[i]
#一旦出现了满足条件的子数组，就迭代一次最小子数组长度，同时left指针向右移动一个直至不符合条件的数组产生。
            while cur_sum >= s:
                min_len = min(cur_len, min_len)
                cur_sum -= nums[left]
                cur_len -= 1
                left += 1
```
边界条件，考虑如果数组所有数相加都不能达到值s时的情况，

```python
if cur_len == 0 and cur_sum < s:
	return 0
```

 ## **5.前缀和**
   **- 例题**
 - [LC560. 和为K的子数组](https://leetcode-cn.com/problems/subarray-sum-equals-k/solution/xiong-mao-shua-ti-python3-qian-zhui-he-zi-dian-yi-/)
 - [LC1248. 统计“优美子数组”](https://leetcode-cn.com/problems/count-number-of-nice-subarrays/)
 - [LC974. 和可被K整除的子数组](https://leetcode-cn.com/problems/subarray-sums-divisible-by-k/)
 ****
 **- 核心思想**
能够应用前缀和思想的题目一般为求解一个连续或不连续的子数组
 ** **
 **- 代码模板**
以题LC560为例，
```python
num_times = collections.defaultdict(int) #存储某“前缀和”出现的次数，这里用collections.defaultdict来定义它
num_times[0] = 1 #用来记录出现前缀和为0的次数为1
cur_sum = 0 #用来记录当前前缀和
res = 0 
for i in range（len(nums)):
	cur_sum += nums[i]
	if cur_sum - k in num_times:
		res += num_times[cur_sum-k]
	num_times[cur_sum] += 1
return res

```
## **6.中序遍历/前序遍历/后序遍历**
   **- 例题**
 - [剑指Offer 36.二叉搜索树与双向链表](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/)

 ****
 **- 核心思想**
前序：以“根左右”顺序递归打印
中序：以“左根右” 顺序递归打印
后序：以“左右根” 顺序递归打印
 ** **
 **- 代码模板**
后序：
```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
"""
def dfs(root):
	if not root: return root
	dfs(root.left)
	print(root.val)
	dfs(root.right)
```

## **7.回溯法**
   **- 例题**
 - [剑指Offer 38.字符串的排列](https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/)

 ****
 **- 核心思想**
回溯算法框架：解决一个问题，实际上就是一个决策树的遍历过程。
1. 路径：做出的选择
2. 选择列表：当前可以做的选择
3. 结束条件：当到达决策树底层，无法再做选择的条件。
 **- 伪代码**
result = []
def backtrack(路径，选择列表)：
if 满足结束条件：
	result.add(路径)
	return 
for 选择 in 选择列表：
	做选择
	backtrack(路径，选择列表)
	撤销选择
 ** **
 **- 代码模板**
 字符串
 ```python
 def permutation(self, s: str) - > List[str]:
 	if not s:
 		retrurn
 	result = []
 	s = list(sorted(s))
 	def backtrack(s, tmp):
 		if not s:
 		result.append(''.join(tmp))
 		for i, char in enumerate(s):
 			if i > 0 and s[i] == s[i-1]:
 				continue
			#这条判断对应去除掉重复的两个元素顺序不影响结果
         #如'abb'选了一个'a'之后，就不需要再选了
 			backtrack(s[:i] + s[i+1], tmp + [char])
 		backtrack(s, [] )
 		return result
 
```
