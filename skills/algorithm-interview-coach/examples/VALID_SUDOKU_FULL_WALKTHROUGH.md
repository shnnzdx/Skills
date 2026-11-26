# Full Worked Example — Valid Sudoku (36) / 完整教学示范

> **Purpose:** This is a **concrete exemplar of the expected learner-facing teaching output**, not just an outline or a problem solution. A future assistant should study its level of detail, pacing, worked examples, code explanations, conceptual questions, and transition rules **before** coaching a new problem. Reproduce the **teaching method and depth**, not this Sudoku content when the user is learning a different problem.
>
> **Provenance and scope:** Based on the actual Valid Sudoku lesson, follow-up optimization lesson and later Notion-save request in the coaching conversation. The static board illustrations below replace ephemeral interactive widgets. Optional example quiz grading and code review near the end are **illustrative training examples, not past user submissions**. In the actual session, the learner requested the next optimized lesson without answering the first quiz and later requested saving notes; do **not** represent those quizzes or independent coding as completed.
>
> **Public-repository rule:** Do not paste personal study logs, private Notion links, account data, proprietary sources, or automatically generated UI internals into this public exemplar.

---

# CONVERSATION TURN 1 — New problem / 新题：先教学 Brute Force

## User message (example)

> 下一题：Valid Sudoku。给定一个 9×9 的 Sudoku board。如果每一行、每一列、每个 3×3 Sub-box 的数字 1–9 都不重复，则返回 true，否则返回 false。空格为 "."。棋盘不需要填满或可解。Example 1 返回 true；Example 2 因左上九宫格出现两个 "1" 返回 false。Constraints: board.length=9; 每行长度 9；每格是 "1"–"9" 或 "."。

## Assistant — FULL learner-facing example response

我们正式进入 **NeetCode Roadmap — Valid Sudoku (36)**。

这道 Medium 题会把我们之前学习的 **Contains Duplicate、Hash Set、二维数组（2D Array）和坐标索引** 结合起来。难点是：一个数字不仅要检查所在的 Row（行）、Column（列），还要检查它所在的 **3×3 Sub-box（九宫格）**。

我们继续使用熟悉的训练节奏：**先理解题意和 Python 二维数组 → Brute Force → 手动推演 → 复杂度 → 理解检查**。在你要求“开始优化”之后，再进入单次遍历 Hash Set 解法。

## Part 1 — Understand the Problem / 题目理解

输入是一张 **9×9 的数独棋盘**。必须同时满足：

1. **Row（行）**：每一行中，非空数字不能重复。
2. **Column（列）**：每一列中，非空数字不能重复。
3. **3×3 Sub-box（九宫格）**：九个小方格中，每个小方格内的非空数字不能重复。

**`"."` 表示空白位置，不是数字。** 这个符号可以在同一行、列、Box 出现多次。

**Valid 不等于 Solved。** 这里不要求 Sudoku 填满，更不要求判断 Sudoku 是否存在完整解。只检查当前棋盘有没有违反规则。

### 用 Example 2 认识“只检查行列还不够”

无效棋盘的关键左上角 3×3 区域：

```text
         col 0  col 1  col 2
row 0      1      2      .
row 1      4      .      .
row 2      .      9      1
```

- `board[0][0] = "1"`
- `board[2][2] = "1"`
- 两个 "1" **不在同一行**，也**不在同一列**。
- 但它们处于相同的 **左上角 3×3 Box**。

因此返回 `False`。

**关键结论：即使所有 Row 和 Column 都没有重复，也不能保证 Sudoku 有效。**

如果聊天界面支持交互式视觉组件，可以展示原题 9×9 棋盘，并设置 **Row / Column / 3×3 Box** 三种高亮模式。无论视觉是否可用，上面的文本解释必须独立完整。

## Part 2 — Python 2D Array / 二维数组

以前的数组：

```python
nums = [1, 2, 3]
```

现在的二维列表：

```python
board = [
    ["1", "2", "3"],
    ["4", "5", "6"],
    ["7", "8", "9"],
]
```

二维列表是 **List of Lists**。访问方式为：

```python
board[row][col]
```

第一个索引决定第几行，第二个索引决定该行中的第几列。

```python
board[0][0]   # "1"
board[0][2]   # "3"
board[1][0]   # "4"
board[1][2]   # "6"
board[2][1]   # "8"
```

**行和列固定哪个变量？**

- 检查第 0 行：固定 `row=0`，依次改变 `col=0...8`。
- 检查第 0 列：固定 `col=0`，依次改变 `row=0...8`。

```python
# 遍历第 0 行
for col in range(9):
    value = board[0][col]

# 遍历第 0 列
for row in range(9):
    value = board[row][0]
```

对比：

| 检查对象 | 固定 | 变化 |
| --- | --- | --- |
| Row | row | col |
| Column | col | row |

Python 里的 `board[r][c]` 不是“第 r 个元素 + 第 c 个元素”，而是**先定位行，再定位列中的元素**。

## Part 3 — Brute Force：先解决 Row

一个很自然的办法是：对于当前一行，新建 `seen=[]`。扫描该行每个格子，跳过 `"."`。如果数字已经出现，就返回 `False`；否则把它加入 `seen`。

```python
for row in range(9):
    seen = []

    for col in range(9):
        value = board[row][col]

        if value == ".":
            continue

        if value in seen:
            return False

        seen.append(value)
```

### 逐行解释

```python
for row in range(9):
```

从第 0 行一直检查到第 8 行。

```python
seen = []
```

**每一行都创建一个新的记录列表。** 因为不同的行允许出现相同数字；如果所有 Row 共享一个 `seen`，就会把合法的跨行数字误判为重复。

```python
for col in range(9):
    value = board[row][col]
```

定位当前 Row 中第 col 列的值。

```python
if value == ".":
    continue
```

`continue` 代表**跳过当前这一轮循环，进入下一次迭代**。空白不是数字，不纳入查重。

```python
if value in seen:
    return False
```

发现当前行已经保存过这个数字，说明同一 Row 重复，直接失败。

```python
seen.append(value)
```

只有确认没有重复，才保存当前数字。

## Part 4 — Column 检查

Row 和 Column 使用完全相同的思路，但循环顺序互换：

```python
for col in range(9):
    seen = []

    for row in range(9):
        value = board[row][col]

        if value == ".":
            continue

        if value in seen:
            return False

        seen.append(value)
```

**Row**：外层固定 row，内层扫描 col。**Column**：外层固定 col，内层扫描 row。

不要让看起来类似的双循环掩盖了“谁固定、谁在变化”的本质区别。

## Part 5 — 最难的 3×3 Sub-box

九个 3×3 Box 的左上角位置为：

```text
(0,0)  (0,3)  (0,6)
(3,0)  (3,3)  (3,6)
(6,0)  (6,3)  (6,6)
```

它们的行起点、列起点都是 `0,3,6`。因此我们可以使用：

```python
range(0, 9, 3)
```

### Python `range(start, stop, step)` 语法

- `start`：从哪里开始。
- `stop`：在哪里**之前**结束，不包含该值。
- `step`：每次增加多少。

`range(0,9,3)` 依次得到 `0,3,6`。

选中一个 Box：

```python
for box_row in range(0, 9, 3):
    for box_col in range(0, 9, 3):
        ...
```

选中左上角 `(0,0)` 时，Box 内九个坐标是：

```text
(0,0) (0,1) (0,2)
(1,0) (1,1) (1,2)
(2,0) (2,1) (2,2)
```

因此再加两层循环扫描当前 Box：

```python
for row in range(box_row, box_row + 3):
    for col in range(box_col, box_col + 3):
        value = board[row][col]
```

### Box 检查完整代码

```python
for box_row in range(0, 9, 3):
    for box_col in range(0, 9, 3):
        seen = []

        for row in range(box_row, box_row + 3):
            for col in range(box_col, box_col + 3):
                value = board[row][col]

                if value == ".":
                    continue

                if value in seen:
                    return False

                seen.append(value)
```

四层循环的实际分工：

1. 第一层选 Box 的起始 Row。
2. 第二层选 Box 的起始 Column。
3. 第三层扫描**当前 Box 内**三个 Row。
4. 第四层扫描**当前 Box 内**三个 Column。

**前两层选九宫格，后两层检查九宫格。**

例：`box_row=3,box_col=6` 时，覆盖 Row `3,4,5` 和 Column `6,7,8`。注意 `range(3,6)` 不含 6。

## Part 6 — Brute Force 完整 Python 解法

```python
class Solution:
    def isValidSudoku(self, board: list[list[str]]) -> bool:

        # Step 1: Check rows
        for row in range(9):
            seen = []

            for col in range(9):
                value = board[row][col]

                if value == ".":
                    continue

                if value in seen:
                    return False

                seen.append(value)

        # Step 2: Check columns
        for col in range(9):
            seen = []

            for row in range(9):
                value = board[row][col]

                if value == ".":
                    continue

                if value in seen:
                    return False

                seen.append(value)

        # Step 3: Check 3x3 boxes
        for box_row in range(0, 9, 3):
            for box_col in range(0, 9, 3):
                seen = []

                for row in range(box_row, box_row + 3):
                    for col in range(box_col, box_col + 3):
                        value = board[row][col]

                        if value == ".":
                            continue

                        if value in seen:
                            return False

                        seen.append(value)

        return True
```

注意三个关键点：

- 只要发现重复就立刻返回 `False`。
- 每个新的 Row、Column 或 Box 有自己的独立 `seen`。
- **全部三个规则都通过以后**，才在函数最后返回 `True`；不能在中途提前返回。

## Part 7 — Complexity Analysis / 复杂度

**Sudoku 题目固定 9×9**：棋盘只有 81 格，规模不会随输入参数增长，所以严格针对这题可表述为：

| 指标 | 复杂度 |
| --- | --- |
| Time | O(1) |
| Auxiliary Space | O(1) |

这里 O(1) **不代表只计算一次**，而是工作量有固定上限。

如果推广成具有对应子方格定义的 **N×N** 棋盘，同时仍使用 `List` 的 `value in seen` 查重，那么最多 O(N²) 个格子分别需要做 O(N) 查找，故最坏时间可达 **O(N³)**，辅助空间 **O(N)**。

下一阶段 Hash Set 会把平均成员查询降到 O(1)，再进一步用一次 Board 遍历同时完成所有约束检查。

## Part 8 — Understanding Check / 理解检查

**此处停下，先让学生作答；不要附正确答案。**

**Q1. 如果每一行、每一列都没有重复数字，Board 一定有效吗？**

- A. 是，因为已经检查 Row 和 Column。
- B. 不一定，还要检查每个 3×3 Sub-box。
- C. 是，只要 Board 还没填满。

**Q2. 为什么必须跳过 `"."`？**

- A. 因为 "." 是空白，可以重复出现。
- B. 因为 "." 表示数字 0。
- C. 因为 Python 无法把 "." 放进 List。

**Q3. 当 `box_row=3,box_col=6` 时，`range(box_row,box_row+3)` 和 `range(box_col,box_col+3)` 检查哪个区域？**

- A. Row 3–5，Column 6–8。
- B. Row 3–6，Column 6–9。
- C. Row 0–2，Column 6–8。

**Q4. 为什么每次检查新的 Row、Column 或 Box 都要重新创建 `seen=[]`？**

请用自己的话解释：如果所有行共享同一个 `seen`，会发生什么错误？

**停止点 / Stop gate：** 如果学生提交 Quiz，逐题评价后再进入优化；如果学生直接明确说“开始优化”，说明他们选择跳过 Quiz，不要擅自认定 Quiz Passed。

---

# CONVERSATION TURN 2 — User explicitly asks: “好的开始优化方案”

## Assistant — FULL learner-facing optimization lesson

我们正式进入 **Valid Sudoku（36）— Optimized Solution：Hash Set**。

上一阶段我们已经学会单独检查 Row、Column 和 3×3 Sub-box。现在要进一步优化：

**能不能只遍历整个 Sudoku Board 一次，就同时完成三种检查？**

答案是可以。关键是：

1. `set()`：快速检测某个数字是否已经出现。
2. `//`：根据 Row、Column 下标确定所属 3×3 Box。
3. Hash Map + Hash Set：给每个 Row、Column 和 Box 维护独立的已见数字记录。

## Part 1 — 从 Brute Force 到 Hash Set

Brute Force 通过 List 查重：

```python
seen = []

if value in seen:
    return False

seen.append(value)
```

List 成员查询需要在最坏情况下逐个比较元素。Set 的平均查找则是 O(1)。

### `set()` 和 `add()`

```python
seen = set()

seen.add("1")
seen.add("3")
seen.add("5")

print("3" in seen)  # True
print("8" in seen)  # False
```

| 操作 | List | Set |
| --- | --- | --- |
| 创建 | [] | set() |
| 添加 | append(x) | add(x) |
| 成员测试 | x in list | x in set |
| 允许重复元素 | 是 | 否 |
| 成员查询 | O(k) | 平均 O(1) |

**Set 已有元素再次被 `add` 不会报错。** 因此我们仍然需要先检测重复，再加入集合。

## Part 2 — 为什么需要三组独立的 Set？

Sudoku 有 9 个 Row、9 个 Column、9 个 3×3 Box。

例如，`board[0][0]="1"` 与 `board[4][4]="1"` 可以同时存在，因为它们不在同一行、列、Box。若所有区域只用一个 Set，第二次遇到 "1" 就会**错误**地返回 False。

因此我们需要：

```python
rows = [set() for _ in range(9)]
cols = [set() for _ in range(9)]
boxes = {}
```

### `[set() for _ in range(9)]` 是什么？

**List Comprehension（列表推导式）**：每轮执行 `set()`，创建一个**新的**独立集合。`_` 代表不使用的循环变量。

不要写：

```python
rows = [set()] * 9
```

这样九个位置会引用**同一个** Set，改动其中一个会影响所有位置。它和之前 Bucket Sort 的 `[[]] * n` 共享引用问题相同。

`rows[0].add("5")` 表示第 0 行已经见过 "5"；`cols[3].add("7")` 表示第 3 列已经见过 "7"。

### 为什么 Box 使用 Dictionary？

可以为每个九宫格用一个 Tuple 作为 Key：

```python
boxes = {}
boxes[(0, 0)] = set()
boxes[(0, 0)].add("1")
```

这里 `(0,0)` 表示左上角的九宫格；`(1,2)` 表示中间右侧的九宫格。Tuple 可 Hash，因此可作为 Dictionary Key。

我们只在第一次遇到 Box 时建立 Set：

```python
if box not in boxes:
    boxes[box] = set()
```

否则直接读取一个尚不存在的 Key 会得到 `KeyError`。

## Part 3 — 核心：`(row // 3, col // 3)`

这是 Valid Sudoku 最关键的新技巧：

```python
box = (row // 3, col // 3)
```

Python 的 `//` 是 **Floor Division（向下取整除法）**：

```python
7 / 3     # 2.333333...
7 // 3    # 2
8 // 3    # 2
9 // 3    # 3
```

对 9×9 Board：

| Row / Column | index // 3 | 组别 |
| --- | --- | --- |
| 0、1、2 | 0 | 第一组 |
| 3、4、5 | 1 | 第二组 |
| 6、7、8 | 2 | 第三组 |

- `row // 3` 决定当前格子位于上 / 中 / 下哪一组 Box。
- `col // 3` 决定当前格子位于左 / 中 / 右哪一组 Box。

**九宫格映射图：**

```text
              c=0..2      c=3..5      c=6..8
r=0..2         (0,0)       (0,1)       (0,2)
r=3..5         (1,0)       (1,1)       (1,2)
r=6..8         (2,0)       (2,1)       (2,2)
```

例如：

```python
row = 5
col = 7

box = (row // 3, col // 3)
# (1, 2)
```

注意，`(1,2)` 与 `(2,1)` 是两个不同的 Tuple，也是不同 Box。

### 为什么不能用 `(row,col)` 作为 Box Key？

因为 `(row,col)` 区分的是**每个单独的格子**；我们希望同一个 Box 内 9 个格子都属于**相同的 Key**。因此必须除以 3 进行分组。

**视觉建议：** 如支持可交互元素，让用户点击 9×9 Board 中任意坐标，展示当前 Row、Column、Box Key 和被高亮的 3×3 区域。例如点击 (5,7) 后显示 (5//3,7//3)=(1,2)。不要让视觉取代可阅读的文本推导。

## Part 4 — 同时检查三种约束

对于任意非空格子 `board[r][c]`：

```python
value = board[r][c]
box = (r // 3, c // 3)
```

我们要检查：

```python
value in rows[r]
value in cols[c]
value in boxes[box]
```

分别代表：同行重复？同列重复？同 Box 重复？

只要其中**任何一个条件**为 True，整个 Sudoku 就不合法：

```python
if (
    value in rows[r]
    or value in cols[c]
    or value in boxes[box]
):
    return False
```

`or` 是逻辑“或”。三种约束都无冲突时，再将该数字登记到三个对应集合：

```python
rows[r].add(value)
cols[c].add(value)
boxes[box].add(value)
```

**为什么 add 三次？** 一个格子本身同时属于一个 Row、一个 Column、一个 Box。将同一个格子的 "1" 放入它对应的三个 Set 是完全正常的，不是重复违规。

**为什么先检查再 add？** 如果先将当前格子的 "1" 加入 Set，下一行检查就会发现当前格子自己的 "1"，产生虚假冲突。

## Part 5 — Hash Set 完整优化代码

```python
class Solution:
    def isValidSudoku(self, board: list[list[str]]) -> bool:
        rows = [set() for _ in range(9)]
        cols = [set() for _ in range(9)]
        boxes = {}

        for r in range(9):
            for c in range(9):
                value = board[r][c]

                if value == ".":
                    continue

                box = (r // 3, c // 3)

                if box not in boxes:
                    boxes[box] = set()

                if (
                    value in rows[r]
                    or value in cols[c]
                    or value in boxes[box]
                ):
                    return False

                rows[r].add(value)
                cols[c].add(value)
                boxes[box].add(value)

        return True
```

### 逐块解释

- `rows=[set() for _ in range(9)]` / `cols=...`：9 个互相独立的集合，各自记住对应的区域数字。
- `boxes={}`：Dictionary，Key 是 Box 的 Tuple 坐标，Value 是独立的 Set。
- `for r in range(9): for c in range(9)`：只进行一次 9×9 全棋盘扫描。
- `if value==".": continue`：跳过多个合法的空位。
- `box=(r//3,c//3)`：将当前位置映射到固定九宫格。
- `if box not in boxes: boxes[box]=set()`：按需初始化。
- 三项 `in` + `or`：任一约束已出现当前数字就 False。
- 三项 `add`：没有冲突时同步更新三种约束的状态。
- `return True` 必须在两个循环全部完成后才执行。

## Part 6 — 手动追踪 Example 2

关键两个冲突位置：

```python
board[0][0] = "1"
board[2][2] = "1"
```

对应的 `box` 都是：

```python
(0 // 3, 0 // 3)  # (0,0)
(2 // 3, 2 // 3)  # (0,0)
```

左上九宫格的处理顺序：

| 访问坐标 | value | box | box Set 访问前内容 | 本轮结果 |
| --- | --- | --- | --- | --- |
| (0,0) | "1" | (0,0) | {} | add "1" |
| (0,1) | "2" | (0,0) | {"1"} | add "2" |
| (1,0) | "4" | (0,0) | {"1","2"} | add "4" |
| (2,1) | "9" | (0,0) | {"1","2","4"} | add "9" |
| (2,2) | "1" | (0,0) | {"1","2","4","9"} | **重复，False** |

当遍历到 `board[2][2]`：

- Row 2 尚未有 "1"。
- Column 2 尚未有 "1"。
- **Box (0,0) 已经有 "1"**，所以第三个条件为 True。

不需要等这个 Box 的九个位置都扫完才返回，遇到重复立刻结束即可。

## Part 7 — Complexity

固定棋盘 9×9 = 81 格，因此：

| 约束规模 | Time | Auxiliary Space |
| --- | --- | --- |
| 固定 9×9 Sudoku | **O(1)** | **O(1)** |
| 推广成 N×N + Hash Set | **平均 O(N²)** | **O(N²)** |

固定棋盘时最多保存 9 个 Row Set、9 个 Column Set、9 个 Box Set，总大小有常数上限。

推广时要检查 N² 个格子，每个格子做固定次数 Hash Set 操作，**平均**每次为 O(1)。所有 Row/Column/Box 中保存的记录数是 O(N²)。

Hash Set 的 O(1) 是**平均**情况，不应无条件声称绝对最坏 O(1)。

对比初始 List 方案，在 N×N 推广场景里因为 List 的 `in` 可能是 O(N)，最坏整体可以达到 O(N³)。

## Part 8 — Another implementation: integer Box Index

熟悉 Tuple Key 之后还可用整数 0–8 标识九宫格：

```python
box_index = (r // 3) * 3 + (c // 3)
```

对应：

```text
0  1  2
3  4  5
6  7  8
```

可以直接使用三个 **List of Sets**，无需 Box Dictionary：

```python
class Solution:
    def isValidSudoku(self, board: list[list[str]]) -> bool:
        rows = [set() for _ in range(9)]
        cols = [set() for _ in range(9)]
        boxes = [set() for _ in range(9)]

        for r in range(9):
            for c in range(9):
                value = board[r][c]

                if value == ".":
                    continue

                box_index = (r // 3) * 3 + (c // 3)

                if (
                    value in rows[r]
                    or value in cols[c]
                    or value in boxes[box_index]
                ):
                    return False

                rows[r].add(value)
                cols[c].add(value)
                boxes[box_index].add(value)

        return True
```

两种方案本质相同。对初学者，**先掌握 Tuple Box Key**；整数映射作为可选精简写法，而不是同时强制背诵两个版本。

## Part 9 — English interview explanation

“I would use separate hash sets to track the digits already seen in each row, column, and three-by-three box.”

我用独立的 Hash Set 记录每行、每列和每个九宫格中出现过的数字。

“For each non-empty cell, I check whether its value exists in any of the three corresponding sets.”

对于每个非空格子，检查数字是否已存在于对应的三个集合之一。

“To identify a sub-box, I use integer division on the row and column indices.”

通过行列坐标整除 3 确定所属九宫格。

“If the value is repeated, I return false immediately; otherwise I add it to all three sets.”

如果发现任意重复，就立即返回 False；否则登记到三个集合。

“This validates the board in a single pass. Because the Sudoku board is fixed at nine by nine, time and space are both constant.”

这是一次遍历；由于棋盘固定 9×9，因此时间和空间为常数。

## Part 10 — Optimization comprehension quiz

**停下，先让学生答，不要提前公开答案。**

**Q1. 对于 `row=7, col=5`，其 Box Key 是什么？**

- A. (2,1)
- B. (1,2)
- C. (2,2)

**Q2. 为什么使用 `[set() for _ in range(9)]`，不能简单写 `[set()] * 9`？**

- A. Python 不允许后一种写法。
- B. 前者创建独立 Set，后者共享同一个 Set。
- C. Set 不能放入 List。

**Q3. `board[2][2]="1"`，且 `boxes[(0,0)]` 已经包含 "1"，为什么应当 False？**

- A. Sudoku 不能在任何位置出现相同数字。
- B. 当前数字在同一个 3×3 Box 中第二次出现。
- C. Column 2 一定重复。

**Q4. 为什么只扫描整个 Board 一次就可以同时检查 Row、Column 和 Box？**

用自己的话解释：对于任意一个非空格子，为什么只需访问三个对应 Set？

结束语示范：

> 这题真正需要掌握的不是单独的 `set()`，而是一个更广泛的模式：**每个元素同时属于多个约束范围，我们为每种约束维护独立的 Hash Set，在一次访问中检查并更新所有约束。** 答完这四题，我们再进入不看答案独立手写代码的阶段。

---

# CONVERSATION TURN 3 — User asks “整理到 Notion”

## Assistant — Save/report exemplar

这一步的重点是**实际保存**，而不是只口头说保存。

**Execution checklist:**

1. Read the connected Notion skill and the platform's Markdown/content rules.
2. Resolve the learner's established Roadmap parent page; check whether this problem's page already exists.
3. Save the complete **problem-specific** learning note, including both taught methods, full Python code, explanations of row/column/box, Box Mapping diagrams or text alternatives, single-pass check, manual trace, complexity, edge cases, interview wording, quiz and coding checklist.
4. Keep the saved note separate from proof of independent coding mastery.
5. Verify the write succeeded and, when supported, open the saved page alongside the conversation.

**Example reply ONLY IF tool confirms actual save:**

> **已成功保存并验证。**
>
> **Problem 08 — Valid Sudoku (36)**
>
> 已归档到既定 Arrays & Hashing Roadmap，内容包括：Brute Force、Hash Set 优化、二维数组基础、`(r//3,c//3)` 的推导、两种 Box Mapping、完整代码、复杂度、边界测试、英文面试表达和理解检查。点击[真实返回的 Notion 链接]打开笔记。
>
> 下一阶段继续独立 Python Live Coding。**Notion 已保存 ≠ 学生已经通过代码实测。**

If saving fails or lacks authorization, **do not say “已保存”**; provide a complete copy-ready draft and explain the limitation.

---

# OPTIONAL TURN 4 — Illustrative quiz evaluation (NOT actual learner history)

This part demonstrates **how future assistants should respond if the learner later answers the optimized quiz**. These are **fictional example answers**, not a report that the actual learner answered.

## Hypothetical learner answer

> Q1 B；Q2 B；Q3 B；Q4 因为一个数字只需要扫描一遍。

## Coach response example

你的 Q2、Q3 正确；Q1 错误；Q4 只有一部分正确。我们逐题看：

| 题目 | 回答 | 评价 |
| --- | --- | --- |
| Q1 | B | **错误** |
| Q2 | B | **正确** |
| Q3 | B | **正确** |
| Q4 | 只扫描一遍 | **部分正确** |

**Q1 — Box Mapping：**

`row=7, col=5`。计算 `7//3=2`，`5//3=1`，因此 Box Key 应是 `(2,1)`，正确选项 **A**。B 代表 `(1,2)`，对应的是不同区域。记住 Tuple 先 Row 分组，后 Column 分组。

**Q2 — 集合独立性：**

B 正确。`[set() for _ in range(9)]` 会调用 9 次 `set()`，从而创建 9 个不同对象。`[set()] * 9` 只创建一个 Set，再复制 9 次引用。

**Q3 — Box 内重复：**

B 正确。即使 Row 和 Column 不重复，仍可能在所属 Box 中找到之前出现的同一数字。

**Q4 — 为什么一次扫描就足够：**

你说“只扫描一遍”描述的是结果，但没有说明**为什么正确**。关键是：每个格子属于一个 Row、一个 Column 和一个 Box；我们为这三个区域分别保留已出现过的数字。访问当前非空格子时，立即对三个 Set 做检查并在无冲突时更新。这样后续格子自然会被前面保留的状态检测到。

**反馈原则：** 纠正具体错误，而不是只写“答案 A”；解释当前水平已掌握与尚未掌握的部分。若一题错误影响后续写代码，应给一个小追问确认，再开始 Live Coding。不要虚构“Quiz passed”。

---

# OPTIONAL TURN 5 — Illustrative independent Live Coding invitation (NOT actual learner history)

如果学生已经理解了 Hash Set，停止再次展示优化完整代码。只给空白骨架：

```python
class Solution:
    def isValidSudoku(self, board: list[list[str]]) -> bool:
        # Step 1: Create row, column, and box tracking structures

        # Step 2: Traverse board

        # Step 3: Skip blanks, compute box, check duplicates

        # Step 4: Record values and return the result
        pass
```

提供需要独立验证的场景：

1. 原题 Example 1：True。
2. 原题 Example 2：False，仅 Box 冲突。
3. 81 个 "."：True。
4. 相同 Row 两个 "5"：False。
5. 相同 Column 两个 "7"：False。
6. 不同行、不同列、不同 Box 的两个 "1"：应该**允许**。

告诉学生：

> 请你不看前面的代码独立写一遍，提交后我会重点检查：二维索引、Set 独立初始化、`(r//3,c//3)`、`"."` 跳过、先查重再添加、`return True` 的位置，以及实际时间/空间复杂度。如果发现错误，我先定位问题，不直接给标准答案。

---

# OPTIONAL TURN 6 — Illustrative interview-style review (NOT actual learner history)

## Hypothetical buggy submission

```python
class Solution:
    def isValidSudoku(self, board: list[list[str]]) -> bool:
        rows = [set()] * 9
        cols = [set()] * 9
        boxes = {}

        for r in range(9):
            for c in range(9):
                value = board[r][c]
                box = (r // 3, c // 3)
                if box not in boxes:
                    boxes[box] = set()
                if value in rows[r] or value in cols[c] or value in boxes[box]:
                    return False
                rows[r].add(value)
                cols[c].add(value)
                boxes[box].add(value)
        return True
```

## Coach response example: hint-first

你的整体算法方向正确：你使用双层循环遍历 Board，分别查对应的 Row、Column、Box，并在检测到重复时返回 False。

不过当前代码**还不能通过测试**，有两个独立的关键问题：

| 检查 | 状态 | 具体问题 |
| --- | --- | --- |
| Python 语法、签名 | 正确 | 当前缩进、签名、return 正常 |
| rows / cols 初始化 | 错误 | 9 个位置共享**同一个 Set** |
| Box Key | 正确 | `(r//3,c//3)` 映射正确 |
| 跳过空白 | 错误 | 没有跳过 "." |
| 检查与 add 顺序 | 正确 | 先检查再添加 |
| 复杂度 | 核心意图正确 | 9×9 固定为 O(1)/O(1)，但功能上需要先修复 Bug |

**Issue 1：为什么不同 Row/Column 会互相影响？**

定位到：

```python
rows = [set()] * 9
cols = [set()] * 9
```

提示：在 Python 中，重复列表项不代表每次都创建新 Set。设想 Row 0 加入 "1" 后，Row 1 的 Set 是否也能看到 "1"？请想想你之前学过的列表推导式。

**Issue 2：为什么空白 Board 会被判为 False？**

定位到：

```python
value = board[r][c]
box = (r // 3, c // 3)
```

提示：在执行三个重复检查之前，有一类不代表数字的字符不应该被记录。试着用全是 "." 的棋盘手动模拟前两个格子。

**你需要自行修复**。我不会直接给完整改好后的代码。提交下一版之后，我会复查两个问题，并给出具体的测试结果。

**Verification discipline:** This example is a *manual code review* of hypothetical code; do not claim that tests were executed unless a tool actually ran them.

---

# How to generalize this exemplar to other questions / 可复用的迁移方法

| Valid Sudoku example section | General replacement for the next problem |
| --- | --- |
| Rules and Example 2 3×3 conflict | Inputs, constraints, a minimal counterexample |
| Row/Col 2D indexing | Specific unfamiliar Python types, syntax and indices |
| Per-region List check | Correct simplest baseline |
| 9 Box starting coordinates | Diagram of actual state or pointer ranges |
| Single-pass Hash Sets | Optimized data structure and invariant |
| (r//3,c//3) mapping | Derive the problem-specific key or recurrence |
| Three-set state update | Explain update order and why it preserves correctness |
| Error-causing duplicates | Concrete edge cases and actual test expectations |
| Four-question quiz | 2–4 questions covering indices, invariants, and pitfalls |
| Independent coding scaffold | Skeleton only; no solution spoilers |
| Targeted code review | Actual user submission, failure reproduction, hint ladder |
| Opt-in Notion report | Tool-verified save with honest status |

**Fidelity rule:** Make each new lesson **as specific and detailed** as this example, but do not force every problem into Sudoku's three sets or use the same number of headings when inappropriate. If the learner explicitly wants one small clarification or short answer, respect that instead of reproducing a giant lesson. **The real teaching behavior is adaptive, staged, and user-controlled.**
