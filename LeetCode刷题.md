 <font  size=5 color="orange">**LeetCode热题100** </font>     

> - 哈希
>
>     原地哈希(自造哈希)
>
> - 链表
>
> - 堆
>
> - 栈
>
> - 矩阵
>
> - 数组
>
> - 二叉树
>
> - 图论
>
> - 二分查找
>
> - 双指针
>
> - 滑动窗口
>
> - 动态规划
>
> - 多维动态规划
>
> - 回溯
>
> - 贪心算法
>
> - 技巧
>
> ==094 递归可以使用迭代的条件==
>
> ==回溯  需要记录过程  动态规划  需要某个结果量,该结果量可以通过更小范围的结果量求得  见070 131(经典)==
>
> ==023  比较器使用 lambda表达式 `PriorityQueue<ListNode> queue = new PriorityQueue<>((o1,o2)-> o1.val - o2.val);`==



- <font  size=5 color="red">238   除自身以外数组的乘积(未解出)</font>

- <font  size=5 color="red">031  下一个排列(未解出)</font>

- <font  size=5 color="red">763  划分字母区间(未解出)</font>

- <font  size=5 color="red">300  最长递增子序列(未解出)</font>

- <font  size=5 color="red">41  缺失的第一个整数(未原地算法)</font>

- <font  size=5 color="red">004 寻找两个正序数组的中位数(未解出)</font>

- <font  size=5 color="red">084.  柱状图的最大的矩形(未解出)</font>

- <font  size=5 color="red">394.  字符串解码(极大优化代码)</font>

- <font  size=5 color="red">437.  路径总和(只会暴力)</font>

- 

  ​    



# 哈希

### <font  size=5 color="red">001. 两数之和</font>


  **题目**

```
给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。
```
**示例 1：**

```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

**示例 2：**

```
输入：nums = [3,2,4], target = 6
输出：[1,2]
```

**示例 3：**

```
输入：nums = [3,3], target = 6
输出：[0,1]
```



**思路**

1. 双向寻找,数据需要满足有序(自创)

2. 双for循环,优化后,第二个for从第一个索引后开始

3. > **HashMap**
   >
   > 数组元素不断加入哈希表(键为元素的值,值为元素的索引),
   >
   > 加入时将差和哈希表的键集合进行对比,没有就继续加入
   >
   > (,时间复杂度为n,哈希查找快)

**代码 思路3 ** **HashMap**

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> hashtable = new HashMap<Integer, Integer>();
        for (int i = 0; i < nums.length; ++i) {
            if (hashtable.containsKey(target - nums[i])) {
                return new int[]{hashtable.get(target - nums[i]), i};
            }
            hashtable.put(nums[i], i);
        }
        return new int[0];
    }
}
```

**代码 思路2**

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int n = nums.length;
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                if (nums[i] + nums[j] == target) {
                    return new int[]{i, j};
                }
            }
        }
        return new int[0];
    }
}
```



### <font  size=5 color="red">049. 字母异位词分组</font>

​	**题目**

```
给你一个字符串数组，请你将 **字母异位词** 组合在一起。可以按任意顺序返回结果列表。

**字母异位词** 是由重新排列源单词的所有字母得到的一个新单词。
```

​	**示例 1:**

```
输入: strs = ["eat", "tea", "tan", "ate", "nat", "bat"]
输出: [["bat"],["nat","tan"],["ate","eat","tea"]]
```

​	**示例 2:**

```
输入: strs = [""]
输出: [[""]]
```

​	**示例 3:**

```
输入: strs = ["a"]
输出: [["a"]]
```





**思路**

1. > **自己** 利用哈希表储存数据,将同一类字符串放在同一个集合,作为一个值,键表示这这集合的统一特征
   >
   > 判断两个字符串是否同类,将字符串转为数组,判断新字符串数组和该数组的长度和内容是否一样

2. 判断两字符串是否同类和思路一不同,==将字符串转为数组,进行排序,再转为字符串==,这样字母异位词对应的字符串应该一样(<font size =5  color="red">寻找不同元素相同点,将不同元素经过一定的操作转化为统一的特征值,利用哈希表进行储存</font>)

3. ==将每个出现次数大于 0 的字母和出现次数按顺序拼接成字符串==，作为哈希表的键

4.  **思路分析** 哈希查找速度快,HashMap相比HashSet,当需要键的值时使用,单用键或者后面的键加入时对前面的键有影响时,就用HashSet,(否则浪费资源或者处理有影响的键比较浪费资源)

     

     

    **思路2**

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        // 利用哈希表储存数据,将同一类字符串放在同一个集合,作为一个值,键表示这这集合的统一特征
        // 统一特征 若两字符串为字母异位词,将字符转为字符数组,进行排序,再转为字符串, 最后的字符串就作为键

        Map<String, List<String>> map = new HashMap<String, List<String>>();

        // 获取字符串的特征字符串,哈希表的键中存在该特征字符串,就将字符串家加入对应的值,字符串列表中
        for(String str: strs){
            char[] array = str.toCharArray();
            Arrays.sort(array);
            String key = new String(array);
            List<String> list = map.getOrDefault(key, new ArrayList<String>());
            list.add(str);
            map.put(key,list);
        }

        // 将哈希表中的内容转化为返回的结果
        return new ArrayList<List<String>>(map.values());
    }
}
```

​	**思路2 细节**

> * 利用多态创建无实现类对象的抽象父类或接口 List, Map 
> * Map 的**getOrDefault()**方法有助于键不存在是添加添加键值对或者修改键值对(当值是集合)
> * 利用映射快速获得map值的Collection

​	**思路1**

```java
public static ArrayList<ArrayList<String>> groupAnagrams(String[] strs) {
        Hashtable<List<Character>, ArrayList<String>> ht = new Hashtable<>();

        // 若有字符串对应的键,就将对应键的集合值加入str
        // 若无字符串对应的键,就新加键,把字符串放入值中
        for(String str: strs){
            if (isContains(ht, str) == null) {
                ArrayList<Character> list = new ArrayList<>();
                for (char c : str.toCharArray()) {
                    list.add(c);
                }

                ArrayList<String> list1 = new ArrayList<>();
                list1.add(str);
                ht.put(list, list1);
            } else {
                ArrayList<String> list1 = ht.get(isContains(ht, str));
                list1.add(str);
                ht.put(isContains(ht, str),list1);
            }
        }
        ArrayList<ArrayList<String>> list = new ArrayList<>();
        Set<List<Character>> keyset = ht.keySet();
        for (List<Character> key : keyset) {
            list.add(ht.get(key));
        }
        return list;
    }

    // 判断ht中的键是否有str的字母异位词
    // 若有就返回该键,没有就返回null
    private static List<Character> isContains(Hashtable<List<Character>, ArrayList<String>> ht, String str){
        Set<List<Character>> lists = ht.keySet();
        for (List<Character> list : lists) {
            if (judge(list, str)) {
                return list;
            }
        }
        return null;
    }

    // 检验两个字符串是否为 字母异位词, 暂不考虑有相同字母的情况
    private static boolean judge(List<Character> s1list, String s2){
        char[] s2Array = s2.toCharArray();

        if(s1list.size() != s2Array.length){
            return false;
        }

        for(int i = 0; i < s2Array.length; i++){
            if(!s1list.contains(s2Array[i])){
                return false;
            }
        }

        return true;
    }
```

​	**思路1 反思**

> ==未找到字母异位词的准确的特征,而是利用方法进行判断,过程复杂,不利于直接查找键对应的值==
>
> 添加键值对或者修改键值对可以利用映射进行优化





### <font  size=5 color="red">128. 最长连续序列</font>

给定一个未排序的整数数组 `nums` ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。

请你设计并实现**时间复杂度为 `O(n)`** 的算法解决此问题。

 

**示例 1：**

```
输入：nums = [100,4,200,1,3,2]
输出：4
解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。
```

**示例 2：**

```
输入：nums = [0,3,7,2,5,8,4,6,0,1]
输出：9
```



**思路**

> **思路一(自己) HashMap**
>
> 利用哈希表储存数据,键为连续索引的最小值,值为连续索引的长度
>
> **优点**  得到每个值所在索引的长度
>
> **优化**
>
> 插入数,若左右有值,向两端出发统计此列的长度(左右不需要更新,此列长度相等,只需要知道最大值)
>
> **不足:** 
>
> - 因为值的存在,需要不断动态规划,可以最后统计进行改进,见思路2
> - 时间复杂度不为O(n)
>
> **思路二 HashSet** 
>
> 利用HashSet查找快查找以每个值num开头的数据的长度,其中查找的值num要满足HashSet不包含num-1,这样非开可以快速跳过外循环,只出现在内循环
>
> **总结**
>
> 哈希查找时间复杂度为O(1)
>
> ==非相互独立的数据加入需要动态规划考虑其他值的影响或对其他值的影响,或者最后再进行统计如思路2==
>
> ==等价的结果可以只计算一个==,如思路一不需要更新每个值对应的列的长度,思路二只需要从序列开头开始统计





**思路一(自己) HashMap**

- 利用哈希表储存,key为连续数列的最小值,value为连续数列的长度
- 动态规划:新加入的数据并非相对独立,如果key-1存在,则之前的长度都应该更新,直到前面的数不存在(**优化:**实际上只需要更新最前面的,并进行比较,因为我们只需要获取最大值)
- 动态规划: 如果有重复值加入,哈希表就不变,否则二次可能加入会影响其他数据的更新
- 总之,非相互独立的数据加入需要动态规划考虑其他值的影响或对其他值的影响,或者最后再进行统计如思路2

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        // 利用哈希表储存,key为连续数列的最小值,value为连续数列的长度
        HashMap<Integer, Integer> startTable = new HashMap<Integer, Integer>();
        int startlength;
        int startNum;
        int temp;
        int maxLength;
        int length;

        for(int num: nums){
            startlength = startTable.getOrDefault(num + 1, 0);
            startNum = num - 1; // 暂时储存可能要更新的键
            temp = startTable.getOrDefault(num, 0); // 返回为0表示第一次出现
            
            // 若果num第一次出现
            // 如果num可以加入的数列存在,就放入(num,新length)
            // 如果num可以加入的数列不存在,就放入(num,0 + 1)此时length就是0
            // 如果num不是第一次出现,num-1对应的值重复+startlength,导致长度变大
           
           if(temp == 0){
                startlength++; // 应该放入的长度
                startTable.put(num,  startlength);
               // 如若num前面键存在,就进行更新
                temp = startTable.getOrDefault(startNum, 0);
                while(temp != 0){
                    startTable.put(startNum, temp + startlength);
                    startNum--;
                    temp = startTable.getOrDefault(startNum, 0);
                }       
            }           
        }

        // 遍历获取最大长度
        for(int num: nums){
            int startlength = startTable.get(num);
            length = startlength ;
            if(length > maxLength){
                maxLength = length;
            }
        }

        return maxLength;  
    }
}

```

**思路一 HashMap 优化**

```java 
class Solution {
    public int longestConsecutive(int[] nums) {
        
        int n=nums.length;
        HashMap<Integer,Integer>map = new HashMap<Integer,Integer>();
        int res = 0;
        for(int num:nums){
            if(!map.containsKey(num)){
                int left = map.get(num-1)==null?0:map.get(num-1);
                int right = map.get(num+1)==null?0:map.get(num+1);
                int cur = 1+left+right;
                if(cur>res){
                    res = cur;
                }
                map.put(num,cur);
                map.put(num-left,cur);
                map.put(num+right,cur);
            }
        }
        return res;
    }
}
```



**思路二 HashSet**

```java
 public int longestConsecutive(int[] nums) {
        HashSet<Integer> numSet = new HashSet<Integer>();
        int maxLength = 0;
        int tempLength = 0;

        for(int num: nums){
            numSet.add(num);
        }

        for(int num: numSet){
            tempLength = 0;

            if(!numSet.contains(num - 1)){
                tempLength++;
                
                while(numSet.contains(num + 1)){
                    tempLength++;
                    num++;
                }
                
                maxLength = Math.max(tempLength, maxLength);
            }
        }

        return maxLength;
    }
```





# 链表

### <font  size=5 color="red">003.  两数相加</font>

给你两个 **非空** 的链表，表示两个非负的整数。它们每位数字都是按照 **逆序** 的方式存储的，并且每个节点只能存储 **一位** 数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0 开头。

 

**示例 1：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/01/02/addtwonumber1.jpg)

```
输入：l1 = [2,4,3], l2 = [5,6,4]
输出：[7,0,8]
解释：342 + 465 = 807.
```

**示例 2：**

```
输入：l1 = [0], l2 = [0]
输出：[0]
```

**示例 3：**

```
输入：l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
输出：[8,9,9,9,0,0,0,1]
```

 

**提示：**

- 每个链表中的节点数在范围 `[1, 100]` 内
- `0 <= Node.val <= 9`
- 题目数据保证列表表示的数字不含前导零



> **思路1  据参考优化细节**
>
> 4到6行,19到24行,  实际上这两块可以合并

**思路1  据参考优化细节**

```java
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        int num, numnext = 0,n1, n2;
        ListNode head = null, tail = null;
        while(l1 != null || l2 != null){
            n1 = l1 != null ? l1.val : 0;
            n2 = l2 != null ? l2.val : 0;

            num = n1 + n2 + numnext;
            numnext = num / 10;
            num = num % 10;

            if (head == null) {
                head = tail = new ListNode(num);
            } else {
                tail.next = new ListNode(num);
                tail = tail.next;
            }

            if (l1 != null) {
                l1 = l1.next;
            }
            if (l2 != null) {
                l2 = l2.next;
            }                   
        }

        if(numnext == 1){
            tail.next = new ListNode(numnext);
        }

        return head;
    }
```



### <font  size=5 color="red">021. 合并两个有序列表 </font>

将两个升序链表合并为一个新的 **升序** 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/03/merge_ex1.jpg)

```
输入：l1 = [1,2,4], l2 = [1,3,4]
输出：[1,1,2,3,4,4]
```

**示例 2：**

```
输入：l1 = [], l2 = []
输出：[]
```

**示例 3：**

```
输入：l1 = [], l2 = [0]
输出：[0]
```

 

**提示：**

- 两个链表的节点数目范围是 `[0, 50]`
- `-100 <= Node.val <= 100`
- `l1` 和 `l2` 均按 **非递减顺序** 排列



> **思路1  迭代**
>
> 3-7行 无用,被29-33 包含
>
> 29-33 ==两个对立的选一个可以利用三元表达式简化代码,不必要==

> **思路1  修改细节**
>
> 2行  ==直接设置链表开头的前一个,最后返回第二个,避免开头不知道选哪个的情况==

**思路1  迭代**

```java
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        ListNode head, temp;
        if(list1 == null){
            return list2;
        }else if(list2 == null){
            return list1;
        }

        if(list1.val > list2.val){
            head = list2;
            list2 = list2.next;
        }else{
            head = list1;
            list1 = list1.next;
        }
        temp = head;

        while(list1 != null && list2 != null){
            if(list1.val > list2.val){
                temp.next = list2;
                list2 = list2.next;
            }else{
                temp.next = list1;
                list1 = list1.next;
            }
            temp = temp.next;
        }

        if(list1 == null){
            temp.next = list2;
        }else{
            temp.next = list1;
        }

        return head;
    }
```

**思路1  修改细节**

```java
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode prehead = new ListNode(-1);

        ListNode prev = prehead;
        while (l1 != null && l2 != null) {
            if (l1.val <= l2.val) {
                prev.next = l1;
                l1 = l1.next;
            } else {
                prev.next = l2;
                l2 = l2.next;
            }
            prev = prev.next;
        }

        // 合并后 l1 和 l2 最多只有一个还未被合并完，我们直接将链表末尾指向未合并完的链表即可
        prev.next = l1 == null ? l2 : l1;

        return prehead.next;
		}
```



### <font  size=5 color="red">023.  合并k个升序链表</font>

给你一个链表数组，每个链表都已经按升序排列。

请你将所有链表合并到一个升序链表中，返回合并后的链表。

 

**示例 1：**

```
输入：lists = [[1,4,5],[1,3,4],[2,6]]
输出：[1,1,2,3,4,4,5,6]
解释：链表数组如下：
[
  1->4->5,
  1->3->4,
  2->6
]
将它们合并到一个有序链表中得到。
1->1->2->3->4->4->5->6
```

**示例 2：**

```
输入：lists = []
输出：[]
```

**示例 3：**

```
输入：lists = [[]]
输出：[]
```

 

**提示：**

- `k == lists.length`
- `0 <= k <= 10^4`
- `0 <= lists[i].length <= 500`
- `-10^4 <= lists[i][j] <= 10^4`
- `lists[i]` 按 **升序** 排列
- `lists[i].length` 的总和不超过 `10^4`



> **思路1 优先级队列  思路一样,未想到队列的实现方式 **
>
> ==4行 lambda表达式 `PriorityQueue<ListNode> queue = new PriorityQueue<>((o1,o2)-> o1.val - o2.val);`==

> **思路 2  分治法** 
>
> 递归使用的比较好



**思路1 优先级队列  思路一样,未想到队列的实现方式 **

```java
class Solution {
   public ListNode mergeKLists(ListNode[] lists) {
        if (lists == null || lists.length == 0) return null;
        PriorityQueue<ListNode> queue = new PriorityQueue<>(lists.length, new Comparator<ListNode>() {
            @Override
            public int compare(ListNode o1, ListNode o2) {
				return o1.val - o2.val;
            }
        });
        ListNode dummy = new ListNode(0);
        ListNode p = dummy;
        for (ListNode node : lists) {
            if (node != null) queue.add(node);
        }
        while (!queue.isEmpty()) {
            p.next = queue.poll();
            p = p.next;
            if (p.next != null) queue.add(p.next);
        }
        return dummy.next;
   }
}
```

**思路 2  分治法** 

```java
class Solution {
   public ListNode mergeKLists(ListNode[] lists) {
        if (lists == null || lists.length == 0) return null;
        return merge(lists, 0, lists.length - 1);
    }

    private ListNode merge(ListNode[] lists, int left, int right) {
        if (left == right) return lists[left];
        int mid = left + (right - left) / 2;
        ListNode l1 = merge(lists, left, mid);
        ListNode l2 = merge(lists, mid + 1, right);
        return mergeTwoLists(l1, l2);
    }

    private ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if (l1 == null) return l2;
        if (l2 == null) return l1;
        if (l1.val < l2.val) {
            l1.next = mergeTwoLists(l1.next, l2);
            return l1;
        } else {
            l2.next = mergeTwoLists(l1,l2.next);
            return l2;
        }
    }
}
```











### <font  size=5 color="red">025.  k个一组反转链表</font>

给你链表的头节点 `head` ，每 `k` 个节点一组进行翻转，请你返回修改后的链表。

`k` 是一个正整数，它的值小于或等于链表的长度。如果节点总数不是 `k` 的整数倍，那么请将最后剩余的节点保持原有顺序。

你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex1.jpg)

```
输入：head = [1,2,3,4,5], k = 2
输出：[2,1,4,3,5]
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex2.jpg)

```
输入：head = [1,2,3,4,5], k = 3
输出：[3,2,1,4,5]
```

 

**提示：**

- 链表中的节点数目为 `n`
- `1 <= k <= n <= 5000`
- `0 <= Node.val <= 1000`



> **思路1  自己   `O(1)` **
>
>   反转k个,重新循环,使用计数器
>   结束时,若计数器不等于k,再将最后的反转回来

> **思路2  栈**

> **反思**
>
> 行22-28  ==达到条件后要立即执行,否则while结束后可能有最后一组需要结尾,如果这些放在最前,如果刚好结束,这些还需要再运行一遍==

 

**进阶：**你可以设计一个只用 `O(1)` 额外内存空间的算法解决此问题吗？



**思路1  自己   `O(1)` **

```java
    public ListNode reverseKGroup(ListNode head, int k) {

        ListNode preHead = new ListNode(); // 记录头结点的前一个
        ListNode pre = preHead, temp, cur = head;
        ListNode laterk = pre; // 上一个段落反转后的尾结点
        ListNode preNext = new ListNode(); // 当前反转段落的头结点

        int n = 0; // 记录已经反转的个数

        while(cur != null){           
            if(n == 0){
                preNext = cur;
            } 

            temp = cur.next;
            cur.next = pre;
            pre = cur;
            cur = temp;
            n++;

            // 反转k个后,局部初始化
            if(n == k){
                n = 0;
                laterk.next = pre;
                laterk = preNext;
                preNext = null;               
            }            
        }
        
        // 结束时,若计数器不等于k,再将最后的反转回来
        if(n != k){
            while(n != 0){
                n--;
                temp = pre.next;
                pre.next = cur;
                cur = pre;
                pre = temp;
            }
            laterk.next = preNext;              
        }

        return preHead.next;
    }
```





### <font  size=5 color="red">142.  环形链表2</font>

给定一个链表的头节点  `head` ，返回链表开始入环的第一个节点。 *如果链表无环，则返回 `null`。*

如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 `pos` 来表示链表尾连接到链表中的位置（**索引从 0 开始**）。如果 `pos` 是 `-1`，则在该链表中没有环。**注意：`pos` 不作为参数进行传递**，仅仅是为了标识链表的实际情况。

**不允许修改** 链表。



 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

```
输入：head = [3,2,0,-4], pos = 1
输出：返回索引为 1 的链表节点
解释：链表中有一个环，其尾部连接到第二个节点。
```

**示例 2：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test2.png)

```
输入：head = [1,2], pos = 0
输出：返回索引为 0 的链表节点
解释：链表中有一个环，其尾部连接到第一个节点。
```

**示例 3：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test3.png)

```
输入：head = [1], pos = -1
输出：返回 null
解释：链表中没有环。
```

 

**提示：**

- 链表中节点的数目范围在范围 `[0, 104]` 内
- `-105 <= Node.val <= 105`
- `pos` 的值为 `-1` 或者链表中的一个有效索引



> **思路 1  hashSet**

> **思路2  ==快慢指针==**
>
>    // 用快慢指针
>    // 如果不成环,二者会遇到null
>    // 如果成环.二者相遇时相等
>    // 列等式可得,当第三个指针从头开始一次一步,慢指针也一次一步,当相遇时就是环开端
>
> ==注意10行 结束条件  应该将范围大的结束条件放在while后,而可能提前结束的小范围的的结束条件应该放在while内,否则小的不结束循环就不会结束==

**思路 1  hashSet**

```java
        HashSet<ListNode> set = new HashSet<ListNode>();
        ListNode temp = head;
        while(temp != null){
            if(set.contains(temp)){
                return temp;
            }else{
                set.add(temp);
                temp = temp.next;
            }            
        }

        return null;       
    }
```

**思路2  ==快慢指针==**

```java
    public ListNode detectCycle(ListNode head) {

        if(head == null){
            return null;
        }
        
        ListNode l1 = head;
        ListNode l2 = head;

        while(l2.next != null && (l2 = l2.next.next) != null){
           l1 = l1.next;
           if(l1 == l2){
               l2 = head;
               while(l1 != l2){
                   l1 = l1.next;
                   l2 = l2.next;
               }
               return l1;
           }
        }
        
       return null;       
    }
```



### <font  size=5 color="red">146.  LRU缓存</font>

请你设计并实现一个满足 [LRU (最近最少使用) 缓存](https://baike.baidu.com/item/LRU) 约束的数据结构。

实现 `LRUCache` 类：

- `LRUCache(int capacity)` 以 **正整数** 作为容量 `capacity` 初始化 LRU 缓存
- `int get(int key)` 如果关键字 `key` 存在于缓存中，则返回关键字的值，否则返回 `-1` 。
- `void put(int key, int value)` 如果关键字 `key` 已经存在，则变更其数据值 `value` ；如果不存在，则向缓存中插入该组 `key-value` 。如果插入操作导致关键字数量超过 `capacity` ，则应该 **逐出** 最久未使用的关键字。

函数 `get` 和 `put` 必须以 `O(1)` 的平均时间复杂度运行。

**示例：**

```
输入
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
输出
[null, null, null, 1, null, -1, null, -1, 3, 4]

解释
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // 缓存是 {1=1}
lRUCache.put(2, 2); // 缓存是 {1=1, 2=2}
lRUCache.get(1);    // 返回 1
lRUCache.put(3, 3); // 该操作会使得关键字 2 作废，缓存是 {1=1, 3=3}
lRUCache.get(2);    // 返回 -1 (未找到)
lRUCache.put(4, 4); // 该操作会使得关键字 1 作废，缓存是 {4=4, 3=3}
lRUCache.get(1);    // 返回 -1 (未找到)
lRUCache.get(3);    // 返回 3
lRUCache.get(4);    // 返回 4
```

 

**提示：**

- `1 <= capacity <= 3000`
- `0 <= key <= 10000`
- `0 <= value <= 105`
- 最多调用 `2 * 105` 次 `get` 和 `put`



> **思路1  自己  哈希表储存数据,链表标记最就未使用的节点**
>
> 1067ms   太久了
>
> 单向链表
>
> 在更新链表的时候需要遍历链表花费时间开销较大
>
> **注意**  如果单向链表尾节点有存放数据需要先加入再删除,防止原节点是尾节点导致尾节点失效

> **思路1  优化  双向链表,哈希表value存节点**
>
> 双向链表方便知道节点的时候直接修改,不需要通过遍历获得前置节点
>
> 哈希表value存放节点方便节点直接修改不用遍历

**思路1  优化**

```java
class LRUCache {
    int capacity;
    int num; // 统计节点数目
    HashMap<Integer, Node> map;
    Node head; // 伪头结点,不存放数据
    Node tail; // 伪尾节点

    public LRUCache(int capacity) {
        this.capacity = capacity;
        num = 0;
        map = new HashMap<Integer, Node>(capacity + 1);
        head = new Node();
        tail = new Node();
        head.next = tail;
        tail.pre = head;
    }
    
    public int get(int key) {
        if(map.containsKey(key)){
            Node cur = map.get(key);
            // 在链表中删除该节点
            cur.next.pre = cur.pre;
            cur.pre.next = cur.next;
            // 在尾部中插入该节点
            cur.pre = tail.pre;
            cur.next = tail;
            cur.pre.next = cur;
            tail.pre = cur; 
            return cur.value; 
        }else return -1;        
    }
    
    public void put(int key, int value) {
        if(map.containsKey(key)){
            Node cur = map.get(key);
            cur.value = value;
            // 在链表中删除该节点
            cur.next.pre = cur.pre;
            cur.pre.next = cur.next;
            // 在尾部中插入该节点
            cur.pre = tail.pre;
            cur.next = tail;
            cur.pre.next = cur;
            tail.pre = cur;         
        }else{
            Node cur = new Node(key, value);
            map.put(key, cur);
            num++;
            if(num > capacity){
                map.remove(head.next.key);
                head.next.next.pre = head;
                head.next = head.next.next; 
            }
            // 在尾部插入新节点
            cur.pre = tail.pre;
            cur.next = tail;
            cur.pre.next = cur;
            tail.pre = cur;
        }
    }
}
    class Node{
        int value;
        int key;
        Node next;
        Node pre;
        public Node(){}
        public Node(int key, int value){
            this.key = key;
            this.value = value;
        }
    }

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
```

**思路1  自己**

```java
class LRUCache {
    int capacity;
    int num; // 统计元素数目
    HashMap<Integer, Integer> map;
    Node head; // 伪头结点,不存放数据
    Node tail; // 尾节点,指向最后一个



    public LRUCache(int capacity) {
        this.capacity = capacity;
        num = 0;
        map = new HashMap<Integer, Integer>(capacity + 1);
        head = new Node();
        tail = head;
    }
    
    public int get(int key) {
        if(map.containsKey(key)){
            tail.next = new Node(key);
            tail = tail.next;
            Node temp = head;
            while(temp.next.key != key){
                temp = temp.next;
            }
            temp.next = temp.next.next;
            return map.get(key); 
        }else return -1;        
    }
    
    public void put(int key, int value) {
        if(map.containsKey(key)){
            map.put(key, value);
            // 先插入该节点至结尾,防止该节点原本是尾节点
            tail.next = new Node(key);
            tail = tail.next;
            // 删除旧节点
            Node temp = head;
            while(temp.next.key != key){
                temp = temp.next;
            }
            temp.next = temp.next.next;            
        }else{
            map.put(key, value);
            num++;
            tail.next = new Node(key);
            tail = tail.next;
            if(num > capacity){
                map.remove(head.next.key);
                head.next = head.next.next; 
            }            
        }
    }

    private class Node{
        int value;
        int key;
        Node next;
        public Node(){}
        public Node(int key) {this.key = key;}
    }
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
```





### <font  size=5 color="red">206. 反转链表</font>

给你单链表的头节点 `head` ，请你反转链表，并返回反转后的链表。

```
输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]
```

**示例 2：**

```
输入：head = [1,2]
输出：[2,1]
```

**示例 3：**

```
输入：head = []
输出：[]
```

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
```

> **思路1  (自己) 数组列表储存  超出内存**  
>
> 列表储存,反向加入
>
> **超出内存原因**
>
> 思路复杂,可以直接破坏原链表

> **思路2 (自己)  迭代  死循环  ** 
>
> 将下一个的next指向向前一个
>
> **死循环原因**
>
> ==指针指向混乱==,node2.next一旦赋值,就无法调用原node2下一个,见14,15,16行node2未后移而是向前指向node1
>
> **解决方法**
>
> 引入新变量暂时储存,见3的第6行

> **思路3 迭代 优化正确的思路2开头**
>
> 思路12有误,头结点不为空
>
> 对2过程进行优化,步骤简化,效率比2的开头少一点点,可忽略不计

> **思路4  递归  ==妙==** 

****

**思路1  (自己) 数组储存  超出内存**

```java
public ListNode reverseList(ListNode head) {
        ArrayList<Integer> list = new ArrayList<Integer>();
        ListNode temp = head.next;
        while(temp != null){
            list.add(temp.val);
        }

        temp = head.next;
        for(int i = list.size() - 1; i >= 0; i--){
            temp.val = list.get(i);
            temp = temp.next;
        }

        temp = null;
        return head;
    }
```

**思路2 (自己)  迭代  超时  **

```java
public ListNode reverseList(ListNode head) {
        ListNode node1; //用来储存节点
        ListNode node2; //用来储存节点1的下一个节点
        
        node1 = head.next;
        // 排除为空的情况
        if(node1 == null){
            return head;
        }
        node2 = node1.next;
        node1.next = null;

        while(node2 != null){
            node2.next = node1;
            node2 = node2.next;
            node1 = node2;
        }
    //  while(node2 != null){ // 纠错
    //		  ListNode temp = node2.next; 
    //        node2.next = node1;
    //        node1 = node2;
    //		  node2 = temp;
    //    }

        head.next = node1;
        return head;
    }
```

**思路3 迭代 优化思路2**

```java
    public ListNode reverseList(ListNode head) {
        ListNode pre = null; //用来储存节点
        ListNode cur = head; //用来储存节点1的下一个节点
        
        while(cur!= null){
            ListNode temp = cur.next;
            cur.next = pre;
            pre = cur;
            cur = temp;
        }

        return pre;
    }
```

**思路4  递归  妙** 

```java
public:
    ListNode* reverseList(ListNode head) {
        // 链表为空时直接返回，链表不为空则到返回最后一个节点
        if(head == null || head.next == null) {
            return head;
        }
        // newHead先指向最后一个节点，注意此时参数是倒数第二个节点
        // 这一步很精妙，每一次newHead都是指向空指针（链表为空）或保留在原链表中的最后一个节点（链表不空），作用就是返回新的头结点
        ListNode newHead = reverseList(head.next);
        // 最后一个节点指向倒数第二个节点
        head.next.next = head;
        // 倒数第二个节点的下一节点置空。此时倒数第三个节点仍指向倒数第二个节点，下一次递归中将倒数第二个节点下一节点指向倒数第三个节点，不断重复这一过程
        head.next = null;
        return newHead;
    }
```

### <font  size=5 color="red">234.  回文链表</font>

给你一个单链表的头节点 `head` ，请你判断该链表是否为回文链表。如果是，返回 `true` ；否则，返回 `false` 。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/03/03/pal1linked-list.jpg)

```
输入：head = [1,2,2,1]
输出：true
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/03/03/pal2linked-list.jpg)

```
输入：head = [1,2]
输出：false
```

 

**提示：**

- 链表中节点数目在范围`[1, 105]` 内
- `0 <= Node.val <= 9`



> **思路1  自己  将值放入数组中**
>
> 时间O(n),空间O(n)  8ms 50%

> **思路2  快慢指针 **
>
> 反转前半部分,迭代比较
>
> **错误**
>
> 21行  当只有两个数,当快指针未动而慢指针移动,l1.next已经变向
>
> **纠错**
>
> 先判断结束再修改快慢指针和反转
>
> **反思**
>
> ==while循环,结束条件不在括号内时,结束条件的判断要放在最前或最后,防止忽略部分循环内容的变化影响结束的判断==
>
> ==之前写多条并列的if有类似错误,前面的if语句中的内容对后面if的判断产生影响==



**思路1  自己  将值放入数组中**

```java
    public boolean isPalindrome(ListNode head) {
        ArrayList<Integer> list = new ArrayList<Integer>();
        while(head != null){
            list.add(head.val);
            head = head.next;
        }

        for(int i = 0, j = list.size() - 1; i < j; i++, j--){
            if(list.get(i) != list.get(j)){
                return false;
            }
        }

       return true;
    }
```

**思路2  快慢指针  有小错误 **

```java
    public boolean isPalindrome(ListNode head) {
        ListNode l1 = head; // 快指针
        ListNode l2 = head; // 慢指针
        ListNode l3; // 用作比较时后半段的开始节点
        ListNode l4; // 比较时的前半段的开始节点
        ListNode pre = null;
        ListNode temp;

        if(head == null){
            return true;
        }

        while(true){
            // 反转,慢指针移动
            temp = l2;
            l2 = l2.next;
            temp.next = pre;
            pre = temp;
            // 判断是否结束
            // 奇数个
            if(l1.next == null){
                l3 = l2;
                l4 = pre.next;
                break;
            }
            // 偶数个
            if(l1.next.next == null){
                l3 = l2;
                l4 = pre;
                break;
            }
            // 快指针移动
            l1 = l1.next.next;
        }

        while(l3 != null){
            if(l3.val != l4.val){
                return false;
            }
            l3 = l3.next;
            l4 = l4.next;
        }

        return true;
    }
```

**思路2  快慢指针  改错**

```java
    public boolean isPalindrome(ListNode head) {
        ListNode pre = null;
        ListNode slow = head;
        ListNode fast = head;
        while(fast != null && fast.next != null){
            ListNode temp = slow.next;
            if(pre != null) {
                slow.next = pre;
            }
            pre = slow;
            fast = fast.next.next;
            slow = temp;
        }
        if(fast != null) slow = slow.next;
        while(slow != null){
            if(slow.val != pre.val) return false;
            slow = slow.next;
            pre = pre.next;
        }
        return true;
    }
```







### <font  size=5 color="red">160. 相交链表</font>

给你两个单链表的头节点 `headA` 和 `headB` ，请你找出并返回两个单链表相交的起始节点。如果两个链表不存在相交节点，返回 `null` 。

图示两个链表在节点 `c1` 开始相交**：**

[![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)

题目数据 **保证** 整个链式结构中不存在环。

**注意**，函数返回结果后，链表必须 **保持其原始结构** 。

**自定义评测：**

**评测系统** 的输入如下（你设计的程序 **不适用** 此输入）：

- `intersectVal` - 相交的起始节点的值。如果不存在相交节点，这一值为 `0`
- `listA` - 第一个链表
- `listB` - 第二个链表
- `skipA` - 在 `listA` 中（从头节点开始）跳到交叉节点的节点数
- `skipB` - 在 `listB` 中（从头节点开始）跳到交叉节点的节点数

评测系统将根据这些输入创建链式数据结构，并将两个头节点 `headA` 和 `headB` 传递给你的程序。如果程序能够正确返回相交节点，那么你的解决方案将被 **视作正确答案** 。

> **思路1 散列集(自己)**  5ms
>
> 散列集储存链表1,用链表2中元素一一判断

> **思路2 双指针 ==妙==**  空间O(1) 1ms
>
> 双指分别指向两个开头
>
> 若不等,都向后,向后遇到null就指向另一个开头
>
> 当相等时,如果有交点,就是交点,没交点,就是两个都到头为null

> **思路3  想过 未深入 ** 1ms 
>
> 先求两个长度,较长的先走差值,之后一起走的就是可能一样的

> **总结**
>
> 该题==查找用时要大于统计用时,所以思路3和1相比虽然遍历两遍,但用时短==



**思路1 散列集(自己)**

```java
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        HashSet<ListNode> nodeSet = new HashSet<ListNode>();
        
        ListNode temp = new ListNode();
        temp = headA;
        while(temp != null){
            nodeSet.add(temp);
            temp = temp.next;
        }

        temp = headB;
        while(temp != null){
            if(nodeSet.contains(temp)){
                return temp;
           }
           temp = temp.next;
        }

        return null;
    }
```

**思路2 双指针 ==妙==**  空间O(1)

```java
 public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if (headA == null || headB == null) return null;
        
        ListNode la = headA, lb = headB;
        
        while(la != lb){
            la = la == null ? headB : la.next;
            lb = lb == null ? headA : lb.next;
        }

        return la;
    }
```

**思路3  想过 未深入  时间短**

```java
		//headA链表长度
        int aLength = 0;
        //headB链表长度
        int bLength = 0;

        //链表长度的差值
        int moveStep = 0;
        //headA 遍历的节点
        ListNode aItem = headA;

        //headB 遍历的节点
        ListNode bItem = headB;
        while (null != aItem) {
            //headA链表长度
            aLength++;
            aItem = aItem.next;
        }
        while (null != bItem) {
            //headB链表长度
            bLength++;
            bItem = bItem.next;
        }

        //重置遍历的链表
        aItem = headA;
        bItem = headB;

        if (aLength > bLength) {
            //headA 先走moveStep步长
            moveStep = aLength - bLength;
            while (moveStep > 0) {
                aItem = aItem.next;
                moveStep--;
            }
        }

        if (aLength < bLength) {
            moveStep = bLength - aLength;
            while (moveStep > 0) {
                bItem = bItem.next;
                moveStep--;
            }
        }

        while (bItem!=aItem) {
            //只要不相等，就各自走下去
            aItem = aItem.next;
            bItem = bItem.next;
        }

        return aItem;
```



# 堆

### <font  size=5 color="red">347.  前k个高频元素</font>

给你一个整数数组 `nums` 和一个整数 `k` ，请你返回其中出现频率前 `k` 高的元素。你可以按 **任意顺序** 返回答案。

 

**示例 1:**

```
输入: nums = [1,1,1,2,2,3], k = 2
输出: [1,2]
```

**示例 2:**

```
输入: nums = [1], k = 1
输出: [1]
```

 

**提示：**

- `1 <= nums.length <= 105`
- `k` 的取值范围是 `[1, 数组中不相同的元素的个数]`
- 题目数据保证答案唯一，换句话说，数组中前 `k` 个高频元素的集合是唯一的



> **思路1  自己  优先队列 **
>
> 想到了,但是未写,代码细节很多
>
> 6行  ==改写 `m.merge(num, 1, Integer::sum);`==
>
> 11行 ==键值对遍历==
>
> 12行 ==数组的构造方式==

> **思路2  有个数的优先队列**
>
> 队列数目小于k,直接加入,等于k,比较后选择是否取代

**思路1  自己  优先队列 **

```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> m = new HashMap();

        for (int num : nums) {
            m.put(num, m.getOrDefault(num, 0) + 1);
        }

        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> b[0] - a[0]);

        for (Map.Entry<Integer, Integer> entry: m.entrySet()) {
            pq.offer(new int[]{entry.getValue(), entry.getKey()});
        }

        int[] ans = new int[k];
        
        for (int i = 0; i < k; i++) ans[i] = pq.poll()[1];

        return ans;
    }
}
```





# 栈

### <font  size=5 color="red">020.  有效的括号</font>

给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串 `s` ，判断字符串是否有效。

有效字符串需满足：

1. 左括号必须用相同类型的右括号闭合。
2. 左括号必须以正确的顺序闭合。
3. 每个右括号都有一个对应的相同类型的左括号。

 

**示例 1：**

```
输入：s = "()"
输出：true
```

**示例 2：**

```
输入：s = "()[]{}"
输出：true
```

**示例 3：**

```
输入：s = "(]"
输出：false
```

 

**提示：**

- `1 <= s.length <= 104`
- `s` 仅由括号 `'()[]{}'` 组成



> **思路1  栈  题目理解错误**
>
> 碰到一个左括号就压入一个右括号，碰到右括号就弹出 判断是否相等
>
> 注意==返回值二选一代码的简化==

**思路1  栈  题目理解错误**

```java
    public boolean isValid(String s) {
        //碰到一个左括号就压入一个右括号，碰到右括号就弹出 判断是否相等
        Stack<Character> stack=new Stack<>();
        char[] charArray=s.toCharArray();
        char ch;
        for(int i=0;i<charArray.length;i++){
            if(ch =='('){
                stack.push(')');
            }else if(ch == '['){
                stack.push(']');
            }else if(ch == '{'){
                stack.push('}');
            }else{
                if(stack.size()==0){
                    return false;
                }
                char res=stack.pop();
                if(res!=charArray[i]){
                    return false;
                }
            }
        }
 
        return stack.size() == 0;
    }
```

### <font  size=5 color="red">084.  柱状图的最大的矩形(未解出)</font>

给定 *n* 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。

求在该柱状图中，能够勾勒出来的矩形的最大面积。

 

**示例 1:**

![img](LeetCode刷题.assets/histogram.jpg)

```
输入：heights = [2,1,5,6,2,3]
输出：10
解释：最大的矩形为图中红色区域，面积为 10
```

**示例 2：**

![img](LeetCode刷题.assets/histogram-1.jpg)

```
输入： heights = [2,4]
输出： 4
```

 

**提示：**

- `1 <= heights.length <=105`
- `0 <= heights[i] <= 104`



> **思路1  自己  二位动态规划  O(n2)**
>
> dp记录以每个点为开头和结尾的中最小高度
>
> **超时**

> **思路2  单调栈 O(n) **
>
> 记录每个点左右第一个严格小于自己的横坐标
>
> 左右个找一次

> **思路2  优化  单调栈  哨兵元素**
>
> 确定一个柱形的面积的时候，**除了右边要比当前严格小**，**左边也要比当前高度严格小**
>
> ==存放时,如果当前高度大于等于之前的高度,就直接存放,则上一个高度就是该高度对应的左边界==
>
> ==遍历时,如果当前的高度值比上一个严格小,就能直接确定上一个的右边界,==此时上一个元素对应的左右边界都能确定下来,开始取出上一个元素进行计算可能的最大值
>
> (注意)  当当前高度和上一个高度相同时也直接存放,由于只需要知道最大值,两个高度相等的直接存放,这两个高度对应的最大值实际上时相等的,但只需要最大值,最左边的高度能求出最大值而右边的求得的值不是最大值也不影响最后的结果
>
> 左右各加入一个哨兵元素,对结果无影响
>
> 左哨兵:置0,则左哨兵不会出栈(栈内元素比当前元素大才会出栈) 不需要判断栈为空的情况
>
> 右哨兵:置0,把之前的除了高度为0的元素全部出栈,计算可能的最大值
>
> ==缓存数据的时候，是从左向右缓存的，计算出一个结果的顺序是从右向左的，并且计算完成以后我们就不再需要了，符合后进先出的特点,使用栈==

> **反思**
>
> ==单调栈的经典应用场景是，在一维数组中，对每一个数字，找到前/后面第一个比自己大/小的元素==

**思路2  单调栈  优化**

```java
public class Solution {

    public int largestRectangleArea(int[] heights) {
        int len = heights.length;
        // 剪枝
        if (len == 0) {
            return 0;
        }

        if (len == 1) {
            return heights[0];
        }

        int res = 0;

        int[] newHeights = new int[len + 2];
        newHeights[0] = 0;
        System.arraycopy(heights, 0, newHeights, 1, len);
        newHeights[len + 1] = 0;
        len += 2;
        heights = newHeights;

        Deque<Integer> stack = new ArrayDeque<>(len);
        // 先放入哨兵，在循环里就不用做非空判断
        stack.addLast(0);
        
        for (int i = 1; i < len; i++) {
            while (heights[i] < heights[stack.peekLast()]) {
                int curHeight = heights[stack.pollLast()];
                int curWidth = i - stack.peekLast() - 1;
                res = Math.max(res, curHeight * curWidth);
            }
            stack.addLast(i);
        }
        return res;
    }

作者：liweiwei1419
链接：https://leetcode.cn/problems/largest-rectangle-in-histogram/solutions/266844/zhu-zhuang-tu-zhong-zui-da-de-ju-xing-by-leetcode-/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```



**思路2  单调栈 O(n) **

```java
class Solution {
    public int largestRectangleArea(int[] heights){
        int length = heights.length;
        int[] left = new int[length];
        int[] right = new int[length];
        int ans = 0; 
        int num = 0;

        Deque<Integer> stack = new ArrayDeque<Integer>(); // 内部存放索引
        // 找到右边第一个小于自己的值的前一个值和自己的索引差,需要从左边遍历
        for(int i = 0; i < length; i++){
            while(true){
                if(stack.isEmpty() || heights[i] >= heights[stack.peek()]){
                    stack.push(i);
                    break;
                }
                num = stack.poll();
                right[num] = i - num - 1;                
            }
        }
        while(!stack.isEmpty()){
            num = stack.poll();
            right[num] = length - 1 - num;
        }

        stack.clear();
        // 找到左边第一个小于自己的值的后一个值和自己的索引差,从右边开始遍历
        for(int i = length - 1; i >= 0; i--){
            while(true){
                if(stack.isEmpty() || heights[i] >= heights[stack.peek()]){
                    stack.push(i);
                    break;
                }
                num = stack.poll();
                left[num] = num - i - 1;
            }
        }
        while(!stack.isEmpty()){
            num = stack.poll();
            left[num] = num - 0;
        }

        for(int i = 0; i < length; i++){
            ans = Math.max(ans, (left[i] + right[i] + 1) * heights[i]);
        }

        return ans;
    }
}
```

**思路1  自己  二位动态规划  O(n2)**

```java
class Solution {
    public int largestRectangleArea(int[] heights) {
        int length = heights.length;
        int ans = 0;
        int[][] dp = new int[length][length];
        for(int i = 1; i <= length; i++){
            dp[i - 1][i - 1] = heights[i - 1];
            ans = Math.max(ans, heights[i - 1]);
            for(int j = i - 2; j >= 0; j--){
                dp[i - 1][j] = Math.min(dp[i - 1][j + 1], dp[i - 2][j]);
                ans = Math.max(ans, dp[i - 1][j] * (i - j));
            }
        }
        return ans;
    }
}
```



### <font  size=5 color="red">394.  字符串解码</font>

给定一个经过编码的字符串，返回它解码后的字符串。

编码规则为: `k[encoded_string]`，表示其中方括号内部的 `encoded_string` 正好重复 `k` 次。注意 `k` 保证为正整数。

你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。

此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数 `k` ，例如不会出现像 `3a` 或 `2[4]` 的输入。

 

**示例 1：**

```
输入：s = "3[a]2[bc]"
输出："aaabcbc"
```

**示例 2：**

```
输入：s = "3[a2[c]]"
输出："accaccacc"
```

**示例 3：**

```
输入：s = "2[abc]3[cd]ef"
输出："abcabccdcdcdef"
```

**示例 4：**

```
输入：s = "abc3[cd]xyz"
输出："abccdcdcdxyz"
```

 

**提示：**

- `1 <= s.length <= 30`
- `s` 由小写英文字母、数字和方括号 `'[]'` 组成
- `s` 保证是一个 **有效** 的输入。
- `s` 中所有整数的取值范围为 `[1, 300]` 



> **思路1  自己  递归**  
>
> 遇到'['开始递归,']'结束递归,开始前将[前的加入StringBuilder(注意不要加入数字)
>
> 遇到'['开始递归后,遍历字符数组时,i要调到递归结束的']'的后面
> 最终结束要将]后面的加入StringBuilder中
>
> 计算数字时要注意数字可能不止1个

> **思路1  优化**
>
> 基本思路一样
>
> 从前到后遍历
>
> 是数字就统计数字
>
> 是[就进入递归,递归后i移动到递归结束的位置(递归结束]的索引作为返回值返回),累加递归得到到的字符串
>
> 是]就结束递归
>
> 由于最外层无[和]所以最终结束时只需要返回1个元素即可

> **反思**
>
> 和020有一些区别,由于遇到 [ 后,就可以直接对 [ 后面的元素进行处理,并不需要再次获取[ 所以只需要递归即可,不需要使用栈



**思路1 递归 优化**

```java
class Solution {
    public String decodeString(String s) {
        return dfs(s, 0)[0];
    }
    
    private String[] dfs(String s, int i) {
        StringBuilder res = new StringBuilder();
        int multi = 0;
        while(i < s.length()) {
            // 当遇到数字,一定是连续的数字,数字后面就是要成倍加入的[]中的内容
            // 数字和[]轮流出现
            if(s.charAt(i) >= '0' && s.charAt(i) <= '9') 
                multi = multi * 10 + Integer.parseInt(String.valueOf(s.charAt(i))); 
            else if(s.charAt(i) == '[') {
                String[] tmp = dfs(s, i + 1);
                // 将i放到递归结束时]所在的位置,结束时i++就会自动到下一个位置
                i = Integer.parseInt(tmp[0]);
                // 将[]内内容multi次加入 StringBuilder res,结束后mulit会归零
                while(multi > 0) {
                    res.append(tmp[1]);
                    multi--;
                }
            }
            else if(s.charAt(i) == ']') 
                // 递归结束后,返回String[] 数组第一个元素使]结束对应的索引的字符串
                return new String[] { String.valueOf(i), res.toString() };
            else 
                // 如果是单个字母,就直接加入 StringBuilder res
                res.append(String.valueOf(s.charAt(i)));
            i++;
        }
        // 对于最外层的递归,由于不会遇到]只需要返回包含一个元素的String[]即可
        return new String[] { res.toString() };
    } 
}
```

**思路2  双栈**

```java
   public String decodeString(String s) {
		StringBuffer ans=new StringBuffer();
		Stack<Integer> multiStack=new Stack<>();
		Stack<StringBuffer> ansStack=new Stack<>();
		int multi=0;
		for(char c:s.toCharArray()){
			if(Character.isDigit(c))multi=multi*10+c-'0';
			else if(c=='['){
				ansStack.add(ans);
				multiStack.add(multi);
				ans=new StringBuffer();
				multi=0;
			}else if(Character.isAlphabetic(c)){
				ans.append(c);
			}else{
				StringBuffer ansTmp=ansStack.pop();
				int tmp=multiStack.pop();
				for(int i=0;i<tmp;i++)ansTmp.append(ans);
				ans=ansTmp;
			}
		}
		return ans.toString();
	}
```



**思路2  栈**

```java
public String decodeString(String s) {
        
        Stack<Character> stack = new Stack<>();
        
        for(char c : s.toCharArray())
        {
            if(c != ']') 
                stack.push(c); // 把所有的字母push进去，除了]
            
            else 
            {
                //step 1: 取出[] 内的字符串
                
                StringBuilder sb = new StringBuilder();
                while(!stack.isEmpty() && Character.isLetter(stack.peek()))
                    sb.insert(0, stack.pop());
                
                String sub = sb.toString(); //[ ]内的字符串
                stack.pop(); // 去除[
                
                
                //step 2: 获取倍数数字
                    
                sb = new StringBuilder();
                while(!stack.isEmpty() && Character.isDigit(stack.peek()))
                    sb.insert(0, stack.pop());
                    
                int count = Integer.valueOf(sb.toString()); //倍数
                
                
                //step 3: 根据倍数把字母再push回去
                
                while(count > 0)
                {
                    for(char ch : sub.toCharArray())  
                        stack.push(ch);
                    count--;
                }
            }
        }
        
      //把栈里面所有的字母取出来，完事L('ω')┘三└('ω')｣
        StringBuilder retv = new StringBuilder();
        while(!stack.isEmpty())
            retv.insert(0, stack.pop());

        return retv.toString();
    }
```



**思路1  自己  递归**  

```java
class Solution {
    String str;
    int startIndex = 0;
    public String decodeString(String s) {
        char[] charList = s.toCharArray();
        StringBuilder ans = new StringBuilder();
        Deque<Integer> stack = new ArrayDeque<Integer>(); // 存放左括号的索引

        // 利用栈储存左括号在charList的索引,记录开始的索引,遇到左括号将之前的加入StringBuilder开始递归
        return new String(decodeString(charList));
    }

    // startIndex初始为0,表示左括号内第一个字符的索引,当有字符加入StringBuilder,startIndex就应该更新
    private StringBuilder decodeString(char[] charList){
        int len = charList.length;
        StringBuilder ans = new StringBuilder();
        // 遇到[开始进入递归,当遇到]递归结束,返回[]内字符的StringBuilder和]的索引(用来更新startIndex)
        for(int i = startIndex; i < len; i++){
            if(charList[i] == '['){
                // 求出[前的数字tempNum,tempNum可能不是个位数
                int startIndexOfTempNum = i - 1;
                while(startIndexOfTempNum >= 0 && (charList[startIndexOfTempNum] - '0' <= 9 && charList[startIndexOfTempNum] - '0' >= 0)) startIndexOfTempNum--;
                startIndexOfTempNum++;
                int tempNum = Integer.parseInt(new String(charList, startIndexOfTempNum, i - 1 - startIndexOfTempNum + 1));

                ans.append(charList, startIndex, startIndexOfTempNum - 1 - startIndex + 1);
                startIndex = i + 1;
                // 获取并更新下一个小括号
                StringBuilder temp = decodeString(charList)
                while(tempNum-- != 0) ans.append(temp);

                // i跳转到该[对应的]后
                i = startIndex - 1;

            }else if(charList[i] == ']'){
                // 结束时,将]前未加入的字符加入ans,返回ans
                ans.append(charList, startIndex, i - 1 - startIndex + 1);
                startIndex = i + 1;
                break;
            }else if(i == len - 1){
                ans.append(charList, startIndex, i - startIndex + 1);
            }
        }
        return ans;
    }
}
```



### <font  size=5 color="red">739. 每日温度</font>

给定一个整数数组 `temperatures` ，表示每天的温度，返回一个数组 `answer` ，其中 `answer[i]` 是指对于第 `i` 天，下一个更高温度出现在几天后。如果气温在这之后都不会升高，请在该位置用 `0` 来代替。

**示例 1:**

```
输入: temperatures = [73,74,75,71,69,72,76,73]
输出: [1,1,4,2,1,1,0,0]
```

**示例 2:**

```
输入: temperatures = [30,40,50,60]
输出: [1,1,1,0]
```

**示例 3:**

```
输入: temperatures = [30,60,90]
输出: [1,1,0]
```

**提示：**

- `1 <= temperatures.length <= 105`
- `30 <= temperatures[i] <= 100`

> **思路1  (自己) 动态规划  反向查找  用时最短8ms**
>
> 反向求值,如果下一个值比当前大,就得到结果
>
> 如果比当前小,就直接找比下一个大的值比较,没有就是0

> **思路2  栈  用时24ms**
>
> 有序栈,小的放入,有大的一直取,直到遇到更大的

> **总结**
>
> 每个值需要和后面的相比,我们需要知道每个值和后面的大小关系
>
> ==思路2 局部的动态规划思想==,降序栈为数据结构,适合寻找局域的大值
>
> ==栈的使用,部分连续满足某种条件.如降序栈,排列局域的降序==
>
> 使用有序栈储存已经比较过的,遇到比栈内大的就可以确定站内的,所以使用降序栈 
>
> ==思路1 全局的动态规划==

**思路一  (自己) 动态规划  反向查找  用时最短**

```java
    public int[] dailyTemperatures(int[] temperatures) {
        int[] days = new int[temperatures.length];
        days[temperatures.length - 1] = 0;
        int temp; // 暂时储存索引

        // 从反方向查找
        for(int i = temperatures.length - 2; i >= 0; i--){
            temp = 1;
            while(i + temp < temperatures.length){
                if(temperatures[i + temp] > temperatures[i]){
                    days[i] = temp;
                    break;
                }else if(days[i + temp] != 0){
                    temp+=days[i + temp];
                }else{
                    days[i] = 0;
                    break;
                }
            }
        }

        return days;
    }
```

**思路2  栈  用时24ms**

```java
    public int[] dailyTemperatures(int[] temperatures) {
        int length = temperatures.length;
        int[] ans = new int[length];
        Deque<Integer> stack = new LinkedList<Integer>();

        for(int i = 0; i < temperatures.length; i++){
            int temp = temperatures[i];
            while(!stack.isEmpty() && temp > temperatures[stack.peek()]){
                int preIndex = stack.pop();
                ans[preIndex] = i - preIndex;
            }
            stack.push(i);
        }

        return ans;
    }
```





# 矩阵

### <font  size=5 color="red">054.  螺旋矩阵</font>

给你一个 `m` 行 `n` 列的矩阵 `matrix` ，请按照 **顺时针螺旋顺序** ，返回矩阵中的所有元素。

 

**示例 1：**

![img](LeetCode刷题.assets/spiral1.jpg)

```
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,2,3,6,9,8,7,4,5]
```

**示例 2：**

![img](LeetCode刷题.assets/spiral.jpg)

```
输入：matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
输出：[1,2,3,4,8,12,11,10,9,5,6,7]
```

 

**提示：**

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 10`
- `-100 <= matrix[i][j] <= 100`



> **思路1  **
>
> 遍历,遇到边界就开始转向,无法转向时就结束
>
> 按右下左上的顺序转圈
>
> 初始(-1, 0),先移动格点,到达边界就回退,上来就到达格点就结束

```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> list = new ArrayList<Integer>();
        int x = -1, y = 0; // 初始索引
        int right = matrix[0].length, left = -1, up = 0, down = matrix.length; // 遇到边界就应该改变方向
        while(true){
            // 当无法转向时结束

            // 向右
            x++;
            if(x >= right) break;
            while(x < right){
                list.add(matrix[y][x]);
                x++;
            }
            x--; // x回到矩阵内
            right--;


            // 向下
            y++;
            if(y >= down) break;
            while(y < down){
                list.add(matrix[y][x]);
                y++;
            }
            y--;
            down--;

            // 向左
            x--;
            if(x <= left) break;
            while(x > left){
                list.add(matrix[y][x]);
                x--;
            }
            x++; 
            left++;

            // 向上
            y--;
            if(y <= up) break;
            while(y > up){
                list.add(matrix[y][x]);
                y--;
            }
            y++;
            up++;
        }

        return list;
    }
}
```







### <font  size=5 color="red">073.  矩阵置0</font>

给定一个 `*m* x *n*` 的矩阵，如果一个元素为 **0** ，则将其所在行和列的所有元素都设为 **0** 。请使用 **[原地](http://baike.baidu.com/item/原地算法)** 算法**。**



 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/08/17/mat1.jpg)

```
输入：matrix = [[1,1,1],[1,0,1],[1,1,1]]
输出：[[1,0,1],[0,0,0],[1,0,1]]
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2020/08/17/mat2.jpg)

```
输入：matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]
输出：[[0,0,0,0],[0,4,5,0],[0,3,1,0]]
```

 

**提示：**

- `m == matrix.length`
- `n == matrix[0].length`
- `1 <= m, n <= 200`
- `-231 <= matrix[i][j] <= 231 - 1`

> **思路1  自己  未写**
>
> 用两个数组储存每行每列是否有0
>
> 最后将特定的行或列置0

> **思路2  原地算法**
>
> ==用原数据的第一行和第一列记录数据==
>
> 用两个布尔值标记第一行和第一列是否有0
>
> 如果每行或每列有0,就将第一行或第一列的数字标0
>
> 根两种据标记置0

**思路2  原地算法**

```java
    public void setZeroes(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        boolean flagCol0 = false, flagRow0 = false;

        for(int i = 0; i < m; i++){
            if (matrix[i][0] == 0) {
                flagCol0 = true;
            }
        }
                for (int j = 0; j < n; j++) {
            if (matrix[0][j] == 0) {
                flagRow0 = true;
            }
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][j] == 0) {
                    matrix[i][0] = matrix[0][j] = 0;
                }
            }
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                    matrix[i][j] = 0;
                }
            }
        }
        if (flagCol0) {
            for (int i = 0; i < m; i++) {
                matrix[i][0] = 0;
            }
        }
        if (flagRow0) {
            for (int j = 0; j < n; j++) {
                matrix[0][j] = 0;
            }
        }
    }
```



# 数组

### <font  size=5 color="red">041.  缺失的第一个整数(未原地算法)</font>

给你一个未排序的整数数组 `nums` ，请你找出其中没有出现的最小的正整数。

请你实现时间复杂度为 `O(n)` 并且只使用常数级别额外空间的解决方案。

 

**示例 1：**

```
输入：nums = [1,2,0]
输出：3
```

**示例 2：**

```
输入：nums = [3,4,-1,1]
输出：2
```

**示例 3：**

```
输入：nums = [7,8,9,11,12]
输出：1
```

 

**提示：**

- `1 <= nums.length <= 5 * 105`
- `-231 <= nums[i] <= 231 - 1`



> **思路1  自己  哈希表**
>
> 时间复杂O(n)但很耗时间

> **思路2  原地哈希**
>
> 方式1,让nums[i]回到索引nums[i]的位置
>
> 行5,只有当该索引位置数大于0,小于nums.length ,要交换的位置不是已经放置好的数时才需要交换(防止有重复的数交替交换进入死循环),本身是否满足条件不加也可以,因为会被第四个条件包含,加了更符合思考逻辑
>
> 方式2,让nums[i]回到索引为nums[i] - 1的位置



**思路2  原地哈希 方式1**

```java
class Solution {
    public int firstMissingPositive(int[] nums) {
        for(int i = 0; i < nums.length; i++){
            int temp;
            while(nums[i] > 0 && nums[i] != i && nums[i] <= nums.length - 1 &&nums[nums[i]] != nums[i]){
                temp = nums[nums[i]];
                nums[nums[i]] = nums[i];
                nums[i] = temp;
            }
        }
        for(int i = 1; i < nums.length; i++){
            if(nums[i] != i){
                return i;                
            } 
        }
        return nums[0] == nums.length? nums.length + 1:nums.length;
    }
}
```

**思路1  自己  哈希表**

```java
class Solution {
    public int firstMissingPositive(int[] nums) {
        HashSet<Integer> set = new HashSet<>();
        int min = Integer.MAX_VALUE;
        for(int num: nums){
            if(num > 0){
                set.add(num);
                min = Math.min(min, num);
            }
        }

        if(min != 1){
            return 1;
        }else{
            min++;
            while(set.contains(min)) min++;
        }
        return min;
    }
}
```









### <font  size=5 color="red">053.  最大子数组和</font>

给你一个整数数组 `nums` ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

**子数组** 是数组中的一个连续部分。

 

**示例 1：**

```
输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
输出：6
解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。
```

**示例 2：**

```
输入：nums = [1]
输出：1
```

**示例 3：**

```
输入：nums = [5,4,-1,7,8]
输出：23
```

 

**提示：**

- `1 <= nums.length <= 105`
- `-104 <= nums[i] <= 104`

 

**进阶：**如果你已经实现复杂度为 `O(n)` 的解法，尝试使用更为精妙的 **分治法** 求解。



> **思路1  自己  动态规划**
>
> 没增加一个负数就进行考虑,是否要添加这个负数
>
> **改进细节**
>
> *f*(*i*) 代表以第 i* 个数结尾的「连续子数组的最大和」
>
> 因此我们只需要求出每个位置的 f(i)，然后返回 *f* 数组中的最大值即可。
>
> *f*(*i* ) = max { *f*(*i*−1) + *nums*[*i*] , *nums*[*i*] }

> **思路2  分治法**
>
> 对于一个区间 [l,r]，我们可以维护四个量：
>
> lSum 表示[l,r] 内以l 为左端点的最大子段和
> rSum 表示 [l,r] 内以r 为右端点的最大子段和
> mSum 表示 [l,r] 内的最大子段和
> iSum 表示[l,r] 的区间和
>
> 对于[l,r] 的lSum，存在两种可能，它要么等于「左子区间」的 lSum，要么等于「左子区间」的 iSum 加上「右子区间」的 lSum，二者取大。
>
> 

**思路1  自己  动态规划**

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int max = nums[0], temp = 0;
        for(int i = 0; i < nums.length; i++){
            if(temp + nums[i] < 0){
                max = Math.max(max, nums[i]);
                temp = 0;
            }else{
                temp = temp + nums[i];
                max = Math.max(max, temp);
            }
        }
        return max;
    }
}
```

**思路1  改进细节**

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int pre = 0, maxAns = nums[0];
        for (int x : nums) {
            pre = Math.max(pre + x, x);
            maxAns = Math.max(maxAns, pre);
        }
        return maxAns;
    }
}
```

**思路2  分治法**

```java
class Solution {
    public class Status {
        public int lSum, rSum, mSum, iSum;

        public Status(int lSum, int rSum, int mSum, int iSum) {
            this.lSum = lSum;
            this.rSum = rSum;
            this.mSum = mSum;
            this.iSum = iSum;
        }
    }

    public int maxSubArray(int[] nums) {
        return getInfo(nums, 0, nums.length - 1).mSum;
    }

    public Status getInfo(int[] a, int l, int r) {
        if (l == r) {
            return new Status(a[l], a[l], a[l], a[l]);
        }
        int m = (l + r) >> 1;
        Status lSub = getInfo(a, l, m);
        Status rSub = getInfo(a, m + 1, r);
        return pushUp(lSub, rSub);
    }

    public Status pushUp(Status l, Status r) {
        int iSum = l.iSum + r.iSum;
        int lSum = Math.max(l.lSum, l.iSum + r.lSum);
        int rSum = Math.max(r.rSum, r.iSum + l.rSum);
        int mSum = Math.max(Math.max(l.mSum, r.mSum), l.rSum + r.lSum);
        return new Status(lSum, rSum, mSum, iSum);
    }
}
```

### <font  size=5 color="red">189.  轮转数组</font>

给定一个整数数组 `nums`，将数组中的元素向右轮转 `k` 个位置，其中 `k` 是非负数。

 

**示例 1:**

```
输入: nums = [1,2,3,4,5,6,7], k = 3
输出: [5,6,7,1,2,3,4]
解释:
向右轮转 1 步: [7,1,2,3,4,5,6]
向右轮转 2 步: [6,7,1,2,3,4,5]
向右轮转 3 步: [5,6,7,1,2,3,4]
```

**示例 2:**

```
输入：nums = [-1,-100,3,99], k = 2
输出：[3,99,-1,-100]
解释: 
向右轮转 1 步: [99,-1,-100,3]
向右轮转 2 步: [3,99,-1,-100]
```

 

**提示：**

- `1 <= nums.length <= 105`
- `-231 <= nums[i] <= 231 - 1`
- `0 <= k <= 105`

 

**进阶：**

- 尽可能想出更多的解决方案，至少有 **三种** 不同的方法可以解决这个问题。
- 你可以使用空间复杂度为 `O(1)` 的 **原地** 算法解决这个问题吗？



> **思路1  自己  用临时数组储存**

> **思路2  自己  循环赋值**
>
> bug
>
> 未注意k为length公约数情况

> **思路3  反转数组**

**思路3  反转数组**

```java
class Solution {
    public void rotate(int[] nums, int k) {
        k %= nums.length;
        reverse(nums, 0, nums.length - 1);
        reverse(nums, 0, k - 1);
        reverse(nums, k, nums.length - 1);
    }

    public void reverse(int[] nums, int start, int end) {
        while (start < end) {
            int temp = nums[start];
            nums[start] = nums[end];
            nums[end] = temp;
            start += 1;
            end -= 1;
        }
    }
}
```

**思路2  自己  循环赋值**

```java
class Solution {
    public void rotate(int[] nums, int k) {
        int n = nums.length;
        k = k % n;
        int count = gcd(k, n);
        for (int start = 0; start < count; ++start) {
            int current = start;
            int prev = nums[start];
            do {
                int next = (current + k) % n;
                int temp = nums[next];
                nums[next] = prev;
                prev = temp;
                current = next;
            } while (start != current);
        }
    }

    public int gcd(int x, int y) {
        return y > 0 ? gcd(y, x % y) : x;
    }
}
```



**思路1  自己  用临时数组储存**

```java
class Solution {
    public void rotate(int[] nums, int k) {
        k = k % nums.length;
        int[] ans = new int[nums.length];
        int num = 0;
        for(int i = nums.length - k; i < nums.length; i++) ans[num++] = nums[i];
        for(int i = 0; i < nums.length - k; i++) ans[num++] = nums[i];
        System.arraycopy(ans, 0, nums, 0, nums.length);
        // 不通过,由于未修改原nums, nums = ans;
    }
}
```





### <font  size=5 color="red">238.  除自身以外数组的乘积(未解出)</font>

给你一个整数数组 `nums`，返回 *数组 `answer` ，其中 `answer[i]` 等于 `nums` 中除 `nums[i]` 之外其余各元素的乘积* 。

题目数据 **保证** 数组 `nums`之中任意元素的全部前缀元素和后缀的乘积都在 **32 位** 整数范围内。

请 **不要使用除法，**且在 `O(*n*)` 时间复杂度内完成此题。

 

**示例 1:**

```
输入: nums = [1,2,3,4]
输出: [24,12,8,6]
```

**示例 2:**

```
输入: nums = [-1,1,0,-3,3]
输出: [0,0,9,0,0]
```

 

**提示：**

- `2 <= nums.length <= 105`
- `-30 <= nums[i] <= 30`
- **保证** 数组 `nums`之中任意元素的全部前缀元素和后缀的乘积都在 **32 位** 整数范围内

 

**进阶：**你可以在 `O(1)` 的额外空间复杂度内完成这个题目吗？（ 出于对空间复杂度分析的目的，输出数组 **不被视为** 额外空间。）



> **思路1**
>
> 求出每个数组元素左右的积,分别用两个数组储存,最后相乘

> **思路2**
>
> 用输出数组暂时储存

> **反思**
>
> ==未能把握特征,每个数和相邻得数相乘都有很大一部分相同的,以此为突破口,两侧遍历求左或右边的积==

**思路1**

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int length = nums.length;

        // L 和 R 分别表示左右两侧的乘积列表
        int[] L = new int[length];
        int[] R = new int[length];

        int[] answer = new int[length];

        // L[i] 为索引 i 左侧所有元素的乘积
        // 对于索引为 '0' 的元素，因为左侧没有元素，所以 L[0] = 1
        L[0] = 1;
        for (int i = 1; i < length; i++) {
            L[i] = nums[i - 1] * L[i - 1];
        }

        // R[i] 为索引 i 右侧所有元素的乘积
        // 对于索引为 'length-1' 的元素，因为右侧没有元素，所以 R[length-1] = 1
        R[length - 1] = 1;
        for (int i = length - 2; i >= 0; i--) {
            R[i] = nums[i + 1] * R[i + 1];
        }

        // 对于索引 i，除 nums[i] 之外其余各元素的乘积就是左侧所有元素的乘积乘以右侧所有元素的乘积
        for (int i = 0; i < length; i++) {
            answer[i] = L[i] * R[i];
        }

        return answer;
    }
}
```

**思路2**

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int length = nums.length;
        int[] answer = new int[length];

        // answer[i] 表示索引 i 左侧所有元素的乘积
        // 因为索引为 '0' 的元素左侧没有元素， 所以 answer[0] = 1
        answer[0] = 1;
        for (int i = 1; i < length; i++) {
            answer[i] = nums[i - 1] * answer[i - 1];
        }

        // R 为右侧所有元素的乘积
        // 刚开始右边没有元素，所以 R = 1
        int R = 1;
        for (int i = length - 1; i >= 0; i--) {
            // 对于索引 i，左边的乘积为 answer[i]，右边的乘积为 R
            answer[i] = answer[i] * R;
            // R 需要包含右边所有的乘积，所以计算下一个结果时需要将当前值乘到 R 上
            R *= nums[i];
        }
        return answer;
    }
}
```



# 二叉树

### <font  size=5 color="red">094.  二叉树的中序遍历</font>

给定一个二叉树的根节点 `root` ，返回 *它的 **中序** 遍历* 。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/09/15/inorder_1.jpg)

```
输入：root = [1,null,2,3]
输出：[1,3,2]
```

**示例 2：**

```
输入：root = []
输出：[]
```

**示例 3：**

```
输入：root = [1]
输出：[1]
```

 

**提示：**

- 树中节点数目在范围 `[0, 100]` 内
- `-100 <= Node.val <= 100`



> **思路1  递归  自己**
>
> **思路1  改细节  简练代码**
>
> 判断为空直接return

> **思路2  迭代  死循环**
>
> 12行 已经遍历过的左子节点会不断加入栈造成死循环
>
> **==思路2  迭代== **
>
> 左子节点有就一直指向左子节点,没有就取出,然后指向右子节点,
>
> 下一个指向为空就直接取出

> **反思**
>
> ==Queue 先进先出 Deque 先进后出==
>
> ==递归不可以用迭代,看是否需要回溯,回溯就不可以,见022==

**思路1  递归**

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        if(root == null){
            return new ArrayList<Integer>();
        }

        List<Integer> list = new ArrayList<Integer>();
        inorderTraversal(list, root);
        return list;
    }

    public void inorderTraversal(List<Integer> list, TreeNode node){
        if(node.left != null){
            inorderTraversal(list, node.left);
        }

        list.add(node.val);

        if(node.right != null){
            inorderTraversal(list, node.right);
        }
    }
}
```

**思路1  改细节  简练代码**

```java
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        inorder(root, res);
        return res;
    }

    public void inorder(TreeNode root, List<Integer> res) {
        if (root == null) {
            return;
        }
        inorder(root.left, res);
        res.add(root.val);
        inorder(root.right, res);
    }
```

**思路2  迭代  死循环**

```java
       public List<Integer> inorderTraversal(TreeNode root) {
        Deque<TreeNode> stack = new LinkedList<TreeNode>();
        List<Integer> list = new ArrayList<>();
        if(root == null){
            return list;
        }
        TreeNode temp;;
        stack.push(root);

        while(!stack.isEmpty()){
            temp = stack.peek();
            if(temp.left != null){
                stack.push(temp.left);
            }else{
                stack.poll();
                list.add(temp.val);
                if(temp.right != null){
                    stack.push(temp.right);
                }
            }
        }
        return list;
    }
```

**思路2  迭代**

```java
public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        Deque<TreeNode> stk = new LinkedList<TreeNode>();
        while (root != null || !stk.isEmpty()) {
            while (root != null) {
                stk.push(root);
                root = root.left;
            }
            root = stk.pop();
            res.add(root.val);
            root = root.right;
        }
        return res;
    }
```



### <font  size=5 color="red">101.  对称二叉树</font>

给你一个二叉树的根节点 `root` ， 检查它是否轴对称。

 

**示例 1：**

![img](LeetCode刷题.assets/1698026966-JDYPDU-image.png)

```
输入：root = [1,2,2,3,4,4,3]
输出：true
```

**示例 2：**

![img](LeetCode刷题.assets/1698027008-nPFLbM-image.png)

```
输入：root = [1,2,2,null,3,null,3]
输出：false
```

 

**提示：**

- 树中节点数目在范围 `[1, 1000]` 内
- `-100 <= Node.val <= 100`

 

**进阶：**你可以运用递归和迭代两种方法解决这个问题吗？



> **思路1  自己  bfs  递归**
>
> 前序遍历,两颗子树一块遍历

> **思路2  迭代  队列**
>
> 先进先出

**思路1  自己  bfs  递归**

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if(root == null) return true;
        return traverse(root.left, root.right);
    }
    public boolean traverse(TreeNode node1, TreeNode node2){
        if(node1 == null && node2 == null) return true;
        if(node1 == null || node2 == null) return false;
        if(node1.val != node2.val) return false;
        return traverse(node1.left, node2.right) && traverse(node1.right, node2.left);        
    }    
}
```

**思路2  迭代  队列**

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        return check(root, root);
    }

    public boolean check(TreeNode u, TreeNode v) {
        Queue<TreeNode> q = new LinkedList<TreeNode>();
        q.offer(u);
        q.offer(v);
        while (!q.isEmpty()) {
            u = q.poll();
            v = q.poll();
            if (u == null && v == null) {
                continue;
            }
            if ((u == null || v == null) || (u.val != v.val)) {
                return false;
            }

            q.offer(u.left);
            q.offer(v.right);

            q.offer(u.right);
            q.offer(v.left);
        }
        return true;
    }
}
```





### <font  size=5 color="red">199.  二叉树的右视图</font>

给定一个二叉树的 **根节点** `root`，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。

 

**示例 1:**

![img](https://assets.leetcode.com/uploads/2021/02/14/tree.jpg)

```
输入: [1,2,3,null,5,null,4]
输出: [1,3,4]
```

**示例 2:**

```
输入: [1,null,3]
输出: [1,3]
```

**示例 3:**

```
输入: []
输出: []
```

 

**提示:**

- 二叉树的节点个数的范围是 `[0,100]`
- `-100 <= Node.val <= 100` 



> **思路1  自己  DFS  前序遍历,先右后左**
>
> 标记当前看到的最大的层数num和当前节点的层数curnum
>
> 当curnum > num  加入节点的值,num++

> **思路2  ==BFS  值得一看==**
>
> 使用队列  先进先出  不断加入每一层  统计每一层的尺寸
>
> 到下一层,取出一个节点,加入该节点的不为空的子节点,取到最后一个时,就统计它的值



**思路1  自己  前序遍历,先右后左**

```java
class Solution {
    int num = -1,curnum = -1; // 当前看到的最大的层数和当前节点的层数
    List<Integer> list;
    public List<Integer> rightSideView(TreeNode root) {
        list = new ArrayList<Integer>();
        if(root != null){
            rightSide(root);
        }
        
        return list;
    }

    private void rightSide(TreeNode root){
        curnum++;
        if(curnum > num){
            list.add(root.val);
            num++;
        }
        if(root.right != null){
            rightSide(root.right);
            curnum--;
        }
        if(root.left != null){
            rightSide(root.left);
            curnum--;
        }
    }
}
```

**思路2  BFS  值得一看**

```java
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if (root == null) {
            return res;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                if (node.left != null) {
                    queue.offer(node.left);
                }
                if (node.right != null) {
                    queue.offer(node.right);
                }
                if (i == size - 1) {  //将当前层的最后一个节点放入结果列表
                    res.add(node.val);
                }
            }
        }
        return res;
    }
}
```



### <font  size=5 color="red">226.  反转二叉树</font>

给你一棵二叉树的根节点 `root` ，翻转这棵二叉树，并返回其根节点。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/03/14/invert1-tree.jpg)

```
输入：root = [4,2,7,1,3,6,9]
输出：[4,7,2,9,6,3,1]
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/03/14/invert2-tree.jpg)

```
输入：root = [2,1,3]
输出：[2,3,1]
```

**示例 3：**

```
输入：root = []
输出：[]
```

 

**提示：**

- 树中节点数目范围在 `[0, 100]` 内
- `-100 <= Node.val <= 100`



> **思路1  自己  前序遍历  递归**
>
> **思路1  后序遍历  递归  变递归细节**
>
> ==注意,只能前序或者后序,中序左右无法交换==

**思路1  自己  前序遍历  递归**

```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        invert(root);
        return root;
    }
    
    public void invert(TreeNode node){
        if(node == null){
            return;
        }
        TreeNode temp = node.right;
        node.right = node.left;
        node.left = temp;
        invert(node.right);
        invert(node.left);        
    }
}
```

**思路1    后序遍历  递归  变递归细节**

```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if (root == null) {
            return null;
        }
        TreeNode left = invertTree(root.left);
        TreeNode right = invertTree(root.right);
        root.left = right;
        root.right = left;
        return root;
    }
}
```



### <font  size=5 color="red">230.  二叉搜索树中第k小的元素</font>

给定一个二叉搜索树的根节点 `root` ，和一个整数 `k` ，请你设计一个算法查找其中第 `k` 个最小元素（从 1 开始计数）。

 

**示例 1：**

![img](LeetCode刷题.assets/kthtree1.jpg)

```
输入：root = [3,1,4,null,2], k = 1
输出：1
```

**示例 2：**

![img](LeetCode刷题.assets/kthtree2.jpg)

```
输入：root = [5,3,6,2,4,null,null,1], k = 3
输出：3
```



> **思路1  自己  优先级队列**
>
> 不足,忽略了二叉搜索树就是二叉排序树的特性

> **思路2  哈希表储存子节点个数**



**思路2  哈希表储存子节点个数**

```java
class Solution {
    public int kthSmallest(TreeNode root, int k) {
        MyBst bst = new MyBst(root);
        return bst.kthSmallest(k);
    }
}

class MyBst {
    TreeNode root;
    Map<TreeNode, Integer> nodeNum;

    public MyBst(TreeNode root) {
        this.root = root;
        this.nodeNum = new HashMap<TreeNode, Integer>();
        countNodeNum(root);
    }

    // 返回二叉搜索树中第k小的元素
    public int kthSmallest(int k) {
        TreeNode node = root;
        while (node != null) {
            int left = getNodeNum(node.left);
            if (left < k - 1) {
                node = node.right;
                k -= left + 1;
            } else if (left == k - 1) {
                break;
            } else {
                node = node.left;
            }
        }
        return node.val;
    }

    // 统计以node为根结点的子树的结点数
    private int countNodeNum(TreeNode node) {
        if (node == null) {
            return 0;
        }
        nodeNum.put(node, 1 + countNodeNum(node.left) + countNodeNum(node.right));
        return nodeNum.get(node);
    }

    // 获取以node为根结点的子树的结点数
    private int getNodeNum(TreeNode node) {
        return nodeNum.getOrDefault(node, 0);
    }
}
```

**思路1  自己  优先级队列**

```java
class Solution {
    public int kthSmallest(TreeNode root, int k) {
        PriorityQueue<TreeNode> queue = new PriorityQueue<TreeNode>((t1,t2) -> t1.val - t2.val);
        bianli(queue, root);
        for(int i = 0; i < k - 1; i++) queue.poll();
        return queue.poll().val;
    }

    private void bianli(PriorityQueue<TreeNode> queue, TreeNode root){
        if(root != null){
            queue.add(root);
            bianli(queue, root.right);
            bianli(queue, root.left);
        } 
        
    }
}
```



### <font  size=5 color="red">236.  二叉树的最近公共祖先(未解出)</font>

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

[百度百科](https://baike.baidu.com/item/最近公共祖先/8918834?fr=aladdin)中最近公共祖先的定义为：“对于有根树 T 的两个节点 p、q，最近公共祖先表示为一个节点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（**一个节点也可以是它自己的祖先**）。”

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
输入：root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出：3
解释：节点 5 和节点 1 的最近公共祖先是节点 3 。
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
输入：root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
输出：5
解释：节点 5 和节点 4 的最近公共祖先是节点 5 。因为根据定义最近公共祖先节点可以为节点本身。
```

**示例 3：**

```
输入：root = [1,2], p = 1, q = 2
输出：1
```

 

**提示：**

- 树中节点数目在范围 `[2, 105]` 内。
- `-109 <= Node.val <= 109`
- 所有 `Node.val` `互不相同` 。
- `p != q`
- `p` 和 `q` 均存在于给定的二叉树中。



> **思路1  自己  中序遍历  bug**
>
> 借助栈迭代中序遍历,如果发现就用栈存储,最后从地下比较两个栈
>
> bug
>
> 中序遍历在最上面的遍历后会出栈,应该采用后序遍历

> **思路2 递归**

> **思路3  遍历  HashMap**

**思路1  自己  中序遍历  bug**

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        int num = 0; //统计找到的节点的数目
        Deque<TreeNode> deque = new LinkedList<TreeNode>();
        Deque<TreeNode> deque1 = new LinkedList<TreeNode>();
        TreeNode temp = root; // 辅助遍历

        while(temp != null){
            deque.offer(temp);
            if(temp == p || temp ==q){
                num++;
                if(num == 1){
                    deque1 = new  LinkedList<TreeNode>(deque);
                }
                if(num == 2){
                    break;
                }
            }
            if(temp.left != null){
                temp = temp.left;
            }else if(temp.right != null){
                temp = temp.right;
            }else{
                while(deque.peek() != null){
                    temp = deque.poll();
                }
                temp = temp.right;
            }
        }

        TreeNode node1 = new TreeNode();

        node1 = deque1.pollLast();
        TreeNode node2 = deque.pollLast();
        while(node1 != node2){
            node1 = deque1.pollLast();
            node2 = deque.pollLast();
        }
        return node1;        
    }
}
```

**思路2 递归**

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null || root == p || root == q) {
            //只要当前根节点是p和q中的任意一个，就返回（因为不能比这个更深了，再深p和q中的一个就没了）
            return root;
        }
        //根节点不是p和q中的任意一个，那么就继续分别往左子树和右子树找p和q
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        //p和q都没找到，那就没有
        if(left == null && right == null) {
            return null;
        }
        //左子树没有p也没有q，就返回右子树的结果
        if (left == null) {
            return right;
        }
        //右子树没有p也没有q就返回左子树的结果
        if (right == null) {
            return left;
        }
        //左右子树都找到p和q了，那就说明p和q分别在左右两个子树上，所以此时的最近公共祖先就是root
        return root;
    }
}
```

**思路3  遍历  HashMap**

```java
class Solution {
    Map<Integer, TreeNode> parent = new HashMap<Integer, TreeNode>();
    Set<Integer> visited = new HashSet<Integer>();

    public void dfs(TreeNode root) {
        if (root.left != null) {
            parent.put(root.left.val, root);
            dfs(root.left);
        }
        if (root.right != null) {
            parent.put(root.right.val, root);
            dfs(root.right);
        }
    }

    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        dfs(root);
        while (p != null) {
            visited.add(p.val);
            p = parent.get(p.val);
        }
        while (q != null) {
            if (visited.contains(q.val)) {
                return q;
            }
            q = parent.get(q.val);
        }
        return null;
    }
}
```



### <font  size=5 color="red">243.  二叉树的直径</font>

给你一棵二叉树的根节点，返回该树的 **直径** 。

二叉树的 **直径** 是指树中任意两个节点之间最长路径的 **长度** 。这条路径可能经过也可能不经过根节点 `root` 。

两节点之间路径的 **长度** 由它们之间边数表示。

 

**示例 1：**

![img](LeetCode刷题.assets/diamtree.jpg)

```
输入：root = [1,2,3,4,5]
输出：3
解释：3 ，取路径 [4,2,1,3] 或 [5,2,1,3] 的长度。
```

**示例 2：**

```
输入：root = [1,2]
输出：1
```

 

**提示：**

- 树中节点数目在范围 `[1, 104]` 内
- `-100 <= Node.val <= 100`



> **思路1  自己  DFS深度优先遍历**
>
> 递归遍历,返回子节点到本节点的最大距离,而以该节点为根节点的最大直径就和左右子节点的返回值之和有关

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    int max = 0;
    public int diameterOfBinaryTree(TreeNode root) {
        traverse(root);
        return max;
    }

    private int traverse(TreeNode root){
        if(root == null) return -1;
        int left = traverse(root.left);
        int right = traverse(root.right);
        max = Math.max(max, left + right + 2);
        return Math.max(++left, ++right);
    }
}
```







# 图论

### <font  size=5 color="red">208.  实现Trie(前缀树)(不理解题目)</font>

**Trie**（发音类似 "try"）或者说 **前缀树** 是一种树形数据结构，用于高效地存储和检索字符串数据集中的键。这一数据结构有相当多的应用情景，例如自动补完和拼写检查。

请你实现 Trie 类：

- `Trie()` 初始化前缀树对象。
- `void insert(String word)` 向前缀树中插入字符串 `word` 。
- `boolean search(String word)` 如果字符串 `word` 在前缀树中，返回 `true`（即，在检索之前已经插入）；否则，返回 `false` 。
- `boolean startsWith(String prefix)` 如果之前已经插入的字符串 `word` 的前缀之一为 `prefix` ，返回 `true` ；否则，返回 `false` 。

 

**示例：**

```
输入
["Trie", "insert", "search", "search", "startsWith", "insert", "search"]
[[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]
输出
[null, null, true, false, true, null, true]

解释
Trie trie = new Trie();
trie.insert("apple");
trie.search("apple");   // 返回 True
trie.search("app");     // 返回 False
trie.startsWith("app"); // 返回 True
trie.insert("app");
trie.search("app");     // 返回 True
```

 

**提示：**

- `1 <= word.length, prefix.length <= 2000`
- `word` 和 `prefix` 仅由小写英文字母组成
- `insert`、`search` 和 `startsWith` 调用次数 **总计** 不超过 `3 * 104` 次



> **思路1  将Trie[]设置为对象Trie的成员变量**

```java
class Trie {
    boolean isEnd;
    Trie[] children;

    public Trie() {
        children = new Trie[26];
    }
    
    public void insert(String word) {
        char[] chars = word.toCharArray();
        Trie temp = this;
        for(int i = 0; i < chars.length; i++){
            if(temp.children[chars[i] - 'a'] == null){
                temp.children[chars[i] - 'a'] = new Trie();
            }
            temp = temp.children[chars[i] - 'a'];
        }
        temp.isEnd = true;
    }
    
    public boolean search(String word) {
        char[] chars = word.toCharArray();
        Trie temp = this;
        for(int i = 0; i < chars.length; i++){
            if(temp.children[chars[i] - 'a'] == null) return false;
            temp = temp.children[chars[i] - 'a'];
        }

        return temp.isEnd == true;         
    }
    
    public boolean startsWith(String prefix) {
        char[] chars = prefix.toCharArray();
        Trie temp = this;
        for(int i = 0; i < chars.length; i++){
            if(temp.children[chars[i] - 'a'] == null) return false;
            temp = temp.children[chars[i] - 'a'];
        }
        return true;
    }
}

/**
 * Your Trie object will be instantiated and called as such:
 * Trie obj = new Trie();
 * obj.insert(word);
 * boolean param_2 = obj.search(word);
 * boolean param_3 = obj.startsWith(prefix);
 */
```



### <font  size=5 color="red">437.  路径总和(只会暴力)</font>

给定一个二叉树的根节点 `root` ，和一个整数 `targetSum` ，求该二叉树里节点值之和等于 `targetSum` 的 **路径** 的数目。

**路径** 不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。

 

**示例 1：**

![img](LeetCode刷题.assets/pathsum3-1-tree.jpg)

```
输入：root = [10,5,-3,3,2,null,11,3,-2,null,1], targetSum = 8
输出：3
解释：和等于 8 的路径有 3 条，如图所示。
```

**示例 2：**

```
输入：root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
输出：3
```

 

**提示:**

- 二叉树的节点个数的范围是 `[0,1000]`
- `-109 <= Node.val <= 109` 
- `-1000 <= targetSum <= 1000` 



> **思路1  自己  一次递归**
>
> 使用递归不断向下一层进行遍历,每次进入子节点就计算以该子节点为结尾是否有满足条件的路径

> **思路2  **



**思路1  自己  一次递归**

```java
class Solution {
    int ans = 0; // 统计结果
    int targetSum;
    public int pathSum(TreeNode root, int targetSum) {
        if(root == null) return 0;

        this.targetSum = targetSum;

        // 使用递归不断向下一层进行遍历,每次进入子节点就计算以该子节点为结尾是否有满足条件的路径
        ArrayList<Integer> valList = new ArrayList<Integer>(); // 存放以root为开头,某个新加入节点结尾的路径的各个节点的值
        digui(valList, 1, root);
        return ans;

    }

    /**
    int ceng 新加入的节点所在的层数,root节点层数为1
    TreeNode node 新加入的节点
     */
    private void digui(ArrayList<Integer> valList, int ceng, TreeNode node){

        // 将node.val 放入 valList
        if(ceng > valList.size()){
            valList.add(node.val);
        } else {
            valList.set(ceng - 1, node.val); 
        }

        // 计算是否有符合条件的路径
        long curSum = 0;
        for(int i = ceng - 1; i >= 0; i--){
            curSum += valList.get(i);
            if(curSum == targetSum){
                ans++;
            }
        }

        // 向左右子节点递归
        if(node.right != null){
            digui(valList, ceng + 1, node.right);
        }
        if(node.left != null){
            digui(valList, ceng + 1, node.left);
        }

    }
}
```







### <font  size=5 color="red">994.  腐烂的橘子</font>

在给定的 `m x n` 网格 `grid` 中，每个单元格可以有以下三个值之一：

- 值 `0` 代表空单元格；
- 值 `1` 代表新鲜橘子；
- 值 `2` 代表腐烂的橘子。

每分钟，腐烂的橘子 **周围 4 个方向上相邻** 的新鲜橘子都会腐烂。

返回 *直到单元格中没有新鲜橘子为止所必须经过的最小分钟数。如果不可能，返回 `-1`* 。

 

**示例 1：**

**![img](LeetCode刷题.assets/oranges.png)**

```
输入：grid = [[2,1,1],[1,1,0],[0,1,1]]
输出：4
```

**示例 2：**

```
输入：grid = [[2,1,1],[0,1,1],[1,0,1]]
输出：-1
解释：左下角的橘子（第 2 行， 第 0 列）永远不会腐烂，因为腐烂只会发生在 4 个正向上。
```

**示例 3：**

```
输入：grid = [[0,2]]
输出：0
解释：因为 0 分钟时已经没有新鲜橘子了，所以答案就是 0 。
```

 

**提示：**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 10`
- `grid[i][j]` 仅为 `0`、`1` 或 `2`



> **思路1  自己  队列储存烂橘子**
>
> 找所有烂橘子和好橘子的数量
>
> 循环,好橘子数量归零时结束
>
> 取出坏橘子,加入一分钟后可能会出现的坏橘子
>
> 循环结束前进行判断,如果没加入新的坏橘子就直接返回值



**思路1  自己  队列储存烂橘子**

```java
class Solution {
    public int orangesRotting(int[][] grid) {
        int length = grid.length, length1 = grid[0].length;
        int badNum = 0, ans = 0, goodNum = 0;
        Deque<int[]> bad = new LinkedList<>();
        HashSet<int[]> good = new HashSet<>();
        for(int i = 0; i < length; i++){
            for(int j = 0; j < length1; j++){
                if(grid[i][j] == 1) goodNum++;
                if(grid[i][j] == 2) bad.push(new int[] {i, j});
            }
        }

        badNum = bad.size();
        while(goodNum != 0){
            int[] curbad;
            int getBadNum = 0;
            int x, y;
            for(int i = 0; i < badNum; i++){
                curbad = bad.pollLast();
                x = curbad[0];
                y = curbad[1];
                if(y + 1 < length1 && grid[x][y + 1] == 1){
                    getBadNum++;
                    bad.push(new int[] {x, y + 1});
                    grid[x][y + 1] = 2;
                }
                if(y - 1 > -1 && grid[x][y - 1] == 1){
                    getBadNum++;
                    bad.push(new int[] {x, y - 1});
                    grid[x][y - 1] = 2;
                }
                if(x + 1 < length && grid[x + 1][y] == 1){
                    getBadNum++;
                    bad.push(new int[] {x + 1, y});
                    grid[x + 1][y] = 2;
                }
                if(x - 1 > -1 && grid[x - 1][y] == 1){
                    getBadNum++;
                    bad.push(new int[] {x - 1, y});
                    grid[x - 1][y] = 2;
                }
            }
            if(getBadNum == 0) return -1;
            goodNum -= getBadNum;
            ans++;
            badNum = bad.size();         
        }
        return ans;
    }
}
```





# 二分查找

### <font  size=5 color="red">004.  寻找两个正序数组的中位数(未解出)</font>

给定两个大小分别为 `m` 和 `n` 的正序（从小到大）数组 `nums1` 和 `nums2`。请你找出并返回这两个正序数组的 **中位数** 。

算法的时间复杂度应该为 `O(log (m+n))` 。

 

**示例 1：**

```
输入：nums1 = [1,3], nums2 = [2]
输出：2.00000
解释：合并数组 = [1,2,3] ，中位数 2
```

**示例 2：**

```
输入：nums1 = [1,2], nums2 = [3,4]
输出：2.50000
解释：合并数组 = [1,2,3,4] ，中位数 (2 + 3) / 2 = 2.5
```

 

 

**提示：**

- `nums1.length == m`
- `nums2.length == n`
- `0 <= m <= 1000`
- `0 <= n <= 1000`
- `1 <= m + n <= 2000`
- `-106 <= nums1[i], nums2[i] <= 106`



> **思路1**
>
> 从两个数组开头开始循环某个次数,开头数小的数组的索引++
>
> 时间不满足O(log(m + n))

> **思路2  自己  二分查找  太复杂,有bug**
>
> 寻找mid1,使得mid1左边有num个数,需要满足nums1[mid1] <= nums2[num - mid1] && nums1[mid1] > nums2[num - mid1 - 1]
>
> 若nums1[mid1] <= nums2[num - mid1] && nums1[mid1] < nums2[num - mid1 + 1],则right2 = num - mid1 - 1;
>
> 若nums1[mid1] < nums2[num - mid1],则left2 = num - mid1
>
> 寻找mid2
>
>  对前四步进行循环

> **思路3  二分查找**
>
> 从两个有序数组中找到第n大的数
>
> 从两个数组中分别找出第n/2个数进行比较,排除较小的数和该数之前的,(如果剩余长度小于n/2就排除剩余长度)
>
> 如果相等就排除开头小的那个
>
> 一直排除到排除结束或者某一个数组到头(排除的数最小值要为1)
>
> **迭代  自己**
>
> 改进  8行  temp = (mid + 1) / 2, 当mid为奇数就是中位数,mid为偶数就是两个中卫数的前
>
> **迭代  官方**

**思路3  迭代  官方**

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int length1 = nums1.length, length2 = nums2.length;
        int totalLength = length1 + length2;
        if (totalLength % 2 == 1) {
            int midIndex = totalLength / 2;
            double median = getKthElement(nums1, nums2, midIndex + 1);
            return median;
        } else {
            int midIndex1 = totalLength / 2 - 1, midIndex2 = totalLength / 2;
            double median = (getKthElement(nums1, nums2, midIndex1 + 1) + getKthElement(nums1, nums2, midIndex2 + 1)) / 2.0;
            return median;
        }
    }

    public int getKthElement(int[] nums1, int[] nums2, int k) {
        /* 主要思路：要找到第 k (k>1) 小的元素，那么就取 pivot1 = nums1[k/2-1] 和 pivot2 = nums2[k/2-1] 进行比较
         * 这里的 "/" 表示整除
         * nums1 中小于等于 pivot1 的元素有 nums1[0 .. k/2-2] 共计 k/2-1 个
         * nums2 中小于等于 pivot2 的元素有 nums2[0 .. k/2-2] 共计 k/2-1 个
         * 取 pivot = min(pivot1, pivot2)，两个数组中小于等于 pivot 的元素共计不会超过 (k/2-1) + (k/2-1) <= k-2 个
         * 这样 pivot 本身最大也只能是第 k-1 小的元素
         * 如果 pivot = pivot1，那么 nums1[0 .. k/2-1] 都不可能是第 k 小的元素。把这些元素全部 "删除"，剩下的作为新的 nums1 数组
         * 如果 pivot = pivot2，那么 nums2[0 .. k/2-1] 都不可能是第 k 小的元素。把这些元素全部 "删除"，剩下的作为新的 nums2 数组
         * 由于我们 "删除" 了一些元素（这些元素都比第 k 小的元素要小），因此需要修改 k 的值，减去删除的数的个数
         */

        int length1 = nums1.length, length2 = nums2.length;
        int index1 = 0, index2 = 0;
        int kthElement = 0;

        while (true) {
            // 边界情况
            if (index1 == length1) {
                return nums2[index2 + k - 1];
            }
            if (index2 == length2) {
                return nums1[index1 + k - 1];
            }
            if (k == 1) {
                return Math.min(nums1[index1], nums2[index2]);
            }
            
            // 正常情况
            int half = k / 2;
            int newIndex1 = Math.min(index1 + half, length1) - 1;
            int newIndex2 = Math.min(index2 + half, length2) - 1;
            int pivot1 = nums1[newIndex1], pivot2 = nums2[newIndex2];
            if (pivot1 <= pivot2) {
                k -= (newIndex1 - index1 + 1);
                index1 = newIndex1 + 1;
            } else {
                k -= (newIndex2 - index2 + 1);
                index2 = newIndex2 + 1;
            }
        }
    }
}
```

**思路3  迭代  自己**

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        if(nums1.length == 0) return (nums2[(nums2.length) / 2 ] + nums2[(nums2.length - 1) / 2])* 0.5;
        if(nums2.length == 0) return (nums1[(nums1.length) / 2 ] + nums1[(nums1.length - 1) / 2])* 0.5;
        int left1 = -1, left2 = -1, mid = (nums1.length + nums2.length) / 2, temp = 0;

        while(mid != 0){
            temp = Math.max(1, mid / 2); // 改进   temp = (mid + 1) / 2
            temp = Math.min(temp, nums1.length - left1 - 1); // left1是数组1排除的最后一个元素的索引
            temp = Math.min(temp, nums2.length - left2 - 1); // temp是下次将要排除的数目
            if(nums1[left1 + temp] < nums2[left2 + temp] || (nums1[left1 + temp] == nums2[left2 + temp] && nums1[left1 + 1] < nums2[left2 + 1])){
                left1 += temp;
            }else{
                left2 += temp;
            }
            mid -= temp;

            if(left1 == nums1.length - 1){
                if((nums1.length + nums2.length) % 2 == 1) return nums2[(nums1.length + nums2.length) / 2 - nums1.length] * 1.0;
                else{
                    int cur = nums1[nums1.length - 1];
                    if((nums1.length + nums2.length) / 2 - nums1.length - 1 >=0){
                        cur = Math.max(cur, nums2[(nums1.length + nums2.length) / 2 - nums1.length -1]);
                    }
                    return (nums2[(nums1.length + nums2.length) / 2 - nums1.length] + cur)* 0.5;
                }
            }
            if(left2 == nums2.length - 1){
                if((nums1.length + nums2.length) % 2 == 1) return nums1[(nums1.length + nums2.length) / 2 - nums2.length] * 1.0;
                else{
                    int cur = nums2[nums2.length - 1];
                    if((nums1.length + nums2.length) / 2 - nums2.length - 1 >=0){
                        cur = Math.max(cur, nums1[(nums1.length + nums2.length) / 2 - nums2.length -1]);
                    }
                    return (nums1[(nums1.length + nums2.length) / 2 - nums2.length] + cur)* 0.5;
                }
            }
        }
        if((nums1.length + nums2.length) % 2 == 1){
            if(nums1[left1 + 1] <= nums2[left2 + 1]){
                return nums1[left1 + 1];
            }else{
                return nums2[left2 + 1];
            }
        }
        // else return (nums1[left1 + 1] + nums2[left2 + 1])* 0.5;
        else{
            int n1, n2;
            if(nums1[left1 + 1] <= nums2[left2 + 1]){
                n1 = nums1[left1 + 1];
            }else {
                n1 = nums2[left2 + 1];
            }
            if(left1 >= 0 && left2 == -1){
                n2 = nums1[left1];
            }else if(left2 >= 0 && left1 == -1) n2 = nums2[left2];
            else n2 = Math.max(nums1[left1], nums2[left2]);
            return (n1 + n2) * 0.5;
        }
    }
}

```





### <font  size=5 color="red">0034.  在排序数组中查找元素第一个和最后一个位置</font>

给你一个按照非递减顺序排列的整数数组 `nums`，和一个目标值 `target`。请你找出给定目标值在数组中的开始位置和结束位置。

如果数组中不存在目标值 `target`，返回 `[-1, -1]`。

你必须设计并实现时间复杂度为 `O(log n)` 的算法解决此问题。

 

**示例 1：**

```
输入：nums = [5,7,7,8,8,10], target = 8
输出：[3,4]
```

**示例 2：**

```
输入：nums = [5,7,7,8,8,10], target = 6
输出：[-1,-1]
```

**示例 3：**

```
输入：nums = [], target = 0
输出：[-1,-1]
```

 

**提示：**

- `0 <= nums.length <= 105`
- `-109 <= nums[i] <= 109`
- `nums` 是一个非递减数组
- `-109 <= target <= 109`



> **思路1  自己  插值查找  有误**
>
> 部分超出时间限制
>
> 插入排序适合找一个,要考虑mid式中分母为0的情况

> **思路2  二分查找**
>
> 查找两次
>
> 7行  找到才会赋值,否则为-1
>
> ==12行  找到后,会继续寻找,因为可能不是最左边或最右边==
>
> ==不断寻找更新覆盖,直到全部查找完毕==



**思路2  二分查找**

```java
 // 两次二分查找，分开查找第一个和最后一个
  // 时间复杂度 O(log n), 空间复杂度 O(1)
  // [1,2,3,3,3,3,4,5,9]
  public int[] searchRange2(int[] nums, int target) {
    int left = 0;
    int right = nums.length - 1;
    int first = -1;
    int last = -1;
    // 找第一个等于target的位置
    while (left <= right) {
      int middle = (left + right) / 2;
      if (nums[middle] == target) {
        first = middle;
        right = middle - 1; //重点
      } else if (nums[middle] > target) {
        right = middle - 1;
      } else {
        left = middle + 1;
      }
    }

    // 最后一个等于target的位置
    left = 0;
    right = nums.length - 1;
    while (left <= right) {
      int middle = (left + right) / 2;
      if (nums[middle] == target) {
        last = middle;
        left = middle + 1; //重点
      } else if (nums[middle] > target) {
        right = middle - 1;
      } else {
        left = middle + 1;
      }
    }

    return new int[]{first, last};
  }
```

**思路1  自己  插值查找  有误**

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int left, right, mid, index1 = -1, index2 = -1;
        if(nums.length == 0 || nums[0] > target || nums[nums.length - 1] < target){
            return new int[] {-1, -1};
        }

        // 寻找左索引
        if(nums[0] == target){
            index1 = 0;
        }else{
            left = 0;
            right = nums.length - 1;
            mid = left + (right - left)*(target- nums[left])/(nums[right] -nums[0]);
            while(left < right){
                if(nums[mid] == target && nums[mid - 1] != target){
                    index1 = mid;
                    break;
                }
                if(nums[mid] < target){    
                    left = mid + 1;
                }else{     
                    right = mid - 1;
                }
                if(nums[right] -nums[left] != 0){
                    mid = left + (right - left)*(target- nums[left])/(nums[right] -nums[left]);
                }else{
                    mid = (left + right)/2;
                }
            }            
        }

        // 寻找右索引
        if(nums[nums.length - 1] == target){
            index2 = nums.length - 1;
        }else{
            left = 0;
            right = nums.length - 1;
            mid = left + (right - left)*(target- nums[left])/(nums[right] -nums[left]);
            while(left < right){
                if(nums[mid] == target && nums[mid + 1] != target){
                    index2 = mid;
                    break;
                }
                if(nums[mid] > target){
                    right = mid - 1;
                }else{
                    left = mid + 1;
                }
                if(nums[right] -nums[left] != 0){
                    mid = left + (right - left)*(target- nums[left])/(nums[right] -nums[left]);
                }else{
                    mid = (left + right)/2;
                }
            }            
        }
        return new int[] {index1, index2};
    }
}
```





### <font  size=5 color="red">153. 寻找旋转排序数组的最小值</font>

已知一个长度为 `n` 的数组，预先按照升序排列，经由 `1` 到 `n` 次 **旋转** 后，得到输入数组。例如，原数组 `nums = [0,1,2,4,5,6,7]` 在变化后可能得到：

- 若旋转 `4` 次，则可以得到 `[4,5,6,7,0,1,2]`
- 若旋转 `7` 次，则可以得到 `[0,1,2,4,5,6,7]`

注意，数组 `[a[0], a[1], a[2], ..., a[n-1]]` **旋转一次** 的结果为数组 `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]` 。

给你一个元素值 **互不相同** 的数组 `nums` ，它原来是一个升序排列的数组，并按上述情形进行了多次旋转。请你找出并返回数组中的 **最小元素** 。

你必须设计一个时间复杂度为 `O(log n)` 的算法解决此问题。

 

**示例 1：**

```
输入：nums = [3,4,5,1,2]
输出：1
解释：原数组为 [1,2,3,4,5] ，旋转 3 次得到输入数组。
```

**示例 2：**

```
输入：nums = [4,5,6,7,0,1,2]
输出：0
解释：原数组为 [0,1,2,4,5,6,7] ，旋转 4 次得到输入数组。
```

**示例 3：**

```
输入：nums = [11,13,15,17]
输出：11
解释：原数组为 [11,13,15,17] ，旋转 4 次得到输入数组。
```

> **思路1   二分查找  递归(自己)   **
>
> 如果left对应值小于等于right对应值,就是升序排序,返回left对应值
>
> 如果left对应值大于right对应值,比mid对应值和left对应值
>
> 若前者大寻找left到min,否则就找min到right
>
> **不足**
>
> 未考虑好只剩一个或两个的情况
>
> **总结**
>
> ==考虑为空,元素数量少特殊情况==

> **思路2 换写法 递归变循环 ** 消耗内存从39.6->38.8

**思路一   二分查找  (自己)  **

```java
public int findMin(int[] nums) {
        return findMin(nums, 0, nums.length -1);
    }

    public int findMin(int[] nums, int left, int right){
        int mid = (left+right)/2;

        if(nums[left] <= nums[right]){ // 考虑只有一个的情况
            return nums[left];
        }else if(nums[mid] >= nums[left]){ // 考虑特殊情况,只有两个数,且后者小,需要加等号,下一行mid需要+1
            return findMin(nums, mid + 1, right); 
        }else{
            return findMin(nums, left, mid);
        }     
    }
```

**思路一  递归变循环**

```java
public int findMin(int[] nums) {
       int right = nums.length - 1;
       int left = 0;
       int mid;
       while(true){
            if(nums[left] <= nums[right]){
                return nums[left];
            }

            mid = (left + right)/2;

            if(nums[mid] >= nums[left]){
                left = mid + 1;
            }else{
                right = mid;
            }  
        }
    }
```









# 双指针

### <font  size=5 color="red">015.  三数之和</font>

给你一个整数数组 `nums` ，判断是否存在三元组 `[nums[i], nums[j], nums[k]]` 满足 `i != j`、`i != k` 且 `j != k` ，同时还满足 `nums[i] + nums[j] + nums[k] == 0` 。请

你返回所有和为 `0` 且不重复的三元组。

**注意：**答案中不可以包含重复的三元组。

 

 

**示例 1：**

```
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
解释：
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0 。
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0 。
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0 。
不同的三元组是 [-1,0,1] 和 [-1,-1,2] 。
注意，输出的顺序和三元组的顺序并不重要。
```

**示例 2：**

```
输入：nums = [0,1,1]
输出：[]
解释：唯一可能的三元组和不为 0 。
```

**示例 3：**

```
输入：nums = [0,0,0]
输出：[[0,0,0]]
解释：唯一可能的三元组和为 0 。
```

 

**提示：**

- `3 <= nums.length <= 3000`
- `-105 <= nums[i] <= 105`



> **思路1  排序双指针  自己  有bug**
>
> bug
>
> 未正确解决重复问题,针对前两个重复的问题,解决方案在8行
>
> 后两个重复问题,解决方案在34行
>
> 自己未能清晰的理解重复的处理方案,而是选择记录上一个重复案例



**思路1  排序双指针**

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> results = new ArrayList<>();
        
        Arrays.sort(nums);
        
        for (int i = 0; i < nums.length; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            
            twoSum(-nums[i], i + 1, nums, results);
        }
        
        return results;
    }
    
    private void twoSum(int target, int startIndex, int[] nums, List<List<Integer>> results) {
        int left = startIndex;
        int right = nums.length - 1;
        
        while (left < right) {            
            int sum = nums[left] + nums[right];
            
            if (sum < target) {
                left++;
            } else if (sum > target) {
                right--;
            } else {
                results.add(List.of(-target, nums[left], nums[right]));
                left++;
                right--;
                
                while (left < right && nums[left] == nums[left - 1]) {
                    left++;
                }
                while (left < right && nums[right] == nums[right + 1]) {
                    right--;
                }
            }
        }
    }
}
```





### <font  size=5 color="red">283. 移动零</font>

给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。

**请注意** ，必须在不复制数组的情况下原地对数组进行操作。

 

**示例 1:**

```
输入: nums = [0,1,0,3,12]
输出: [1,3,12,0,0]
```

**示例 2:**

```java
输入: nums = [0]
输出: [0]
```



> **思路一(自己)** 双指针
>
> index1指向第一个0所在的索引, index2指向index1后第一个不为0的索引
>
> 不断将index2处的值赋给index1,然后index2置0
>
> 当index1或index2大于等于length就停止
>
> **不足**
>
> 当nums[index1]为0,后面不为0的交换后,nums[index + 1]必为0,不需要再找下一个0的索引

> **思路二 对一的优化**
>
> 不断把index2填index1的位置,index1++,index2向后找非0直到结束
>
> 最后将index1后面的置0
>
> 开始nums[index2]不为零,交换时index1==index2

> **思路2 开头换写法 易理解(自己)** 
>
> 先找到第一个0的索引就是index1,index2从index1后面开始
>
> 该写法实际上也是对思路1的优化
>
> 交换时nums[index1]为0,直接覆盖,index2++即可,不需要nums[2]置0,只需要最后index1后面全部置0

>**总结**
>
>==需要同时判断索引是否超过限制时,索引判断放在前面==
>
>见思路2 开头换写法第7行 index1 < length && nums[index1] != 0
>
>==减少多余操作,例如思路1优化为思路2开头换写法==



**思路一** **双指针**

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int index1 = 0; // 表示第一个0所在的索引
        int index2 = 0; // 表示index后第一个不为0的索引
        int length = nums.length;
       
       
        for(index1 = 0; index1 < length; index1++){
            if(nums[index1] == 0){
            // 找到index后第一个不为0的索引
            if(index2 <= index1){
                index2 = index1 + 1;
                if(index2 == length){
                        return;
                }
            }
             while(nums[index2] == 0){
                index2++;
                if(index2 == length){
                    return;
                }
            }
    
            // 交换两个值
            nums[index1] = nums[index2];
            nums[index2] = 0;
            }
        }

    }
}
```

**思路二 优化思路一**

```java'
class Solution {
	public void moveZeroes(int[] nums) {
		if(nums==null) {
			return;
		}
		
		int index1 = 0;
		for(int index2=0;index2<nums.length;index2++) {
			//当前元素!=0，就把其交换到左边，等于0的交换到右边
			if(nums[index2]!=0) {
				int tmp = nums[index2];
				nums[index2] = nums[index1];
				nums[index1] = tmp;
				index1++;
			}
		}
	}
}
```

**思路2 开头换写法 **

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int index1 = 0; // 表示第一个0所在的索引
        int index2 = 0; // 表示index后第一个不为0的索引
        int length = nums.length;

		while(index1 < length && nums[index1] != 0  ){
            index1++;
        }
		
		for(index2=index1 + 1;index2<nums.length;index2++) {
			//当前元素!=0，就把其交换到左边，等于0的交换到右边
			if(nums[index2]!=0) {
				int tmp = nums[index2];
				nums[index2] = nums[index1];
				nums[index1] = tmp;
				index1++;
			}
		}
    }
}
```





### <font  size=5 color="red">011. 盛水最多的容器</font>

给定一个长度为 `n` 的整数数组 `height` 。有 `n` 条垂线，第 `i` 条线的两个端点是 `(i, 0)` 和 `(i, height[i])` 。

找出其中的两条线，使得它们与 `x` 轴共同构成的容器可以容纳最多的水。

返回容器可以储存的最大水量。

**说明：**你不能倾斜容器。

```
输入：[1,8,6,2,5,4,8,3,7]
输出：49 
解释：图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。
```

**示例 2：**

```
输入：height = [1,1]
输出：1
```





> **思路一(自己) 动态规划**
>
> // 数组有两个端口,可以从两个端口出发进行遍历缩小班委
>
>  // 暂时的最大面积为(right - left)*Math.min(right, left) 
>
>  // 下一次小right和left中小的移动,因为小的上限不可能高过暂时的最大面积
>
>  // 小的移动后,如果移动后的值还比原来小,就继续移动该点
>
>   // 若right和left相等,移动right和left不影响最后结果
>
>   // 最开始while判断,当left == right,结束返回maxSize

> **思路一(自己) 不做改进**
>
> 用时4ms(上一个5ms)
>
> 不记录当前指针对应的值,一个指针移动后不进行判断,直接计算比较面积
>
> **不改进用时短原因**
>
> 第7行while循环中内容较少,执行时间短于判断时间,所以改进不必要
>
> 如果循环内容很多,则改进是必要的

> ==通过动态规划和贪心选择,不断减少需要判断的范围,选择可能会有最优结果的范围==(如本题指针变化而不是全部遍历,这样减小了遍历范围),==不可能有最优结果的范围直接舍弃==
>
> ==思路一的改进是更深一步的动态规划和贪心选择==

**思路一(自己)**

```java
public int maxArea(int[] height) {
        // 数组有两个端口,可以从两个端口出发进行遍历缩小班委
        // 暂时的最大面积为(right - left)*Math.min(right, left) 
        // 下一次小right和left中小的移动,因为小的上限不可能高过暂时的最大面积
        // 小的移动后,如果移动后的值还比原来小,就继续移动该点
        // 若right和left相等,移动right和left不影响最后结果
        // 最开始while判断,当left == right,结束返回maxSize
        int left = 0; // 左指针
        int right = height.length - 1; // 右指针
        int maxSize = 0;
        int leftTemp = height[left]; // 暂时记录当前指针的值,移动时
        int rightTemp = height[right];

        maxSize = Math.max(maxSize, (right - left) * Math.min(height[left], height[right]));

        if(height[left]< height[right]){
                left++;
            }else{
                right--;
            }


        while(right != left){
            if(height[left] < leftTemp && height[right] == rightTemp){
                left++;
                continue;
            }else if(height[left] == leftTemp && height[right] < rightTemp){
                right--;
                continue;
            }else if(height[left] == leftTemp && height[right] == rightTemp){
                right--;
                continue;
            }

            maxSize = Math.max(maxSize, (right - left) * Math.min(height[left], height[right]));

            if(height[left]< height[right]){
                leftTemp = height[left];
                left++;
            }else{
                rightTemp = height[right];
                right--;
            }
        }

        return maxSize;
    }
```

**思路一(自己) 不做改进**

```java
public int maxArea(int[] height) {
        
        int left = 0; // 左指针
        int right = height.length - 1; // 右指针
        int maxSize = 0;
        
        while(right != left){ 
            maxSize = Math.max(maxSize, (right - left) * Math.min(height[left], height[right]));
            
            if(height[left]< height[right]){     
                left++;
            }else{    
                right--;
            }
        }

        return maxSize;
    }
```



# 滑动窗口

给定一个字符串 `s` ，请你找出其中不含有重复字符的 **最长子串** 的长度。

 

**示例 1:**

```
输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

**示例 2:**

```
输入: s = "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

**示例 3:**

```
输入: s = "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

 

**提示：**

- `0 <= s.length <= 5 * 104`
- `s` 由英文字母、数字、符号和空格组成



> **思路1  自己  思路有误**
>
> Hashset不断加入字符,遇到相同字符统计长度,清空Hashset

> **思路2  纠正思路**
>
> 加入左指针,左指针一次滑动一格,删除滑动的位置



**思路2  纠正思路**

```java
    public int lengthOfLongestSubstring(String s) {
        char[] chars = s.toCharArray();
        int length = chars.length;
        HashSet<Character> set = new HashSet<Character>();
        int ans = 1, left = 0, right = 1; //right 下一个要加入的索引
        if(length == 0){
            return 0;
        }
        set.add(chars[0]);

        while(right < length){
            if(set.contains(chars[right])){
                set.remove(chars[left]);
                left++;
            }else{
                set.add(chars[right]);
                right++;  
                ans = Math.max(ans, right - left);              
            }
        }

        return ans;
    }
```

**思路1  自己  思路有误**

```java
    public int lengthOfLongestSubstring(String s) {
        char[] chars = s.toCharArray();
        int length = chars.length;
        HashSet<Character> set = new HashSet<Character>();
        int ans = 0, max = 0;

        for(int i = 0; i < length; i++){
            if(set.contains(chars[i])){
                ans = Math.max(ans, max);
                set.clear();
                set.add(chars[i]);
                max = 1;
            }else{
                set.add(chars[i]);
                max++;
            }
        }
        ans = Math.max(ans, max);

        return ans;
    }
```





# 动态规划

### <font  size=5 color="red">070.  爬楼梯 </font>

假设你正在爬楼梯。需要 `n` 阶你才能到达楼顶。

每次你可以爬 `1` 或 `2` 个台阶。你有多少种不同的方法可以爬到楼顶呢？

 

**示例 1：**

```
输入：n = 2
输出：2
解释：有两种方法可以爬到楼顶。
1. 1 阶 + 1 阶
2. 2 阶
```

**示例 2：**

```
输入：n = 3
输出：3
解释：有三种方法可以爬到楼顶。
1. 1 阶 + 1 阶 + 1 阶
2. 1 阶 + 2 阶
3. 2 阶 + 1 阶
```

 

**提示：**

- `1 <= n <= 45`



> **思路1  动态规划**
>
> 用数组储存

> **思路1  优化细节**
>
> 用三个值储存
>
> ==每种都收集才需要迭代,只要结果就动态规划==

> **反思**
>
> ==需要记录种过程,需要回溯,只需要记录个数等某个结果,动态即可==

**思路1  动态规划**

```java
    public int climbStairs(int n) {
        if(n == 1){
            return 1;
        }else if(n == 2){
            return 2;
        }
        int[] ans = new int[n + 1];
        ans[1] = 1;
        ans[2] = 2;
        for(int i = 3; i <= n; i++){
            ans[i] = ans[i - 1] + ans[i - 2];
        }

        return ans[n];
    }
```

**思路1  优化细节**

```java
    public int climbStairs(int n) {
        int p = 0, q = 0, r = 1;
        for (int i = 1; i <= n; ++i) {
            p = q; 
            q = r; 
            r = p + q;
        }
        return r;
    }

```



### <font  size=5 color="red">118.  杨辉三角 </font>

给定一个非负整数 *`numRows`，*生成「杨辉三角」的前 *`numRows`* 行。

在「杨辉三角」中，每个数是它左上方和右上方的数的和。

![img](LeetCode刷题.assets/1626927345-DZmfxB-PascalTriangleAnimated2.gif)

 

**示例 1:**

```
输入: numRows = 5
输出: [[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]
```

**示例 2:**

```
输入: numRows = 1
输出: [[1]]
```

 

**提示:**

- `1 <= numRows <= 30`



> **思路1  自己  动态规划**
>
> 利用上一列进行计算

> **思路2  优化思路1**
>
> 考虑到需要上一行的值来对下一行进行计算,思路1需要每次创建新行
>
> 可以直接在一行上进行修改,到下一行直接在头部add(1),再让每一个数(除了最后一个)加上后面一个数
>
> **在头部add(1)的原因**
>
> 由于不能让修改后的数影响后面数的修改,而修改的顺序是从前到后,所以计算每个数时应该要用到后面的未有修改的数
>
> 所以需要row.set(j,row.get(j)+row.get(j+1));



**思路2  优化思路1**

```java
class Solution {
    public List<List<Integer>> generate(int numRows) {
        List<List<Integer>> final_list=new ArrayList<>();
        ArrayList<Integer> row=new ArrayList<>();
        
        for(int i=0;i<numRows;i++){
            row.add(0,1); //每多一行 多加一個1 i=3時 裡面就已就有四個1了
            for(int j=1;j<i;j++){
                row.set(j,row.get(j)+row.get(j+1));
            }
            final_list.add(new ArrayList<>(row)); //每一次都要new一個list
                                                   //不然全部都會指向最後一個list
                                   //[[1,3,3,1],[1,3,3,1],[1,3,3,1],[1,3,3,1]]
        }
        return final_list;
    }
}
```

**思路1  自己  动态规划**

```java
class Solution {
    public List<List<Integer>> generate(int numRows) {
        List<List<Integer>> ans = new ArrayList<>(numRows);
        List<Integer> arr1 = new ArrayList<Integer>(1);
        arr1.add(1);
        ans.add(arr1);

        int num = 2; // 记录当前的行数
        while(num <= numRows){
            List<Integer> arr = new ArrayList<Integer>(num);
            List<Integer> preArr = ans.get(ans.size() - 1);
            arr.add(1);
            for(int i = 1; i <= num - 2; i++){
                arr.add(preArr.get(i - 1) + preArr.get(i));
            }
            arr.add(1);
            ans.add(arr);
            num++;
        }
        return ans;
    }
}
```













### <font  size=5 color="red">152.  乘积最大子序列 </font>

**231026  第十题**

给你一个整数数组 `nums` ，请你找出数组中乘积最大的非空连续子数组（该子数组中至少包含一个数字），并返回该子数组所对应的乘积。

测试用例的答案是一个 **32-位** 整数。

**子数组** 是数组的连续子序列。

 

**示例 1:**

```
输入: nums = [2,3,-2,4]
输出: 6
解释: 子数组 [2,3] 有最大乘积 6。
```

**示例 2:**

```
输入: nums = [-2,0,-1]
输出: 0
解释: 结果不能为 2, 因为 [-2,-1] 不是子数组。
```

> **思路1  自己  极致分类讨论**
>
> 1ms 击败 95.42%  41.31MB
>
> 根据0划分,分别求每段最大值(小于0直接舍弃),和0比较
>
> 每段根据负数个数奇偶性和是否为1进一步计算

> **思路2  动态规划  滚动法**
>
> 1ms 40.97mb
>
> 求一段子序列的积的最大值,可以利用滚动来解,左右对称,左右开始都可
>
> 动态规划思想,考虑不断加入数字,判断加入后现有的最大
>
> 考虑负数的存在,需要同时保存最小值,最小乘以负数就变成最大
>
> 考虑0的存在,最大从当前值和0中取,最小也是,相当于遇0就刷新状态,除非只有一个负数,只有非零连续子序列有正数最大值
>
> 如果分类讨论下一个数字的正负,过程就会复杂,类似思路1

> **总结**
>
> ==结果为序列中一段,可以滚动来求,结合动态规划,不断考虑新元素的加入,更新需要的结果==

**思路1  自己  极致分类考虑**

```java
    public int maxProduct(int[] nums) {
        ArrayList<Integer> zeroIndex = new ArrayList<Integer>();
        int ans = 0, zeroNum = 0;

        for(int i = 0; i < nums.length; i++){
            if(nums[i] == 0){
                zeroIndex.add(i);
                zeroNum++;
            }
        }

        if(zeroNum == 0){
            return maxProductWithoutZero(nums, 0, nums.length - 1);
        }

        int left = -1;
        
        for(int index: zeroIndex){
            ans = Math.max(ans, maxProductWithoutZero(nums, left + 1, index - 1));
            left = index;     
        }
        ans = Math.max(ans, maxProductWithoutZero(nums, left + 1, nums.length - 1));


        return ans;
    }

    // 求无0子序列最大乘积
    public int maxProductWithoutZero(int[] nums, int left, int right){
        if(left > right) return -65535;

        int fuNum = 0, fuStart = -1, fuEnd = -1;
        // 统计负数个数,第一个负数索引,最后一个负数索引
        for(int i = left; i <= right; i++){
            if(nums[i] < 0){
                fuNum++;
                if(fuStart == -1){
                    fuStart = i;
                }
                fuEnd = i;
            }
        }

        if(fuNum % 2 == 0){
            return ji(nums, left, right);
        }else if(fuStart == fuEnd){
            return Math.max(nums[fuStart], Math.max(ji(nums, left, fuStart - 1), ji(nums, fuStart + 1, right)));
        }else{
            return Math.max(ji(nums, fuStart + 1, right),ji(nums, left, fuEnd - 1));
        }        
    }

    // 求子数组的积
    public int ji(int[] nums, int left, int right){
        if(left > right) return -65535;

        int ans = 1;

        for(int i = left; i <= right; i++){
            ans*=nums[i];
        }

        return ans;
    }
```

**思路2  动态规划  滚动法**

```java
    public int maxProduct(int[] nums) {
        int max = 1, min = 1, ans = Integer.MIN_VALUE, temp;

        for(int i = 0; i < nums.length; i++){
            if(nums[i] < 0){
                temp = max;
                max = min;
                min = temp;
            }

            max = Math.max(max*nums[i], nums[i]);
            min = Math.min(min*nums[i], nums[i]);

            ans = Math.max(max, ans);
        }
        return ans;
    }
```



### <font  size=5 color="red">279.  完全平方数(动态规划域选择不好) </font>

给你一个整数 `n` ，返回 *和为 `n` 的完全平方数的最少数量* 。

**完全平方数** 是一个整数，其值等于另一个整数的平方；换句话说，其值等于一个整数自乘的积。例如，`1`、`4`、`9` 和 `16` 都是完全平方数，而 `3` 和 `11` 不是。

 

**示例 1：**

```
输入：n = 12
输出：3 
解释：12 = 4 + 4 + 4
```

**示例 2：**

```
输入：n = 13
输出：2
解释：13 = 4 + 9
```

 

**提示：**

- `1 <= n <= 10^4`



> **思路1  自己  动态规划**
>
> 求n的完全平方数的最小个数,就是求 n - j*j 的完全平方数个数 + 1 , 适合动态规划
>
> ==由于需要前面部分数字的完全平方数,所以从1开始求完全平方数,最终求到n的完全平方数==
>
> 由于求 n - j*j 的完全平方数也不知道,所以本题应该从前向后,从小向大规划
>
> 两个方向
>
> - 自己  求出  完全平方数个数  == 1 的数, 然后根据这个求出 完全平方数个数 == 2 的十分麻烦
> - 答案  求出数字1的完全平方数个数,然后求2的...
>
> ==本题的可能的dp对象, 数字n, 完全平方数个数, 这两个是本题中的元素,都应该进行考虑,自己只想到了后者==
>
> ==dp对象角度(顺序方向和对象元素)都需要进行考虑==



**思路1  自己  动态规划**

```java
class Solution {
    public int numSquares(int n) {
        int[] dp = new int[n + 1]; // dp[i] 数字i的完全平方数的个数,可能值为dp[i - j*j] + 1
        
        for(int i = 1; i <= n; i++){
            int min = i; // dp[i]
            for(int j = 1; j * j <= i; j++){
                min = Math.min(min, dp[i - j * j]);
            }
            dp[i] = min + 1;
        }

        return dp[n];
    }
}
```





### <font  size=5 color="red">300. 最长递增子序列(未解出) </font>

给你一个整数数组 `nums` ，找到其中最长严格递增子序列的长度。

**子序列** 是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如，`[3,6,2,7]` 是数组 `[0,3,1,6,2,2,7]` 的子序列。

 

**示例 1：**

```
输入：nums = [10,9,2,5,3,7,101,18]
输出：4
解释：最长递增子序列是 [2,3,7,101]，因此长度为 4 。
```

**示例 2：**

```
输入：nums = [0,1,0,3,2,3]
输出：4
```

**示例 3：**

```
输入：nums = [7,7,7,7,7,7,7]
输出：1
```

 

**提示：**

- `1 <= nums.length <= 2500`
- `-104 <= nums[i] <= 104`



> **思路1  动态规划**
>
> 分别找到以每个数组结尾的子串的最长值,进行第二次遍历,结果可能是之前的每一个最长值,如果大于之前的就可能是之前的最长值+1

> **思路2  贪心+二分查找**
>
> 用一个数组储存当前最递增长子序列,将一个数插入,位置为它大于的最大的一个数的后面,如果插入在最后一个数后面,长度++;
>
> ==二分查找细节满满==
>
> 查找最终条件为left < right,所以最终会只得到1个索引
>
> 大于mid加1还是小于mid减一还是二者都有很有讲究
>
> 只有大于mid,left = mid +1,则最终找到的索引就是刚好大于的下一个(下一个是因为mid + 1),只有两个的情况下,由于mid会倾向于左边,而left = mid+ 1会加一,所以不会陷入死循环



**思路1  动态规划**

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int[] dp = new int[nums.length];
        dp[0] = 1;
        int maxans = 1;
        for (int i = 1; i < nums.length; i++) {
            dp[i] = 1;
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
            maxans = Math.max(maxans, dp[i]);
        }
        return maxans;
    }
}
```

**思路2  贪心+二分查找**

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int[] tails = new int[nums.length];
        int end = 0;

        for(int num: nums){
            int i = 0, j = end;
            while(i < j){
                int m = (i + j)/2;
                if(num > tails[m]) i = m + 1;
                else j = m;
            }
            tails[i] = num;
            if(j == end) end++;
        }

        return end;
    }
}
```



### <font  size=5 color="red">416.  分隔等和子集(超时) </font>

给你一个 **只包含正整数** 的 **非空** 数组 `nums` 。请你判断是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

 

**示例 1：**

```
输入：nums = [1,5,11,5]
输出：true
解释：数组可以分割成 [1, 5, 5] 和 [11] 。
```

**示例 2：**

```
输入：nums = [1,2,3,5]
输出：false
解释：数组不能分割成两个元素和相等的子集。
```

 

**提示：**

- `1 <= nums.length <= 200`
- `1 <= nums[i] <= 100`

> **思路1  自己  超时  暴力递归**

> **思路2  动态规划**
>
> 规划空间   `new boolean[nums.length][sum + 1]`;每一行不断加入数字,每一列存在的子集和
>
> **思路2  动态规划 大优化**
>
> 规划空间  可能会出现的子集和
>
> **细节**
>
> - 开始不断剪枝,判断个数是否大于2,判断总和是否为偶数,判断最大值是否超过一半
> - 20行,只能反向遍历,由于后面的值要考虑前面的影响,而每一次是对前面的覆盖,避免影响需要从后面进行遍历

> **反思**
>
> ==规划空间的选择不要太拘泥于原始数据,也可是是答案相关等==

**思路2  动态规划  大优化**

```java
class Solution {
    public boolean canPartition(int[] nums) {
        if (nums == null || nums.length == 0) {
            return false;
        }

        int sum = 0;
        for (int num:nums) {
            sum += num;
        }

        if (sum % 2 == 1) {
            return false;
        }

        int target = sum / 2;
        int[] dp = new int[target + 1];

        for (int i = 0; i < nums.length; i++) {
            for (int j = sum / 2; j >= nums[i]; j--) {
                dp[j] = Math.max(dp[j], dp[j - nums[i]] + nums[i]);
            }

            if (dp[target] == target) {
                return true;
            }
        }

        return dp[target] == target;
    }
}
```

**思路2  动态规划**

```java
class Solution {
    public boolean canPartition(int[] nums) {
        int sum = 0;
        for(int num: nums) sum += num;

        if(sum % 2 == 1) return false;
        else sum = sum / 2;

        boolean[][] dp = new boolean[nums.length][sum + 1]; // 每一行不断加入数字,每一列存在的子集和
        for (int i = 0; i < nums.length; i++) {
            dp[i][0] = true;
        }
        if(nums[0] > sum){
            return false;
        }else{
            dp[0][nums[0]] = true;
        }

        for(int i = 1;i < nums.length; i++){
            int num = nums[i];
            for(int j = 1; j <= sum; j++){
                if(j >= num){
                    dp[i][j] = dp[i - 1][j] || dp[i - 1][j - num];
                }else{
                    dp[i][j] = dp[i - 1][j];
                }
            }
            if(dp[i][sum]) return true;
        }
        return false;
        
    }
}
```



**思路1  自己  超时  暴力递归**

```java
class Solution {
    boolean flag = false;
    public boolean canPartition(int[] nums) {
        int sum = 0;
        for(int num: nums) sum += num;

        if(sum % 2 == 1) return false;
        else sum = sum / 2;
        canPartition(nums, nums.length - 1, sum);

        return flag;            
    }
    // i表示可能要减去的的数字的索引,sum表示剩余需要找到的数字的和
    private void canPartition(int[] nums, int i, int sum){
        if(i == -1 || flag == true) return;
        if(sum == nums[i]){
            flag = true;
            return;
        }
        if(sum > nums[i]) canPartition(nums, i - 1, sum - nums[i]);
        canPartition(nums, i - 1, sum);        
    }
}
```







# 多维动态规划

### <font  size=5 color="red">1143. 最长公共子序列(未解出) </font>

给定两个字符串 `text1` 和 `text2`，返回这两个字符串的最长 **公共子序列** 的长度。如果不存在 **公共子序列** ，返回 `0` 。

一个字符串的 **子序列** 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。

- 例如，`"ace"` 是 `"abcde"` 的子序列，但 `"aec"` 不是 `"abcde"` 的子序列。

两个字符串的 **公共子序列** 是这两个字符串所共同拥有的子序列。

​                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          

**示例 1：**

```
输入：text1 = "abcde", text2 = "ace" 
输出：3  
解释：最长公共子序列是 "ace" ，它的长度为 3 。
```

**示例 2：**

```
输入：text1 = "abc", text2 = "abc"
输出：3
解释：最长公共子序列是 "abc" ，它的长度为 3 。
```

**示例 3：**

```
输入：text1 = "abc", text2 = "def"
输出：0
解释：两个字符串没有公共子序列，返回 0 。
```

 

**提示：**

- `1 <= text1.length, text2.length <= 1000`
- `text1` 和 `text2` 仅由小写英文字符组成。



> **思路1  贪心  自己**
>
>   时间O(mn),mn分别为两数组长度
>
> ​    // 用一个表储存text2中每个元素和text1中字符是否相等,长宽分别为两字符串长度
>
> ​    // 先找长度为1的,最优只可能有两个,左边最靠上的,右边最靠左的,同行或同列可能最优点只有一个
>
> ​    // 再找下一个可能的,下一个只能是上一组中每个点对应的两个可能最优点
>
> ​    // 结束条件,当找到最后一行或最后一列就得到一个可能的最大值

> **思路2  2维动态规划**
>
> 太简单了,抛弃思路1吧



**思路1  贪心  自己**

```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        // 用一个表储存text2中每个元素和text1中字符是否相等,长宽分别为两字符串长度
        // 先找长度为1的,最优只可能有两个,左边最靠上的,右边最靠左的,同行或同列可能最优点只有一个
        // 再找下一个可能的,下一个只能是上一组中每个点对应的两个可能最优点
        // 结束条件,当找到最后一行或最后一列就得到一个可能的最大值
        char[] t1 = text1.toCharArray();
        char[] t2 = text2.toCharArray();
        int ans = 0; // 记录公共子序列最大长度
        int length = Math.max(t1.length, t2.length);
        int[] index1 = new int[length]; // 分别记录某个长度可能最优点在两个数组中的索引
        int[] index2 = new int[length];
        index1[0] = -1;  // 设置初始点,由于要从下一行或列开始找,初始就设为-1
        index2[0] = -1;

        while(true){
            ans++;
            int[] tempindex1 = new int[length]; // 分别暂时记录新可能最优点在两个数组中的索引,最后赋给稳定的
            int[] tempindex2 = new int[length];

            // 遍历每一个点,i1为点在index1中索引,i2为点在index2中索引
            for(int i = 0, i1 = index1[i], i2 = index2[i]; i1 == 0 && i2 == 0; i++){
                // 找到该点的下两个最优点,校验后存入暂时索引数组
                for(int j = 0)
            } 
        }

    }
}
```

**思路2  2维动态规划**

```java
    public int longestCommonSubsequence(String text1, String text2) {
        char[] t1 = text1.toCharArray();
        char[] t2 = text2.toCharArray();
        int[][] max = new int[t1.length + 1][t2.length + 1];


        for(int i = 0; i < t1.length; i++){
            for(int j = 0; j < t2.length; j++){
                if(t1[i] == t2[j]){
                    max[i + 1][j + 1] = max[i][j] + 1;
                }else{
                    max[i + 1][j + 1] = Math.max(max[i + 1][j], max[i][j + 1]);
                }
            }
        }

        return max[t1.length][t2.length];
    }
```







# 回溯



### <font  size=5 color="red">017.  电话号码的字母组合 </font>

给定一个仅包含数字 `2-9` 的字符串，返回所有它能表示的字母组合。答案可以按 **任意顺序** 返回。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

![img](LeetCode刷题.assets/200px-telephone-keypad2svg.png)

 

**示例 1：**

```
输入：digits = "23"
输出：["ad","ae","af","bd","be","bf","cd","ce","cf"]
```

**示例 2：**

```
输入：digits = ""
输出：[]
```

**示例 3：**

```
输入：digits = "2"
输出：["a","b","c"]
```

 

**提示：**

- `0 <= digits.length <= 4`
- `digits[i]` 是范围 `['2', '9']` 的一个数字。



> **思路1  自己  递归**

> **思路1  优化**
>
> `char[][]` 可以改为`Hashmap`



**思路1  自己  递归**

```java
class Solution {
    int len;
    List<String> ans = new ArrayList<String>();
    char[] ansCharList; // 存放目标字符串对应的字符集合

    public List<String> letterCombinations(String digits) {
        char[] charList = digits.toCharArray();
        len = charList.length;
        if(len == 0) return ans;
        ansCharList = new char[len];
        // 第i个数子对应的字符数组索引为 i-2
        char[][] map = {{'a','b','c'},{'d','e','f'},{'g','h','i'},
        {'j','k','l'},{'m','n','o'},{'p','q','r','s'},{'t','u','v'},{'w','x','y','z'}};
        char[][] map1 = new char[len][]; // 存放每个数组对应的字母数组
        
        for(int i = 0; i < len; i++){
            map1[i] = map[charList[i] - '0' - 2];
        }

        letterCombinations(map1, len);

        return ans;
    }

    // len 表示char[] ansCharList中的空位
    private void letterCombinations(char[][] map1, int length){
        int len1 = map1[len - length].length;
        if(length == 1){
            for(int i = 0; i < len1; i++){
                ansCharList[len - length] = map1[len - length][i];
                ans.add(new String(ansCharList));
            }
        }else{
            for(int i = 0; i < len1; i++){
                ansCharList[len - length] = map1[len - length][i];
                letterCombinations(map1, length - 1);
            }
        }        
    }
}
```





### <font  size=5 color="red">022. 括号生成 </font>

数字 `n` 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 **有效的** 括号组合。

 

**示例 1：**

```
输入：n = 3
输出：["((()))","(()())","(())()","()(())","()()()"]
```

**示例 2：**

```
输入：n = 1
输出：["()"] 
```

**提示：**

- `1 <= n <= 8`



> **思路1  回溯**
>
> **错误**
>
> ==StringBuilder克隆时直接赋值是浅clone==,会导致两者指向同一个sb,联系下==链表的遍历,temp就是浅clone,暂时储存浅clone就满足需求==
>
> ==编译错误,返回值需要提前初始化,在循环或if中初始化编译器看不出来==
>
> **不足**
>
> 回溯时不断返回值并进行拼接返回
>
> **改进**
>
> 可以直接定义集合等用来储存结果,在回溯时直接传递该集合,见**思路1  改进**

> **思路1  改进**
>
> ==注意15, 20行 浅clone问题避免,直接对sb进行还原==

> **思路2**
>
> n个括号也在在n-1个括号上插入两个括号,会有重复,需要用HashSet去重



**思路1  回溯  错误**

```java
        StringBuilder sb2 = new StringBuilder(); // 浅clone,没有意义
        sb2 = sb;

        StringBuilder sb2 = new StringBuilder(sb.toString());  // 正确写法
```

**思路1  回溯**

```java
    public List<String> generateParenthesis(int n) {
        StringBuilder sb = new StringBuilder();
        List<String> list= new ArrayList<String>();
        return method(n, n, sb);
    }

    public List<String> method(int l, int r, StringBuilder sb){
        if(l == 0 && r == 1){
            sb.append(")");
            List<String> list3 = new ArrayList<String>();
            list3.add(sb.toString());
            return list3;
        }

        StringBuilder sb2 = new StringBuilder(sb.toString());
        List<String> list1 = new ArrayList<String>();
        List<String> list2 = new ArrayList<String>();

        if(l > 0){
            sb.append("(");
            list1 = method(l - 1, r, sb);
        }
        if(r > l){
            sb2.append(")");
            list2 = method(l, r - 1, sb2);          
        }

        list1.addAll(list2);
        return list1;
    }
```



**思路1  改进**

```java
    public List<String> generateParenthesis(int n) {
        List<String> ans = new ArrayList<String>();
        backtrack(ans, new StringBuilder(), 0, 0, n);
        return ans;
    }

    public void backtrack(List<String> ans, StringBuilder cur, int open, int close, int max) {
        if (cur.length() == max * 2) {
            ans.add(cur.toString());
            return;
        }
        if (open < max) {
            cur.append('(');
            backtrack(ans, cur, open + 1, close, max);
            cur.deleteCharAt(cur.length() - 1);
        }
        if (close < open) {
            cur.append(')');
            backtrack(ans, cur, open, close + 1, max);
            cur.deleteCharAt(cur.length() - 1);
        }
    }
```



### <font  size=5 color="red">051.  N皇后 </font>

按照国际象棋的规则，皇后可以攻击与之处在同一行或同一列或同一斜线上的棋子。

**n 皇后问题** 研究的是如何将 `n` 个皇后放置在 `n×n` 的棋盘上，并且使皇后彼此之间不能相互攻击。

给你一个整数 `n` ，返回所有不同的 **n 皇后问题** 的解决方案。

每一种解法包含一个不同的 **n 皇后问题** 的棋子放置方案，该方案中 `'Q'` 和 `'.'` 分别代表了皇后和空位。

 

**示例 1：**

![img](LeetCode刷题.assets/queens.jpg)

```
输入：n = 4
输出：[[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
解释：如上图所示，4 皇后问题存在两个不同的解法。
```

**示例 2：**

```
输入：n = 1
输出：[["Q"]]
```

 

**提示：**

- `1 <= n <= 9`



> **思路1  自己  回溯**
>
> 对要回溯的内容直接进行覆盖处理,进入下一步前都会覆盖之前的选择,就不需要回退



**思路1  自己  回溯**

```java
class Solution {
    public List<List<String>> solveNQueens(int n) {
        List<List<String>> ansList = new ArrayList<List<String>>(); // 存放所有答案
        int[] ans = new int[n]; // 存放每种可能存在的排列,第i个数字表示第i行的列索引
        recure(ansList, ans, 0, n);
        return ansList;
    }

    // ansList 结果集合,在step== n - 1时,判断第n个是否能够放入
    // ans  存放每种可能存在的排列,第i个数字表示第i行的列索引
    // step 将要放的棋子所在的行索引
    // n 表示棋子的个数
    private void recure(List<List<String>> ansList, int[] ans, int step, int n){
        // 将要放最后一个棋子时,判断结统计可能的结果
        if(step == n -1){
            for(int i = 0; i < n; i++){
                if(isOK(ans, step, i)){
                    ans[step] = i;
                    List<String> ansStrList = new ArrayList<String>(n);
                    char[] ansCharArr = new char[n];
                    Arrays.fill(ansCharArr, '.');
                    for(int j = 0; j < n; j++){
                        ansCharArr[ans[j]] = 'Q';
                        ansStrList.add(new String(ansCharArr));
                        ansCharArr[ans[j]] = '.';
                    }
                    ansList.add(ansStrList);
                }
            }
        }

        // 遍历放入索引为step行的棋子
        for(int i = 0; i < n; i++){
            if(isOK(ans, step, i)){
                ans[step] = i;
                recure(ansList, ans, step + 1, n);                
            }
        }
    }

    // 判断第step行的棋子是否能放在第choice列上
    private boolean isOK(int[] ans, int step, int choice){
        for(int i = 0; i < step; i++){
            if(ans[i] == choice || i + ans[i] == step + choice || i - ans[i] == step - choice) return false;
        }
        return true;
    }
}
```









### <font  size=5 color="red">078.  子集 </font>

给你一个整数数组 `nums` ，数组中的元素 **互不相同** 。返回该数组所有可能的子集（幂集）。

解集 **不能** 包含重复的子集。你可以按 **任意顺序** 返回解集。

 

**示例 1：**

```
输入：nums = [1,2,3]
输出：[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```

**示例 2：**

```
输入：nums = [0]
输出：[[],[0]]
```

 

**提示：**

- `1 <= nums.length <= 10`
- `-10 <= nums[i] <= 10`
- `nums` 中的所有元素 **互不相同**



> **思路1 回溯**
>
> 由于每个元素都有两种可能存在或者不存在,
>
> 所以递归时有两种可能,
>
> 18行装入该元素
>
> 21行取出该元素(元素最终要取出)
>
> 两种情况分别进入递归
>
> 当 n = nums.length(所有应该装入的都装进去了,开始统计)
>
> 也可以用n = nums.length - 1 当结束条件, 按是否包含最后一个元素分别装入

> 思路
>
> 用0 和1 表示每个元素的存在与否,那么n个元素的不同情况可以表示为 n位二进制数的数量
>
> 同样,如果每个元素有m种状态,就是n位m进制数的数量
>
> 每个元素都有两种可能存在或者不存在
>
> ==一个集合有多个元素,每个元素的状态相互独立,遍历时分别考虑每种元素的一种状态然后向下遍历==



**思路1 回溯**

```java
class Solution {
    List<List<Integer>> list;
    List<Integer> ans;
    
    public List<List<Integer>> subsets(int[] nums) {
        ans = new ArrayList<Integer>();
        list = new ArrayList<List<Integer>>();
        subsets(0, nums);
        return list;
    }

    private void subsets(int n, int[] nums){
        if(n >= nums.length){
            list.add(new ArrayList<Integer>(ans));
            return;
        }

        ans.add(nums[n]);
        subsets(n + 1, nums);
        ans.remove(ans.size() - 1);
        subsets(n + 1, nums);
    }
}
```





### <font  size=5 color="red">131.  分隔回文串 </font>

给你一个字符串 `s`，请你将 `s` 分割成一些子串，使每个子串都是 **回文串** 。返回 `s` 所有可能的分割方案。

**回文串** 是正着读和反着读都一样的字符串。

 

**示例 1：**

```
输入：s = "aab"
输出：[["a","a","b"],["aa","b"]]
```

**示例 2：**

```
输入：s = "a"
输出：[["a"]]
```

 

**提示：**

- `1 <= s.length <= 16`
- `s` 仅由小写英文字母组成



> **思路1  动态规划 + 回溯  未想到动态规划**
>
> 动态规划求以任意开头和结尾是否为回文串
>
> 回溯获取最终值
>
> **大bug**
>
> ==浅克隆问题==,处理见行32
>
> 反思
>
> ==动态规划的场景,新加的值要用到之前一些值的结果,想不到方案时可以看是否能够转为为动态规划问题==



**思路1  动态规划 + 回溯  未想到动态规划**

```java
class Solution {
    boolean[][] f;
    List<List<String>> ret;
    List<String> ans;
    int length;

    public List<List<String>> partition(String s) {
        char[] str = s.toCharArray();
        length = str.length;
        f = new boolean[length][length]; // 以行索引为开头,列索引为结尾的字符串是否为回文串
        ret = new ArrayList<List<String>>();
        ans = new ArrayList<String>();

        // 获取f的值
        for(int i = 0; i < length; i++){
            Arrays.fill(f[i], true);
        }
        for(int i = length - 1; i >= 0; i--){
            for(int j = i + 1; j < length; j++){
                f[i][j] = f[i + 1][j - 1] && str[i] == str[j];
            }
        }

        // 输出结果
        partition(0, s);
        return ret;
    }

    // n为开头的索引
    private void partition(int n, String s){
        if(n >= length){
            ret.add(new ArrayList<String>(ans));
            return;
        }

        for(int i = n; i < length; i++){
            if(f[n][i]){
                ans.add(s.substring(n,i + 1));
                partition(i + 1, s);
                ans.remove(ans.size() - 1);
            }
        }
    }
}
```



# 贪心算法

### <font  size=5 color="red">045. 跳跃游戏2</font>

给定一个长度为 `n` 的 **0 索引**整数数组 `nums`。初始位置为 `nums[0]`。

每个元素 `nums[i]` 表示从索引 `i` 向前跳转的最大长度。换句话说，如果你在 `nums[i]` 处，你可以跳转到任意 `nums[i + j]` 处:

- `0 <= j <= nums[i]` 
- `i + j < n`

返回到达 `nums[n - 1]` 的最小跳跃次数。生成的测试用例可以到达 `nums[n - 1]`。

 

**示例 1:**

```
输入: nums = [2,3,1,1,4]
输出: 2
解释: 跳到最后一个位置的最小跳跃数是 2。
     从下标为 0 跳到下标为 1 的位置，跳 1 步，然后跳 3 步到达数组的最后一个位置。
```

**示例 2:**

```
输入: nums = [2,3,0,1,4]
输出: 2
```

**提示:**

- `1 <= nums.length <= 104`
- `0 <= nums[i] <= 1000`
- 题目保证可以到达 `nums[n-1]`



> **思路1  自己  有细节的暴力求解  **
>
> 空间小 时间8ms
>
> 找到每一步能到达的位置并记录

> **思路2  贪心  正向  优化思路1**
>
> 时间复杂O(n)
>
> 思路1未考虑到:
>
> 若第n步到达某个位置,则该位置前的所有位置在n步内都能到达,所以尽量越远越好
>
> 所以每一次只需要找下一步能到的最远的位置

> **思路3  贪心  反向**
>
> 两层循环,慢,时间复杂O(n2)



**思路2  贪心  正向**

```java
    public int jump(int[] nums) {
        int length = nums.length;
        int end = 0;
        int maxPosition = 0; 
        int steps = 0;
        for (int i = 0; i < length - 1; i++) {
            maxPosition = Math.max(maxPosition, i + nums[i]); 
            if (i == end) {
                end = maxPosition;
                steps++;
            }
        }
        return steps;
    }
```

**思路3  贪心  反向**

```java
    public int jump(int[] nums) {
        int position = nums.length - 1;
        int steps = 0;
        while (position > 0) {
            for (int i = 0; i < position; i++) {
                if (i + nums[i] >= position) {
                    position = i;
                    steps++;
                    break;
                }
            }
        }
        return steps;
    }

```

**思路1  自己  有细节的暴力求解  **

```java
    public int jump(int[] nums) {
        // 用一个长度为nums.length的数组ans记录到达每个格子的最小步数,初始全为-1
        // 当ans[nums.length- 1]不为-1,就结束,返回该数字
        // 第一次将能到达的记录为1,每次至少跳一格.跳0的不记录下一个地址
        // 第二次将能到的记为2,如果当前格子有记录,就不覆盖
        int[] ans = new int[nums.length]; // 记录到达每个格子的最短步数
        Arrays.fill(ans, -1);
        ans[0] = 0;
        int step = 0; // 记录步数
        int temp; // 暂时储存索引
        ArrayList<Integer> list = new ArrayList<Integer>(); // 记录第n步到达的格子的索引
        list.add(0);

        while(ans[nums.length - 1] == -1 ){
            step++;
            ArrayList<Integer> list1 = new ArrayList<Integer>(); // 记录下一步能到达的索引
            temp = list.get(0);

            for(int num: list){
                for(int i = Math.max(num + 1, temp); i <= num + nums[num] && i < nums.length; i++){
                    if(ans[i] == -1){
                        ans[i] = step;
                        list1.add(i);
                    }
                    temp = num + nums[num]; 
                }
            }

            list = list1;
        }

        return ans[nums.length - 1];
    }
```

### <font  size=5 color="red">121.  买股票的最佳时机</font>

给定一个数组 `prices` ，它的第 `i` 个元素 `prices[i]` 表示一支给定股票第 `i` 天的价格。

你只能选择 **某一天** 买入这只股票，并选择在 **未来的某一个不同的日子** 卖出该股票。设计一个算法来计算你所能获取的最大利润。

返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 `0` 。

 

**示例 1：**

```
输入：[7,1,5,3,6,4]
输出：5
解释：在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
```

**示例 2：**

```
输入：prices = [7,6,4,3,1]
输出：0
解释：在这种情况下, 没有交易完成, 所以最大利润为 0。
```

 

**提示：**

- `1 <= prices.length <= 105`
- `0 <= prices[i] <= 104`



> **思路1  一次遍历  自己**
>
> 遇到更小的最小值就刷新最小值

> **思路1  一次遍历  优化结构**
>
> 直接比较
>
> prices[i] - minprice > maxprofit
>
> ==判断的时间比计算长,不如减少判断直接计算==

**思路1  一次遍历  自己**

```java
    public int maxProfit(int[] prices) {
        int ans = 0, min = prices[0], max = min;

        // i不为开头或者结尾的索引
        for(int i = 1; i < prices.length - 1; i++){
            int l = prices[i - 1], r = prices[i + 1], cur = prices[i];
            if(cur > l && cur >= r && cur > max){
                max = cur;
                ans = Math.max(max - min, ans);
            }else if(cur < l && cur <= r && cur < min){
                min = cur;
                max = cur;
            }            
        }

        // 考虑最后一个值
        max = prices[prices.length - 1];
        ans = Math.max(max - min, ans);

        return ans;
    }
```

**思路1  一次遍历  优化结构**

```java
    public int maxProfit(int prices[]) {
        int minprice = Integer.MAX_VALUE;
        int maxprofit = 0;
        for (int i = 0; i < prices.length; i++) {
            if (prices[i] < minprice) {
                minprice = prices[i];
            } else if (prices[i] - minprice > maxprofit) {
                maxprofit = prices[i] - minprice;
            }
        }
        return maxprofit;
    }
```

### <font  size=5 color="red">763.  划分字母区间(未解出)</font>

给你一个字符串 `s` 。我们要把这个字符串划分为尽可能多的片段，同一字母最多出现在一个片段中。

注意，划分结果需要满足：将所有划分结果按顺序连接，得到的字符串仍然是 `s` 。

返回一个表示每个字符串片段的长度的列表。

 

**示例 1：**

```
输入：s = "ababcbacadefegdehijhklij"
输出：[9,7,8]
解释：
划分结果为 "ababcbaca"、"defegde"、"hijhklij" 。
每个字母最多出现在一个片段中。
像 "ababcbacadefegde", "hijhklij" 这样的划分是错误的，因为划分的片段数较少。 
```

**示例 2：**

```
输入：s = "eccbbbbdec"
输出：[10]
```

 

**提示：**

- `1 <= s.length <= 500`
- `s` 仅由小写英文字母组成



> **思路1  贪心  **
>
> 第一次遍历找到每个字符出现的最后索引,
>
> 第二次找每个片段的长度,结尾为该片段中字符的最后索引,片段结尾索引需要一直更新

> **思路2  自己  复杂的动态规划**
>
> 因为此题==之后出现的数据会对之前有影响,所以动态规划不是很合适,会比较麻烦==
>
> // 用map储存数据,key为字符,value为字符在的片段的索引 
>
> // 出现新字符,就将该索引加一,作为该索引的value
>
> // 如果字符之前出现过,就将所有大于该字符索引的value全改为该字符的value,此时ans变为value

> **反思**
>
> 结合45,==贪心算法在进行每一步的正确选择的边界可能是一个动态的区间,比如本题,也可能是先遍历区间后才能选择正确解,如45==
>
> ==两次/多次遍历思想==,前几次遍历找到需要的值,最后几次确定结果

**思路1  贪心  **

```java
class Solution {
    public List<Integer> partitionLabels(String s) {
        int[] last = new int[26];
        int length = s.length();
        for (int i = 0; i < length; i++) {
            last[s.charAt(i) - 'a'] = i;
        }
        List<Integer> partition = new ArrayList<Integer>();
        int start = 0, end = 0;
        for (int i = 0; i < length; i++) {
            end = Math.max(end, last[s.charAt(i) - 'a']);
            if (i == end) {
                partition.add(end - start + 1);
                start = end + 1;
            }
        }
        return partition;
    }
}
```



# 技巧

### <font  size=5 color="red">031.  下一个排列(未解出)</font>

整数数组的一个 **排列** 就是将其所有成员以序列或线性顺序排列。

- 例如，`arr = [1,2,3]` ，以下这些都可以视作 `arr` 的排列：`[1,2,3]`、`[1,3,2]`、`[3,1,2]`、`[2,3,1]` 。

整数数组的 **下一个排列** 是指其整数的下一个字典序更大的排列。更正式地，如果数组的所有排列根据其字典顺序从小到大排列在一个容器中，那么数组的 **下一个排列** 就是在这个有序容器中排在它后面的那个排列。如果不存在下一个更大的排列，那么这个数组必须重排为字典序最小的排列（即，其元素按升序排列）。

- 例如，`arr = [1,2,3]` 的下一个排列是 `[1,3,2]` 。
- 类似地，`arr = [2,3,1]` 的下一个排列是 `[3,1,2]` 。
- 而 `arr = [3,2,1]` 的下一个排列是 `[1,2,3]` ，因为 `[3,2,1]` 不存在一个字典序更大的排列。

给你一个整数数组 `nums` ，找出 `nums` 的下一个排列。

必须**原地** 修改，只允许使用额外常数空间。

 

**示例 1：**

```
输入：nums = [1,2,3]
输出：[1,3,2]
```

**示例 2：**

```
输入：nums = [3,2,1]
输出：[1,2,3]
```

**示例 3：**

```
输入：nums = [1,1,5]
输出：[1,5,1]
```

 

**提示：**

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 100`



> **思路1  **
>
> 我们需要将一个左边的「较小数」与一个右边的「较大数」交换，以能够让当前排列变大，从而得到下一个排列。
>
> 同时我们要让这个「较小数」尽量靠右，而「较大数」尽可能小。当交换完成后，「较大数」右边的数需要按照升序重新排列。这样可以在保证新排列大于原来排列的情况下，使变大的幅度尽可能小。
>
> **思路 1  改进**
>
> ==交换后后面的数据还是升序的 ,只需要头尾交换即可==



 **思路1  自己解法**

```java
class Solution {
    public void nextPermutation(int[] nums) {
        int c1 = -1, c2 = 0, temp; // 交换的两个数字的索引
        for(int i = nums.length - 2; i >= 0; i--){
            if(nums[i] < nums[i + 1]){
                c1 = i;
                break;
            }
        }

        if(c1 == -1){
            sort(nums, 0);
        }else{
           for(int i = nums.length - 1; i >= 0; i--){
               if(nums[i] > nums[c1]){
                   c2 = i;
                   break;
               }
           }
           temp = nums[c2]; 
           nums[c2] = nums[c1];
           nums[c1] = temp;
           sort(nums, c1 + 1);
        }
    }
    // 将left以及后面的进行排序,从小到大,nums长度为1,left=0
    // nums长度为0,left为0就可
    private void sort(int[] nums, int left){
        int min, index;
        for(int i = left; i <= nums.length - 2; i++){
            min = nums[i];
            index = i;
            for(int j = i + 1; j <= nums.length - 1; j++){
                if(nums[j] < min){
                    min = nums[j];
                    index = j;
                }
            }
            nums[index] = nums[i];
            nums[i] = min;            
        }
    }
}
```

**思路1  改进细节**

```java
class Solution {
    public void nextPermutation(int[] nums) {
        int i = nums.length - 2;
        while (i >= 0 && nums[i] >= nums[i + 1]) {
            i--;
        }
        if (i >= 0) {
            int j = nums.length - 1;
            while (j >= 0 && nums[i] >= nums[j]) {
                j--;
            }
            swap(nums, i, j);
        }
        reverse(nums, i + 1);
    }

    public void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }

    public void reverse(int[] nums, int start) {
        int left = start, right = nums.length - 1;
        while (left < right) {
            swap(nums, left, right);
            left++;
            right--;
        }
    }
}
```

### <font  size=5 color="red">169.  多数元素(未解出进阶)</font>

给定一个大小为 `n` 的数组 `nums` ，返回其中的多数元素。多数元素是指在数组中出现次数 **大于** `⌊ n/2 ⌋` 的元素。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

 

**示例 1：**

```
输入：nums = [3,2,3]
输出：3
```

**示例 2：**

```
输入：nums = [2,2,1,1,1,2,2]
输出：2
```

 

**提示：**

- `n == nums.length`
- `1 <= n <= 5 * 104`
- `-109 <= nums[i] <= 109`

 

**进阶：**尝试设计时间复杂度为 O(n)、空间复杂度为 O(1) 的算法解决此问题。

> **思路1  自己  哈希**
>
> 第7行 改写 `else if (--count == 0)`

> **思路2  投票法**
>
> 选第一个数为待选人,向后遍历,相同票数++,不同--,归零就换为当前的,票数置1
>
> 第7行 改写 `else if (--count == 0)`

**思路2  投票法**

```java
class Solution {
    public int majorityElement(int[] nums) {
        int cand_num = nums[0], count = 1, length = nums.length;
        for(int i = 1; i < nums.length; i++){
            if(nums[i] == cand_num){
                count++;
            }else{
                count--;
                if(count == 0){
                    cand_num = nums[i];
                    count = 1;
                }
            }
        }
        return cand_num;
    }
}
```



**思路1  自己  哈希**

```java
class Solution {
    public int majorityElement(int[] nums) {
        int len = nums.length;
        Map<Integer,Integer> map = new HashMap<>();
        for(int i = 0; i < len; i ++){
            if(map.containsKey(nums[i])){
                map.put(nums[i],map.get(nums[i])+1);
                if(map.get(nums[i])>len/2){
                return nums[i];
            }
            }else{
                map.put(nums[i],1);
            }
        }
        return nums[0];
    }
}
```



### <font  size=5 color="red">287.  寻找重复数</font>

给定一个包含 `n + 1` 个整数的数组 `nums` ，其数字都在 `[1, n]` 范围内（包括 `1` 和 `n`），可知至少存在一个重复的整数。

假设 `nums` 只有 **一个重复的整数** ，返回 **这个重复的数** 。

你设计的解决方案必须 **不修改** 数组 `nums` 且只用常量级 `O(1)` 的额外空间。

 

**示例 1：**

```
输入：nums = [1,3,4,2,2]
输出：2
```

**示例 2：**

```
输入：nums = [3,1,3,4,2]
输出：3
```

 

**提示：**

- `1 <= n <= 105`
- `nums.length == n + 1`
- `1 <= nums[i] <= n`
- `nums` 中 **只有一个整数** 出现 **两次或多次** ，其余整数均只出现 **一次**

 

**进阶：**

- 如何证明 `nums` 中至少存在一个重复的数字?
- 你可以设计一个线性级时间复杂度 `O(n)` 的解决方案吗？



> **思路1  自己  二分查找**

> ==**思路2  化为环形链表**==



**思路1  自己  二分查找**

```java
class Solution {
    public int findDuplicate(int[] nums) {
        int n = nums.length;
        int l = 1, r = n - 1;
        while (l <= r) {
            int mid = (l + r) >> 1;
            int cnt = 0;
            for (int i = 0; i < n; ++i) {
                if (nums[i] <= mid) {
                    cnt++;
                }
            }
            if (cnt <= mid) {
                l = mid + 1;
            } else {
                r = mid - 1;
            }
        }
        return l;
    }
}
```

**思路2  化为环形链表**

```java
class Solution {
    public int findDuplicate(int[] nums) {
        int slow = nums[0], fast = nums[nums[0]];
        while (slow != fast) {
            slow = nums[slow];
            fast = nums[nums[fast]];
        }
        fast = 0;
        while (slow != fast) {
            slow = nums[slow];
            fast = nums[fast];
        }
        return slow;
    }
}
```

