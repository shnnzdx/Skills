# Full Worked Example 02 — Top K Frequent Elements (347) / 第二份完整教学示范

> **Purpose:** Second long-form, learner-facing coaching exemplar. Use together with `VALID_SUDOKU_FULL_WALKTHROUGH.md`. The first example teaches **multiple simultaneous constraints and 2D coordinate mapping**; this example teaches a **multi-stage optimization journey: Brute Force → Hash Map + Sorting → Hash Map + Bucket Sort**. Reproduce the **reasoning depth, Python syntax instruction, numerical traces, quiz gates and hint-first code review**, not the specific problem content when coaching unrelated questions.
>
> **Source discipline:** The algorithms, examples, Python constructs and edge cases in Turns 1–3 are grounded in the previously saved **Problem 05 — Top K Frequent Elements (347)** study note. The turn boundaries, sample dialogue, extra quiz wording and optional mock submissions are **worked training illustrations**, not verified transcripts of the learner's historical answers or code attempts. Saved notes indicate understanding of sorting-key/lambda and an explanation of slices, but **do not establish an independently passed coding submission**.
>
> **Public-skill privacy:** Do not put personal progress histories, private Notion URLs, confidential documents, user identities or account data in this public repository. Saved-note links belong only in the user's actual conversation or private handoff.

---

# TURN 1 — LESSON: Input, Brute Force and Python foundations

## Illustrative user request

> 下一题：Top K Frequent Elements。给一个整数数组 nums 和整数 k，返回频率最高的 k 个不同元素。Medium。我们先理解题目，再讲暴力解法和 Python 基础。

## Learner-facing assistant response — full worked sample

我们进入 **Arrays & Hashing — Top K Frequent Elements (347)**。

这题连接了之前的 **Valid Anagram（统计出现次数）** 和 **Group Anagrams（Hash Map 分组）**，但多了一个要求：不仅要知道每个数字出现几次，还要选出**出现次数最多的 k 个不同数字**。

今天先从最直观的 **Brute Force** 出发，再理解它的低效之处。等你理解检查通过或主动要求“开始优化”，再进入 Hash Map + Sorting，最后再学习 Bucket Sort。

## Part 1 — Understand the Problem / 题意

给定：

```python
nums = [1, 2, 2, 3, 3, 3]
k = 2
```

题目要返回**出现次数最高的 2 个不同整数**，不是索引，也不是这些数字的频率。

| 数字 | 出现次数 |
| --- | --- |
| 1 | 1 |
| 2 | 2 |
| 3 | 3 |

所以：

```python
[3, 2]
# 或者 [2, 3]，如果题目允许任意返回顺序
```

**k 的含义：要返回多少种不同数字。** k 不是最低频率阈值，也不是从列表中取前 k 个元素。

再看：

```python
nums = [7, 7]
k = 1
# 输出 [7]
```

数字 7 出现两次，但它是**一个 distinct number**，因此输出只包含一个 7。

**Constraints（示例对应的 NeetCode 版本）：** 1 ≤ len(nums) ≤ 10⁴；−1000 ≤ nums[i] ≤ 1000；1 ≤ k ≤ distinct_count。可能有负数、重复数。题目保证 Top K 的答案集合唯一；**入选的多个元素内部可以同频**，但不会在“第 k 名的边界”出现导致可选集合不唯一的平局。

## Part 2 — Brute Force 的直觉

最直观的算法可以分为三步：

1. 从 nums 中找出所有**不同数字**，保存到 `unique`。
2. 对每个不同数字，重新扫描原数组，统计出现次数，得到 `frequencies` 记录。
3. 重复 k 次：找到当前频率记录中最大的一项，取其数字，然后删除这条记录，以免重复选中。

先不用 Sorting、Hash Map，便于理解“题目到底让我们计算什么”。

### Step 1 — 先去重

```python
unique = []

for num in nums:
    if num not in unique:
        unique.append(num)
```

对于：

```python
nums = [1, 2, 2, 3, 3, 3]
```

执行结果：

```python
unique = [1, 2, 3]
```

**Python `not in`：** 检查某个数字是否**不在**指定列表内。List 可能需要从头扫描，因此对大量不同数字可能较慢。

### Step 2 — 为每一个不同数字计算 Frequency

```python
frequencies = []

for num in unique:
    count = 0

    for x in nums:
        if x == num:
            count += 1

    frequencies.append([num, count])
```

这里为什么用**双层循环**？

- 外层：选择当前要统计的数字，例如 `num=2`。
- 内层：对整个 `nums` 重新扫描，看有多少次 `x==2`。

手动计算：

| 外层 num | 扫描 nums 时命中的次数 | 保存记录 |
| --- | --- | --- |
| 1 | 1 | [1,1] |
| 2 | 2 | [2,2] |
| 3 | 3 | [3,3] |

最终：

```python
frequencies = [[1, 1], [2, 2], [3, 3]]
```

### Python 重点：二维列表中的两个索引

```python
frequencies[0]     # [1, 1]：第一条记录
frequencies[0][0]  # 1：第一条记录的数字
frequencies[0][1]  # 1：第一条记录的频率

frequencies[2][0]  # 3：第三条记录的数字
frequencies[2][1]  # 3：第三条记录的频率
```

**下标 0 保存原始数字，下标 1 保存频率。** 别把两者混淆。

### Step 3 — 重复 k 次选出最大频率

```python
result = []

for _ in range(k):
    best_index = 0

    for i in range(1, len(frequencies)):
        if frequencies[i][1] > frequencies[best_index][1]:
            best_index = i

    result.append(frequencies[best_index][0])
    frequencies.pop(best_index)
```

逐行：

- `for _ in range(k)`：执行 k 轮。`_` 表示不需要使用当前循环下标。
- `best_index=0`：先假设剩余记录中第 0 项最高频。
- `frequencies[i][1]`：当前候选数字的频率。
- `frequencies[best_index][1]`：目前最高频候选的频率。
- `result.append(frequencies[best_index][0])`：取**数字**，不是取频率。
- `frequencies.pop(best_index)`：删除已选记录，否则下一轮又选到同一个最大值。

**对示例的具体执行：**

| 轮次 | 剩余记录 | best_index | 选中的数字 | result |
| --- | --- | --- | --- | --- |
| 1 | [[1,1],[2,2],[3,3]] | 2 | 3 | [3] |
| 2 | [[1,1],[2,2]] | 1 | 2 | [3,2] |

## Part 3 — 完整 Brute Force Python 代码

```python
class Solution:
    def topKFrequent(self, nums: list[int], k: int) -> list[int]:
        unique = []

        for num in nums:
            if num not in unique:
                unique.append(num)

        frequencies = []

        for num in unique:
            count = 0
            for x in nums:
                if x == num:
                    count += 1
            frequencies.append([num, count])

        result = []

        for _ in range(k):
            best_index = 0

            for i in range(1, len(frequencies)):
                if frequencies[i][1] > frequencies[best_index][1]:
                    best_index = i

            result.append(frequencies[best_index][0])
            frequencies.pop(best_index)

        return result
```

## Part 4 — Complexity / 不能只说“有两层 for”

令：

- `n = len(nums)`，输入总元素数；
- `m = len(unique)`，不同数字数；
- `k`，需要返回多少种不同数字。

满足 `1 ≤ k ≤ m ≤ n`。

**时间：**

- 通过 List `not in` 去重：O(nm)；
- 对 m 个不同数字逐个重扫 nums：O(nm)；
- 重复 k 次寻找当前最大频率，删除记录：O(km)（删除也可能移动列表元素）。

总计 **O(nm + km)**；最坏 m≈n，则 **O(n²)**。

**空间：** unique、frequencies、result 总量为 **O(m)**，最坏 **O(n)**。

算法没有错，但在多次统计和反复找最大值时做了大量重复工作。下一步优化需要想两个问题：**能不能一次遍历就统计所有数字？能不能不必逐轮扫描找 Top K？**

## Part 5 — Brute Force 理解检查

> **真实训练要在这里停止，先等学生作答；下列选项不预先公布答案。**

**Q1.** `nums=[4,4,2,2,2,5],k=2`，输出应是哪组数字？

- A. [4,2]
- B. [2,4]
- C. [2,2]

（如果题目允许任意顺序，A 和 B 均是同一正确集合。这类题**不应设计成单选 A/B 均真**。实际出题时应合并为 “A. [2,4]（任意顺序）”、 “B. [2,2]”、 “C. [3,2]” 等**唯一正确选项**。）

**实际建议修改后的 Q1：** `nums=[4,4,2,2,2,5],k=2`，哪一组值正确？
- A. [2,4]（任意顺序）
- B. [2,2]
- C. [4,5]

**Q2.** `frequencies=[[1,1],[2,2],[3,3]]`，`frequencies[2][1]` 是什么？
- A. 数字 3。
- B. 频率 3。
- C. 数字 2。

**Q3.** 为什么每轮选中频率最高的记录后要用 `pop(best_index)`？

**Q4.** 哪一步造成最严重的重复扫描？如果你有 Hash Map，最想替换掉哪个操作？

**STOP / 等待学生。** 不要直接进入排序或 Bucket Sort，除非学生自己明确要求一次看完或跳过测试。

---

# TURN 2 — OPTIMIZE 1: Hash Map + Sorting

## Illustrative user request

> 我已经理解暴力法，开始第一个优化方案。请详细讲 dict.get、sorted、lambda、reverse、切片。

## Learner-facing assistant response — full worked sample

我们进入 **Optimization 1 — Hash Map + Sorting**。

上一种暴力方法最大的浪费，是针对每个不同数字都把 `nums` 重扫一遍。现在先解决这个问题。

## Part 1 — Hash Map Frequency Counting / 频率字典

```python
count = {}

for num in nums:
    count[num] = count.get(num, 0) + 1
```

Dictionary / Hash Map 的结构：

- **Key** = 数字；
- **Value** = 这个数字出现的次数。

示例：

```python
nums = [1, 2, 2, 3, 3, 3]
count = {1: 1, 2: 2, 3: 3}
```

### 为什么是 `count.get(num,0)+1`？

`count.get(num,0)` 表示：

- 如果 `num` 是已有 Key，就取得之前记录的频率；
- 如果不存在，就用默认数字 0；
- 然后 +1，表示现在又见到它一次。

逐个模拟：

| 读入 num | count 状态 |
| --- | --- |
| 1 | {1:1} |
| 2 | {1:1,2:1} |
| 2 | {1:1,2:2} |
| 3 | {1:1,2:2,3:1} |
| 3 | {1:1,2:2,3:2} |
| 3 | {1:1,2:2,3:3} |

**一次遍历，统计全部数字。** 平均 Hash Map 操作为 O(1)，计数总计预期 O(n)。

## Part 2 — 为什么不能直接 `sorted(count)`？

这是本题最容易出现的 Python 理解错误之一。

```python
count = {4: 3, 5: 1, 6: 2}
sorted(count)
# [4, 5, 6]
```

为什么不是 `[4,6,5]`？

因为对 Dictionary 做 `sorted(count)` 默认遍历的是 **Key**，也就是数字 **4、5、6**，并根据数字本身排序，而不是根据 Value / Frequency 排序。

我们希望依次按照：

```text
4 → frequency 3
6 → frequency 2
5 → frequency 1
```

因此排序依据必须是 `count[x]`。

## Part 3 — 逐个解释 sorted 的三个参数

```python
sorted(count, key=lambda x: count[x], reverse=True)
```

### `sorted(iterable)`

返回一个排序后的**新列表**；这里 iterable 是 Dictionary，因此遍历的是数字 Key。

### `key=...`

指定“排序时比较哪项”。例如：

```python
words = ["hi", "banana", "cat"]
sorted(words, key=len)
# ["hi", "cat", "banana"]
```

它使用字符串**长度**而不是字母顺序排序。

### `lambda x: count[x]`

这是一个短小的匿名函数，含义是：

```python
def frequency(x):
    return count[x]
```

把当前数字 `x` 传进来，返回**这个数字的出现次数**。

所以排序中的 `x` 是**数字本身**，不是该数字的下标，也不是 Frequency。

### `reverse=True`

表示按排序依据**从大到小**排序。

```python
count = {4: 3, 5: 1, 6: 2}
ranked = sorted(count, key=lambda x: count[x], reverse=True)
# [4, 6, 5]
```

注意结果 `ranked` 中装的是原始**数字**，并不是频率 `[3,2,1]`。

## Part 4 — Slicing / `[:k]` 与 `[k]`

假设：

```python
ranked = [4, 6, 5]
k = 2
```

```python
ranked[:k]  # [4, 6]
ranked[k]   # 5
```

**`[:k]`**：从开头直到下标 k **之前**，一共选出 k 个元素。

**`[k]`**：只取下标恰好等于 k 的**一个**元素。

这里应返回 Top K 的**列表**，不能误写成 `ranked[k]`。

## Part 5 — 完整 Hash Map + Sorting Python 解法

```python
class Solution:
    def topKFrequent(self, nums: list[int], k: int) -> list[int]:
        count = {}

        for num in nums:
            count[num] = count.get(num, 0) + 1

        sorted_nums = sorted(
            count,
            key=lambda num: count[num],
            reverse=True,
        )

        return sorted_nums[:k]
```

更紧凑的等价写法：

```python
class Solution:
    def topKFrequent(self, nums: list[int], k: int) -> list[int]:
        count = {}
        for num in nums:
            count[num] = count.get(num, 0) + 1
        return sorted(count, key=lambda x: count[x], reverse=True)[:k]
```

建议初学时先独立写多行版本，逐步理解每个中间变量；熟练后再压缩代码。

## Part 6 — Manual trace 与复杂度

```python
nums = [1, 2, 2, 3, 3, 3]
k = 2
```

- Step 1：`count = {1:1, 2:2, 3:3}`；
- Step 2：按频率降序，`sorted_nums = [3,2,1]`；
- Step 3：`sorted_nums[:2] = [3,2]`。

令 m 为不同数字数量：

- 计数预期 O(n)；
- 对 m 个 Key 排序 O(m log m)；
- 切出 k 个数字 O(k)，且 k≤m。

**总时间预期 O(n + m log m)**，在最坏 m≈n 时渐近为 O(n log n)；**额外空间 O(m)**（Dictionary 和排序结果），最坏 O(n)。

这个版本简单易写，但排序**全部 m 个不同数字**并不总是必要。如果只需要 k 个，能否利用频率范围，避免比较排序？

## Part 7 — Optimization 1 理解检查

**Q1.** `count={4:3,5:1,6:2}`，`sorted(count,key=lambda x:count[x],reverse=True)` 的结果是什么？
- A. [4,5,6]
- B. [4,6,5]
- C. [3,2,1]

**Q2.** `lambda x:count[x]` 是按照什么排序？为什么？
- A. 数字的大小。
- B. 数字的出现次数。
- C. 数组索引。

**Q3.** `ranked=[4,6,5],k=2` 时，`ranked[:k]` 与 `ranked[k]` 各是什么？

**Q4.** 为什么 Hash Map 统计比“每个不同数字都重扫 nums”的办法节省时间？

**STOP / 等待学生。** 不要顺手把 Bucket Sort 也全部贴出来。学生主动说“下一步 Bucket Sort”时才推进。

---

# TURN 3 — OPTIMIZE 2: Hash Map + Bucket Sort

## Illustrative user request

> 好的我理解了排序方案，现在学习 Bucket Sort。请重点解释为什么是 n+1 个桶、Python 嵌套列表、.items()、反向 range、为什么两个 for 不是 O(n²)。

## Learner-facing assistant response — full worked sample

现在开始本题的最优渐近时间版本：**Hash Map + Bucket Sort（频率桶）**。

我们仍然首先用 Hash Map 计数，但之后不再用 Sorting 排列全部数字。先问一个问题：

**一个数字最多出现多少次？**

如果 nums 长度为 n，那么某个数字最多只能出现 **n 次**（全部都是同一个数字）。

因此每个数字的频率只可能是整数 1、2、3……n。

**可以把“频率”直接当成数组下标。**

## Part 1 — Bucket index 是频率，不是数字本身

继续用：

```python
nums = [1, 2, 2, 3, 3, 3]
count = {1: 1, 2: 2, 3: 3}
```

我们想建立：

```text
frequency 0: []
frequency 1: [1]
frequency 2: [2]
frequency 3: [3]
frequency 4: []
frequency 5: []
frequency 6: []
```

这里 `buckets[2]` 的含义是：**所有出现 2 次的原始数字**，不是数字 2 必须放在下标 2。

假设另一个数字 -7 出现 2 次，应该放在：

```python
buckets[2].append(-7)
```

即使原始数字是负数，**Bucket 的下标也还是非负的频率**。

## Part 2 — 为什么一定要 n+1 个桶？

假设：

```python
nums = [5, 5, 5, 5]
n = 4
```

数字 5 的频率是 4，我们必须能够访问 `buckets[4]`。

Python 列表下标从 0 开始，因此要容纳下标 0、1、2、3、4，总共 **n+1=5** 个位置。

代码：

```python
n = len(nums)
buckets = [[] for _ in range(n + 1)]
```

### Python List Comprehension

```python
[[] for _ in range(n + 1)]
```

等价于：

```python
buckets = []

for _ in range(n + 1):
    buckets.append([])
```

每次创建**一个新的空 List**。

### 非常重要的 Python 陷阱

**错误写法：**

```python
buckets = [[]] * (n + 1)
```

这会让所有位置共享**同一个内部列表对象**。

想象：

```python
buckets = [[]] * 3
buckets[1].append(42)

print(buckets)
# [[42], [42], [42]]
```

你原本只想改变 Bucket 1，却让 Bucket 0 和 Bucket 2 都出现了 42。

**正确写法：**

```python
buckets = [[] for _ in range(3)]
buckets[1].append(42)

print(buckets)
# [[], [42], []]
```

这是一个需要真正理解的 Python 引用语义问题，而不是纯背代码。

## Part 3 — `.items()` 与 Bucket 分配

```python
for num, freq in count.items():
    buckets[freq].append(num)
```

### Python `dict.items()`

字典中的每个条目有一对值：

- Key：数字 `num`；
- Value：频率 `freq`。

`count.items()` 就是逐个取出这些 (Key, Value)。

**注意：`enumerate(nums)` 返回的是 (下标, 元素)，不是 (数字, 频率)。** 不要把两个 API 混淆。

示例：

```python
count = {1:1, 2:2, 3:3}
```

遍历：

| num | freq | 执行动作 |
| --- | --- | --- |
| 1 | 1 | buckets[1].append(1) |
| 2 | 2 | buckets[2].append(2) |
| 3 | 3 | buckets[3].append(3) |

如果 n=6：

```python
buckets = [
    [],   # freq 0 — unused
    [1],  # freq 1
    [2],  # freq 2
    [3],  # freq 3
    [],   # freq 4
    [],   # freq 5
    [],   # freq 6
]
```

### 为什么一个桶要是 List？

不同数字可能有相同频率，例如：

```python
nums = [1,1,2,2,3]
count = {1:2, 2:2, 3:1}
buckets[2] = [1,2]
```

如果桶只保存**一个数字**，就会丢失同频的另一个数字。所以每个 `buckets[freq]` 都应是一个列表。

## Part 4 — 从高频往低频扫描

我们需要 Top K，所以从可能的最高频率 n 一直向下降到 1：

```python
result = []

for freq in range(n, 0, -1):
    for num in buckets[freq]:
        result.append(num)

        if len(result) == k:
            return result
```

### Python `range(start, stop, step)` 反向遍历

`range(n,0,-1)` 依次给出：

```text
n, n-1, ..., 3, 2, 1
```

**不含 0**，因为 stop 不包含在区间里。

为什么从高到低？因为我们要先拿到**频率最高**的数字。

为什么在循环内部使用：

```python
if len(result) == k:
    return result
```

因为只要已经取出 **k 个不同数字**，就不需要继续访问剩余桶。不能不考虑 k 一直把所有数字都返回。

### 手动推演

初始：

```python
nums = [1, 2, 2, 3, 3, 3]
k = 2
n = 6
```

| freq | buckets[freq] | result（处理后） |
| --- | --- | --- |
| 6 | [] | [] |
| 5 | [] | [] |
| 4 | [] | [] |
| 3 | [3] | [3] |
| 2 | [2] | [3,2] → 满 k，返回 |

数字 1 位于 Bucket 1，但我们**不会访问到它**，因为已经收集够两个元素。

## Part 5 — Bucket Sort 完整 Python 代码

```python
class Solution:
    def topKFrequent(self, nums: list[int], k: int) -> list[int]:
        # Step 1 — Count frequencies
        count = {}

        for num in nums:
            count[num] = count.get(num, 0) + 1

        # Step 2 — Create buckets and group by frequency
        n = len(nums)
        buckets = [[] for _ in range(n + 1)]

        for num, freq in count.items():
            buckets[freq].append(num)

        # Step 3 — Collect from highest frequency to lowest
        result = []

        for freq in range(n, 0, -1):
            for num in buckets[freq]:
                result.append(num)

                if len(result) == k:
                    return result

        return result
```

**三步记忆线索：**

1. `count[num]`：这个数字出现了几次？
2. `buckets[freq].append(num)`：把这个数字放到它对应的频率桶。
3. 反向扫描 Bucket：从最高频率到最低频率收集，达到 k 个立即停止。

## Part 6 — “为什么两个 for 嵌套不是 O(n²)？”

你可能会觉得：

```python
for freq in range(n, 0, -1):
    for num in buckets[freq]:
        result.append(num)
```

有两个嵌套 for，应该自动是 O(n²)。

**这是错误的 Big O 分析方式。**

真正要看的是**内层循环跨全部外层迭代总共执行多少次**。

- 外层最多检查 n 个桶，所以 O(n)。
- 所有桶加起来只存储 **m 个不同数字**，每个数字只在一个桶出现一次。
- 因此内层循环遍历**所有桶的总次数**最多是 m，而不是每个桶重复 m 次。

所以总工作量是：

**O(n + m)**

由于 m ≤ n，简化成 **O(n)**。

这和“每个桶都有 n 个元素，所以 n×n”是完全不同的结构。用下表思考：

| Bucket | 元素个数 |
| --- | --- |
| Bucket 1 | 2 |
| Bucket 2 | 3 |
| Bucket 3 | 0 |
| Bucket 4 | 1 |
| 所有桶合计 | 6 次内部迭代 |

内层总工作量是 2+3+0+1=6，不是每个 Bucket 都执行 6 次。

## Part 7 — Complexity Comparison

令 n=len(nums)、m=不同数字数、k≤m≤n。

| 方法 | 时间 | 额外空间 | 思路 |
| --- | --- | --- | --- |
| Brute Force | O(nm + km)，最坏 O(n²) | O(m) | 反复计数与逐轮取最大值 |
| Hash Map + Sorting | 预期 O(n + m log m) | O(m) | 计数后完整排序 |
| Hash Map + Bucket Sort | 预期 O(n) | O(n) | 用频率当数组下标，避免比较排序 |

Bucket Sort 的成本：

- 频率统计：预期 O(n)；
- 初始化 n+1 个桶：O(n)；
- 把 m 个不同数字分配到桶：预期 O(m)；
- 扫描桶和其中数字：O(n+m)。

因此**总预期时间 O(n)，额外空间 O(n)**。

这里 O(n) 表示优化的渐近上界；并非保证 Python 在所有小样本里 Bucket Sort 都比 Sorting 运行得更快。

## Part 8 — Edge Cases / 边界案例

| 输入 | 期望结果（顺序可不同） | 为什么 |
| --- | --- | --- |
| [7,7], k=1 | [7] | 只有一个不同 Key |
| [-2,-2,3], k=1 | [-2] | 负数可以作为 Dictionary Key；Bucket 下标是频率 |
| [1,1,2,2,3], k=2 | [1,2] | 两个入选数字可以同频 |
| [1,2,3], k=3 | [1,2,3] | k 等于不同数字数 |
| [5,5,5,5], k=1 | [5] | 最高频率可以等于 n，说明必须创建 n+1 个桶 |

题目保证 Top K 的集合唯一，这与**选中的多个数字可以并列同频**不矛盾；不唯一的是第 k 名边界出现同频并且只允许取其中一部分的情况。

## Part 9 — Interview English / 英文面试表达

“I first count how many times each unique number appears using a hash map.”

我先用哈希表统计每个不同数字的频率。

“One straightforward option is to sort the unique numbers by their frequencies in descending order and return the first k.”

比较直接的方法是对不同数字按频率降序排序，再返回前 k 个。

“To avoid sorting, I can leverage the fact that no frequency can exceed the array length n.”

为了避免完整排序，我利用频率最多为数组长度 n 的性质。

“I create an array of frequency buckets, and each bucket contains all numbers that occur that many times.”

我创建频率桶数组，每个桶存放所有恰好出现对应次数的原始数字。

“Then I scan from the highest possible frequency down and stop once I collect k numbers.”

然后从最高频率向低频率扫描，收集到 k 个数字立即停止。

“Assuming average constant-time hash map operations, this runs in O(n) time and uses O(n) extra space.”

按照哈希表平均常数时间的假设，这个方法的时间为 O(n)，额外空间为 O(n)。

## Part 10 — Bucket Sort 理解检查（不得预先透露答案）

**Q1.** nums 长度为 4，为什么要创建 `buckets=[[] for _ in range(5)]` 而不是 4 个桶？

**Q2.** `buckets[freq].append(num)` 中，`freq` 和 `num` 各自表示什么？

**Q3.** `range(6,0,-1)` 实际生成哪些数字？为什么不包含 0？

**Q4.** 为什么 Bucket 的两层 for 循环整体是 O(n+m)，而不是 O(n²)？

**STOP.** 学生提交答案后先逐题评价，再安排独立代码练习。

---

# TURN 4 — OPTIONAL: Illustrative Quiz Grading (synthetic, not verified student history)

## Hypothetical learner response

> Q1 因为下标要包含 n；Q2 freq 是原数组数字，num 是出现次数；Q3 6,5,4,3,2,1；Q4 因为所有桶里的数字合起来不超过 n。

## Example coach feedback

Q1、Q3、Q4 正确；Q2 把两个变量的意义颠倒了。

| 题 | 回答 | 评价 |
| --- | --- | --- |
| Q1 | 包含下标 n | 正确 |
| Q2 | freq 是数字，num 是次数 | 错误 |
| Q3 | 6、5、4、3、2、1 | 正确 |
| Q4 | 所有桶里数字总数不超过 n | 正确 |

**Q2 的定位解释：**

```python
count = {1:1, 2:2, 3:3}
for num, freq in count.items():
    buckets[freq].append(num)
```

`num` 是 Dictionary 的 Key，即原始数字；`freq` 是 Value，即这个数字出现了几次。

在 `num=3, freq=3` 的简单例子里两者碰巧相同，容易误导。换成：

```python
count = {-7: 2}
```

此时：

```python
num = -7
freq = 2
buckets[2].append(-7)
```

这样才更容易看到变量各自的职责。请用新的例子再解释一次 `buckets[freq]` 的下标含义。

**Important:** The hypothetical answer above was fabricated only for training this skill. Do not treat it as actual user progress or store it as such.

---

# TURN 5 — LIVE CODING: 两轮独立代码练习

Only start after conceptual understanding or an explicit instruction to start. **Do not show the fully worked reference solution here.**

## Round 1 — Hash Map + Sorting

```python
class Solution:
    def topKFrequent(self, nums: list[int], k: int) -> list[int]:
        # Build frequency map

        # Sort distinct numbers by frequency

        # Return exactly k distinct numbers
        pass
```

Ask learner to explain `get(num,0)`, `sorted`, `lambda`, `reverse=True`, and `[:k]`.

## Round 2 — Hash Map + Bucket Sort

```python
class Solution:
    def topKFrequent(self, nums: list[int], k: int) -> list[int]:
        # Step 1: Count frequencies

        # Step 2: Create independent frequency buckets

        # Step 3: Put numbers into buckets

        # Step 4: Scan from largest frequency down

        pass
```

**Give tests, not answers disguised as instructions:**

```python
([1,2,2,3,3,3], 2)   # {2,3}
([7,7], 1)           # {7}
([-2,-2,3], 1)       # {-2}
([1,1,2,2,3], 2)     # {1,2}
([1,2,3], 3)         # {1,2,3}
([5,5,5,5], 1)       # {5}
```

Correctness should compare **result sets** when answer order may differ, while also verifying returned length = k and no duplicate result elements. Do not accidentally let `set(actual)==set(expected)` alone pass a result of wrong length or duplicate entries.

## Round 2 interviewer review categories

1. Signature / indentation / Python syntax.
2. Dictionary count and `get` logic.
3. Independent bucket initialization (`[[] for _ in range(n+1)]` vs aliased lists).
4. Correct `items()` unpacking and `buckets[freq]` indexing.
5. Correct reverse `range` bounds; scan n through 1.
6. Append individual **num**, not the bucket list itself.
7. Stop exactly when the result has k **different** elements.
8. Expected vs actual on the supplied cases.
9. Actual time/space of submitted implementation.
10. Evidence distinction: **executed** vs **manual trace only**.

---

# TURN 6 — OPTIONAL: Hint-first code-review example (synthetic, not verified student history)

## Hypothetical buggy submission

```python
class Solution:
    def topKFrequent(self, nums: list[int], k: int) -> list[int]:
        count = {}
        for num in nums:
            count[num] = count.get(num, 0) + 1

        n = len(nums)
        buckets = [[]] * (n + 1)

        for num, freq in count.items():
            buckets[freq].append(num)

        result = []
        for freq in range(n, 0, -1):
            for num in buckets[freq]:
                result.append(num)
                if len(result) == k:
                    return result

        return result
```

## Interviewer feedback — first hint, not full fix

你正确地使用了 Hash Map 计数，并且知道从最高频率向下扫描 Bucket。

但你的数组初始化有一个关键问题，导致不同频率的数字可能混到同一个桶里。

**定位在：**

```python
buckets = [[]] * (n + 1)
```

这行语法能够运行，但不意味着它创建了 n+1 个独立的列表。

**最小反例：**

```python
buckets = [[]] * 3
buckets[1].append(7)
print(buckets)
```

请你在不看答案的前提下预测输出。为什么会这样？之前我们学过的 List Comprehension 可以避免什么共享引用问题？

**提示级别：H1（定位）+ H2（最小反例）。** 不直接把正确代码整段改写出来。学生提交修复版本后，才重新检查并测试。

If an assistant can run tests, it may execute the submitted code and state exact results. Otherwise it must say “静态评审／手动推演”，不能谎称运行过任何测试。

---

# TURN 7 — SAVE TO NOTION (only if the user asks)

This is an explicit-write workflow, not a routine automatic step.

1. Resolve existing Algorithm Interview Prep / Arrays & Hashing parent.
2. Find and update this problem's existing page rather than creating a duplicate.
3. Include **Brute Force, Hash Map + Sorting, Hash Map + Bucket Sort**, exact code and syntax, frequency counting diagram, bucket distribution diagram, reverse scan trace, complexities, edge cases, interview phrases and quiz/coding checklist.
4. Verify the Notion write and give the tool-returned page link.
5. Say **“笔记已经保存”** only when verification succeeds. Saving study notes never means student code passed.

Illustrative report after a **verified** write:

> 已将 **Problem 05 — Top K Frequent Elements (347)** 归档至 Arrays & Hashing Roadmap。包含三种算法、Python 语法、手动推演、Bucket 图解、复杂度、边界测试和英文面试表达。下一步按我们的节奏继续独立写 Hash Map + Sorting，然后写 Bucket Sort。

---

# Cross-example rules / 第二个示范教会新会话什么？

Valid Sudoku 的示范是 **一个元素同时满足多个区域约束**：通过 Row/Column/Box Sets 进行单次遍历。

Top K 的示范是 **同一道题的多轮优化**：
- **Brute Force → Hash Map + Sorting**：使用统计字典消除重复计数，理解 `sorted(..., key=lambda ..., reverse=True)` 和 `[:k]`。
- **Hash Map + Sorting → Bucket Sort**：利用整数频率范围省去比较排序，理解 `n+1`、独立内部列表、`dict.items()`、反向 `range`、嵌套循环的总工作量不是 O(n²)。

**教学上不要把三个完整方法一次性倾倒给正在分阶段学习的用户。** 把上面的“完整”理解为可供其他 AI 参照的多个连续**教学回合**。用户明确要求完整一篇时才合并输出。Quiz、独立 Coding 与答案审查有各自停止点。

**This file is a worked teaching exemplar, not a statement that the real learner solved all these stages independently.**
