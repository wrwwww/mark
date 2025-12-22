# 1. 模拟

## 1.1. 两数之和

[两数之和](https://leetcode.cn/problems/two-sum/)

```
pub fn two_sum(nums: Vec<i32>, target: i32) -> Vec<i32> {
    let mut map = std::collections::HashMap::with_capacity(nums.len());

    for (idx, num) in nums.iter().enumerate() {
        let tmp = target - num;
        if let Some(v) = map.get(&tmp) {
            return vec![*v, idx as i32];
        }
        map.insert(*num, idx as i32);
    }
    panic!("not found")
}
```

## 1.2. 两数相加

[两数相加](https://leetcode-cn.com/problems/add-two-numbers/)

## 1.3. 最长回文子串

## 1.4. Z字形变换

## 1.5. 整数反转

## 1.6. 字符串转整数

## 1.7. 整数转罗马数字

[整数转罗马数字](https://leetcode.cn/problems/integer-to-roman/description/)

### 1.7.1. 模拟

```
// 跟着官方题解的模拟法
pub fn int_to_roman(mut num: i32) -> String {
    let nums = vec![1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1];
    let symbol = vec!["M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"];
    let mut res = String::default();
    for i in 0..nums.len() {
        while num >= nums[i] {
            num -= nums[i];
            res.push_str(symbol[i]);
        }
        if num == 0 {
            break;
        }
    }
    res
}
// 学习大佬的方法
pub fn int_to_roman(mut num: i32) -> String {
    let nums = vec![1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1];
    let symbol = vec!["M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"];
    let mut res = String::default();
    for i in 0..nums.len() {
    	// 去掉了对num>=nums[i]的判断, num<nums[i] ,num%nums[i]=num
        res.push_str(symbol[i].repeat((num / nums[i]) as usize).as_str());
        num %= nums[i];
        if num == 0 {
            break;
        }
    }
    res
}
```

### 1.7.2. 函数式

```
impl Solution {
    pub fn int_to_roman(num: i32) -> String {
        [(1000, "M"), (900, "CM"), (500, "D"), (400, "CD"),
        (100, "C"), (90, "XC"), (50, "L"), (40, "XL"),
        (10, "X"), (9, "IX"), (5, "V"), (4, "IV"),
        (1, "I")].into_iter()
        .fold((String::with_capacity(20), num), |(mut s, mut num), (base, unit)| {
            (s + &unit.repeat((num / base) as usize), num % base)
        }).0
    }
}
```

## 1.8. 罗马数字转整数

```
impl Solution {
    pub fn roman_to_int(s: String) -> i32 {
    let map = [
        (1000, 'M'),
        (500, 'D'),
        (100, 'C'),
        (50, 'L'),
        (10, 'X'),
        (5, 'V'),
        (1, 'I'),
    ];
    let mut pre_val = i32::MIN;
    let mut ans = 0;
    for c in s.chars().into_iter().rev() {
 
        let val = map.into_iter().find(|(_e1, e2)| e2.eq_ignore_ascii_case(&c)).unwrap();
        if val.0 < pre_val {
            ans-=val.0;
        }else {
            ans+=val.0;
        }
        pre_val=val.0;
    }
    return ans;
    }
}
```

```
pub fn roman_to_int(s: String) -> i32 {
    let map = [
        (1000, 'M'),
        (500, 'D'),
        (100, 'C'),
        (50, 'L'),
        (10, 'X'),
        (5, 'V'),
        (1, 'I'),
    ];
    let res = s
        .chars()
        .rev()
        .fold((0, i32::MIN, map), |(e1, pre_val, map), val| {
            let num = map
                .into_iter()
                .find(|(_e1, e2)| e2.eq_ignore_ascii_case(&val))
                .unwrap().0;
            if num < pre_val {
                (e1 - num, num, map)
            } else {
                (e1 + num, num, map)
            }
        }).0;
    return res;
}
```

## 1.9. 最长公共前缀

## 1.10. 下一个排列

```
pub fn next_permutation(nums: &mut Vec<i32>) {
    // 从后往前遍历,找到第一个开始下降的地方,并从最高数之后寻找比小数大的第一个数交换位置
    // 1231 下一个为 1312
    // 1321 -> 1312
    // 交换之后,大数的后面是递减序列,需要变为递增序列
    let mut idx = 0;
    for i in (0..nums.len() - 1).rev() {
        if nums[i] < nums[i + 1] {
             idx = i + 1;
            for j in (idx..nums.len()).rev() {
                if nums[j] > nums[i] {
                    nums.swap(i, j);
                    break;
                }
            }
            break;
        }
    }
    let mut right=nums.len()-1;
    while idx<right {
        nums.swap(idx, right);
        idx+=1;
        right-=1;
    }

}
```

## 1.11. 外观数列

```
impl Solution {
    pub fn count_and_say(n: i32) -> String {
        let mut ans = String::from("1");
        for _ in 1..n {
            ans = Solution::sub(ans.as_str());
        }
        ans
    }

    pub fn sub(input: &str) -> String {
        let (mut input, mut result) = (input, String::new());
        while input[..].len() > 0 {
            let flag: &char = &input.chars().nth(0).unwrap();
            let counter = input.chars().take_while(|ch| ch.eq(flag)).count();
            input = &input[counter..];
            result.push_str(&counter.to_string());
            result.push(flag.clone());
        }
        result
    }
}
```

```
pub fn count_and_say(n: i32) -> String {
    if n == 1 {
        return "1".to_string();
    }

    let mut res = String::from("1");
    for _ in 1..n {
        res = count(res);
    }
    return res;
}
pub fn count(num: String) -> String {
    let mut res = String::new();
    let nums = num.chars().collect::<Vec<char>>();
    let mut left = 0;
    let mut right = 1;
    while right < nums.len() {
        if nums[left] != nums[right] {
            res.push(char::from_u32(('0' as u32) + ((right - left) as u32)).unwrap());
            res.push(nums[left]);
            left = right;
        }
        right += 1;
    }
    res.push(char::from_u32(('0' as u32) + ((right - left) as u32)).unwrap());
    res.push(nums[left]);
    return res;
}
```

## 1.12. 最后一个单词的长度

[最后一个单词的长度](https://leetcode.cn/problems/length-of-last-word/)

```
pub fn length_of_last_word(s: String) -> i32 {
    let res=s.chars().into_iter().rev().enumerate().skip_while(|e|e.1==' ').take_while(|e|e.1!=' ').count();
    return res as i32;
}
```

```
pub fn length_of_last_word(s: String) -> i32 {
    let l = s.split_whitespace().last();
    if l.is_none() {
        return 0 as i32;
    } else {
        return l.unwrap().len() as i32;
    }
}
```

## 1.13. 加一

[https://leetcode.cn/problems/plus-one/description/](https://leetcode.cn/problems/plus-one/description/)

```
pub fn plus_one(digits: Vec<i32>) -> Vec<i32> {
    let mut digits=digits;
    for i in (0..digits.len()).rev() {
        digits[i]=(digits[i]+1)%10;
        if digits[i]!=0 {
            return digits;
        }
    }
    let mut res=vec![0;digits.len()+1];
    res[0]=1;
    res
}
```

## 1.14. 螺旋矩阵

[https://leetcode.cn/problems/spiral-matrix/](https://leetcode.cn/problems/spiral-matrix/)  

## 1.15. 格雷编码

```
0,1,3,2,6,7,5
0000
0001
0011
0010
0110
0111
```

## 1.16. 数字1的个数

[https://leetcode.cn/problems/number-of-digit-one/description/](https://leetcode.cn/problems/number-of-digit-one/description/)

```
impl Solution {
    pub fn count_digit_one(n: i32) -> i32 {
        let mut nn = n;
        let mut res = 0;
        let mut m = 0;
        let mut f = 1;
        while nn != 0 {
            m = nn % 10;
            nn = nn / 10;
            if m == 0 {
                res += nn * f;
            } else if m == 1 {
                res += nn * f + 1 + n % f;
            } else {
                res += (nn + 1) * f;
            }

            f *= 10;
        }
        return res;

    }
}
```

对于输入的数字n

分别统计每一位为1的数字的个数

不怕重复啥的,例如11 ,我只统计个位的时候无论十位的是1还是2都是用来统计各位上的1的

有三种情况

当前位上为0

23011

百位为0

则百位为1的数有 从

100~22199中

有 2300个1

其中百位1不动等于

00~2299一共2300个

百位为1

23111

100~23111

有0~2311

有2312个

百位为大于1的

23811

100~23199

23199为最大能表示的数

00~2399

2400

## 1.17. excel表名

28

28/26=1

28%26=2

其实就是进制的转换,就是将十进制的数字转为26进制,

26进制用ab表示

```
pub fn convert_to_title(column_number: i32) -> String {
    let mut n = column_number;
    let mut m = 0;
    let mut res = vec![];
    while n != 0 {
        m = (n-1) % 26;
        n = (n-1) / 26;

        res.push('A' as u8 + m as u8 );
    }

    res.reverse();
    String::from_utf8(res).unwrap()
}
```

## Dijkstra算法（迪杰斯特拉算法)
 
### 🔍 一、基础概念回顾

#### ✅ 邻接表（Adjacency List）

```text
graph[u] = [(v1, w1), (v2, w2), ...]
```

- 用于表示：从节点 `u` 出发的所有边。
    
- 实现：`Vec<Vec<(usize, i32)>>` 或 `HashMap<usize, Vec<(usize, i32)>>`
    

#### ✅ 邻接矩阵（Adjacency Matrix）

```text
graph[u][v] = w    // 从u到v的权值w（或1）
```

- 二维矩阵：`Vec<Vec<i32>>`，`graph[i][j]` 表示是否有边，以及边的权重。
    

---

### 🔍 二、稀疏图 vs 稠密图

|图类型|边数量级|特征|
|---|---|---|
|**稀疏图**|边数 ≪ n²|每个点的连接数较少|
|**稠密图**|边数 ≈ n²|大多数节点之间都有边|

---

### 🧠 三、选择使用哪个表示方式？

|比较项|邻接表（Adjacency List）|邻接矩阵（Adjacency Matrix）|
|---|---|---|
|**适合场景**|✅ 稀疏图|✅ 稠密图|
|**空间复杂度**|O(V + E)|O(V²)|
|**是否存在边 (u,v)**|O(degree(u)) 或 O(logN)（用set）|✅ O(1)|
|**遍历所有出边**|✅ 高效 O(degree(u))|❌ O(V)|
|**算法性能**|适合 Dijkstra (用堆)|Floyd-Warshall（多源最短路径）更好|

---

### 🔨 四、实际应用举例

#### ✅ 稀疏图（邻接表更好）：

- 地图路网、社交网络（大多数人不是朋友）
    
- **Dijkstra + Heap**：邻接表更高效
    

#### ✅ 稠密图（邻接矩阵更好）：

- 全互联图、完全图、Floyd 算法
    
- 图中几乎所有点对都有边
    

---

### 🧾 总结一句话：

> **“图稀疏用邻接表，图稠密用邻接矩阵”**。

因为：

- 邻接表节省空间，遍历快；
    
- 邻接矩阵查边快，适合 Floyd 等 O(n³) 算法。
    

---

对，你理解得非常到位！我们可以更形象地总结一下你的理解：

---

### ✅ 邻接矩阵适合稠密图的两个核心原因：

#### 1️⃣ 空间利用率高

- 在**稠密图**中，几乎所有顶点之间都有边。
    
- 使用邻接矩阵（二维数组）不会浪费空间，大部分位置都存有有效值（边权或1）。
    
- ✅ 空间利用率高，✖ 不会造成大量 `0` 或 `INF` 占用内存。
    

#### 2️⃣ 查边效率高

- 邻接矩阵 `graph[u][v]` 是 **数组 O(1)** 访问，适合大量查边操作。
    
- 稠密图中查边频繁，用邻接矩阵效率更高。
    

---

### 🆚 举个对比例子：

假设有 `n = 1000` 个节点：

| 图类型 | 边数量 E         | 邻接表大小            | 邻接矩阵大小             |
| --- | ------------- | ---------------- | ------------------ |
| 稀疏图 | E ≈ 3,000     | O(n + E) ≈ 4,000 | 1,000,000 个格子，浪费严重 |
| 稠密图 | E ≈ 1,000,000 | 邻接表也大            | 每个格子几乎都用到了，邻接矩阵划算  |

---

### 🧠 小结

> ✅ 稠密图：**边多、查边多**，就用邻接矩阵。  
> ✅ 稀疏图：**边少、遍历邻边多**，就用邻接表。

---

### [743. 网络延迟时间](https://leetcode.cn/problems/network-delay-time/)

####  描述

有 `n` 个网络节点，标记为 `1` 到 `n`。
给你一个列表 `times`，表示信号经过 **有向** 边的传递时间。 `times[i] = (ui, vi, wi)`，其中 `ui` 是源节点，`vi` 是目标节点， `wi` 是一个信号从源节点传递到目标节点的时间。
现在，从某个节点 `K` 发出一个信号。需要多久才能使所有节点都收到信号？如果不能使所有节点收到信号，返回 `-1` 。

#### dijkstra

```rust
pub fn network_delay_time (times: Vec<Vec<i32>>, n: i32, k: i32) -> i32 {
    // 构建邻接表
    let mut graph = vec![vec![]; n as usize + 1];
    for time in times {
        let u = time[0] as usize;
        let v = time[1] as usize;
        let w = time[2];
        // 有向图的邻接表 u是源节点，从1-n表示节点，创建graph长度为n+1这样就不用下标减一了
        graph[u].push((v, w));
    }
    // 构建记录表，记录到达某节点得最短权值
    let mut dist = vec![i32::MAX; n as usize + 1];
    // 设置起点k的最短权值为0
    dist[k as usize] = 0;
    // 构建堆 rust标准库中的堆是大顶堆，在使用的时候我们可以将排序数加上负号这样他就能按最小堆的顺序pop出来
    // 或者使用std::cmp::Reverse()
    let mut heap = std::collections::BinaryHeap::new();
    // 把起点k push到堆中(w，u)，注意这里权值其实是添加负号的，只不过0的负数还是零，对于元组的排序，默认取元组第一个元素
    heap.push((0, k as usize));
    while let Some((w, u)) = heap.pop() {
        // 因为传入的时候我们传的负值，这里我们将其正确值取出来
        let w = -w;
        if w > dist[u as usize] {
            continue;
        }
        // 遍历邻接点
        for i in 0..graph[u as usize].len() {
            let nw = graph[u as usize][i].1 + w;
            let nu = graph[u as usize][i].0;
            if nw < dist[nu] {
                dist[nu] = nw;
                heap.push((-nw, nu));
            }
        }
    }
    // 跳过第一个然后查看最大值，如果最大值为i32::MAX则说明有没到达的节点
    dist.iter()
        .skip(1)
        .max()
        .map_or(-1, |&max| if max == i32::MAX { -1 } else { max })
}

```

### [1631. 最小体力消耗路径](https://leetcode.cn/problems/path-with-minimum-effort/)
#### 描述

你准备参加一场远足活动。给你一个二维 `rows x columns` 的地图 `heights` ，其中 `heights[row][col]` 表示格子 `(row, col)` 的高度。一开始你在最左上角的格子 `(0, 0)` ，且你希望去最右下角的格子 `(rows-1, columns-1)` （注意下标从 **0** 开始编号）。你每次可以往 **上**，**下**，**左**，**右** 四个方向之一移动，你想要找到耗费 **体力** 最小的一条路径。
一条路径耗费的 **体力值** 是路径上相邻格子之间 **高度差绝对值** 的 **最大值** 决定的。
请你返回从左上角走到右下角的最小 **体力消耗值** 。

> 最短路径 dijkstra算法
#### dijkstra
```rust
pub fn minimum_effort_path_dijkstra(heights: Vec<Vec<i32>>) -> i32 {
    const DIRECTIONS: [(i32, i32); 4] = [(1, 0), ((-1, 0)), (0, 1), (0, -1)];
    let mut dist = vec![vec![i32::MAX; heights[0].len()]; heights.len()];
    dist[0][0] = 0;
    let mut heap = BinaryHeap::new();
    heap.push(Reverse((0, 0, 0))); // (体力消耗, x, y)
    while let Some(Reverse((effort, x, y))) = heap.pop() {
        for (dx, dy) in DIRECTIONS
            .iter()
            .map(|(dx, dy)| (x + dx, dy + y))
            .filter(|(dx, dy)| {
                *dx >= 0 && *dx < heights.len() as i32 && *dy >= 0 && *dy < heights[0].len() as i32
            })
        {
            if x == (heights.len() - 1) as i32 && y == (heights[0].len() - 1) as i32 {
                return effort;
            }
            let new_effort =
                (heights[dx as usize][dy as usize] - heights[x as usize][y as usize]).abs();
            let min_effort = effort.max(new_effort);
            if dist[dx as usize][dy as usize] > min_effort {
                dist[dx as usize][dy as usize] = min_effort;
                heap.push(Reverse((min_effort, dx, dy)));
            }
        }
    }
    0 // 如果没有路径,返回0
}
```
#### 二分查找
```rust
pub fn minimum_effort_path(heights: Vec<Vec<i32>>) -> i32 {
    const DIRECTIONS: [(i32, i32); 4] = [(1, 0), ((-1, 0)), (0, 1), (0, -1)];
    // 先写个二分查找版本
    let mut left = 0;
    let mut right = 1_000_000;
    while left < right {
        let mid = left + (right - left) / 2;
        // 开始查找
        let mut visited = vec![vec![false; heights[0].len()]; heights.len()];
        let mut queue = std::collections::VecDeque::<(i32, i32)>::new();
        queue.push_back((0, 0)); // 从左上角开始
        visited[0][0] = true;
        while let Some((x, y)) = queue.pop_front() {
            for (dx, dy) in &DIRECTIONS {
                let nx = x + dx;
                let ny = y + dy;
                // 排除不在范围内的,并且权重不超过mid
                let len = heights.len() as i32;
                let len2 = heights[0].len() as i32;
                if nx >= 0
                    && nx < len
                    && ny >= 0
                    && ny < len2
                    && (heights[nx as usize][ny as usize] - heights[x as usize][y as usize]).abs()
                        <= mid
                    && !visited[nx as usize][ny as usize]
                {
                    visited[nx as usize][ny as usize] = true;
                    queue.push_back((nx, ny));
                }
            }
        }
        // 如果右下角被访问过了,说明可以到达
        if visited[heights.len() - 1][heights[0].len() - 1] {
            right = mid; // 可以尝试更小的体力消耗
        } else {
            left = mid + 1; // 需要更大的体力消耗
        }
    }
    left // 最小体力消耗
}
```

### [778. 水位上升的泳池中游泳](https://leetcode.cn/problems/swim-in-rising-water/)

#### 简介
在一个 `n x n` 的整数矩阵 `grid` 中，每一个方格的值 `grid[i][j]` 表示位置 `(i, j)` 的平台高度。

当开始下雨时，在时间为 `t` 时，水池中的水位为 `t` 。你可以从一个平台游向四周相邻的任意一个平台，但是前提是此时水位必须同时淹没这两个平台。假定你可以瞬间移动无限距离，也就是默认在方格内部游动是不耗时的。当然，在你游泳的时候你必须待在坐标方格里面。

你从坐标方格的左上平台 `(0，0)` 出发。返回 _你到达坐标方格的右下平台 `(n-1, n-1)` 所需的最少时间 。_

#### 二分查找
```rust
pub fn swim_in_water_bin_search(grid: Vec<Vec<i32>>) -> i32 {
    let fx: Vec<(i32, i32)> = vec![(1, 0), (-1, 0), (0, 1), (0, -1)];
    let mut left = grid[0][0].max(grid[grid.len() - 1][grid[0].len() - 1]);
    let mut right = 50 * 50;
    while left < right {
        let mid = left + (right - left) / 2;
        // 开始查找
        let mut visited = vec![vec![false; grid[0].len()]; grid.len()];
        let mut queue = std::collections::VecDeque::<(i32, i32)>::new();
        queue.push_back((0, 0)); // 从左上角开始
        visited[0][0] = true;
        while let Some((x, y)) = queue.pop_front() {
            if x == (grid.len() - 1) as i32 && y == (grid[0].len() - 1) as i32 {
                break; // 到达右下角
            }
            for (dx, dy) in fx.iter() {
                let nx = x + dx;
                let ny = y + dy;
                // 排除不在范围内的,并且权重不超过mid
                let len = grid.len() as i32;
                let len2 = grid[0].len() as i32;
                if nx >= 0
                    && nx < len
                    && ny >= 0
                    && ny < len2
                    && grid[nx as usize][ny as usize] <= mid
                    && !visited[nx as usize][ny as usize]
                {
                    visited[nx as usize][ny as usize] = true;
                    queue.push_back((nx, ny));
                }
            }
        }
        if visited[grid.len() - 1][grid[0].len() - 1] {
            right = mid; // 可以尝试更小的体力消耗
        } else {
            left = mid + 1; // 需要更大的体力消耗
        }
    }
    left
}
```

#### dijkstra算法
```rust
pub fn swim_in_water_dijkstra(grid: Vec<Vec<i32>>) -> i32 {
    // directions
    const DIRECTIONS: [(i32, i32); 4] = [(1, 0), (-1, 0), (0, 1), (0, -1)];
    let mut binary_heap = std::collections::BinaryHeap::<std::cmp::Reverse<(i32, i32, i32)>>::new();
    let mut dict = vec![vec![i32::MAX; grid[0].len()]; grid.len()];
    // 初始位置
    binary_heap.push(Reverse((grid[0][0], 0, 0))); // (体力消耗, x, y)
    while let Some(Reverse((effort, x, y))) = binary_heap.pop() {
        if x == (grid.len() - 1) as i32 && y == (grid[0].len() - 1) as i32 {
            return effort; // 到达右下角
        }
        for (dx, dy) in DIRECTIONS.iter() {
            let nx = x + dx;
            let ny = y + dy;
            // 检查边界
            if nx >= 0 && nx < grid.len() as i32 && ny >= 0 && ny < grid[0].len() as i32 {
                let new_effort = effort.max(grid[nx as usize][ny as usize]);
                if dict[nx as usize][ny as usize] > new_effort {
                    binary_heap.push(Reverse((new_effort, nx, ny)));
                    dict[nx as usize][ny as usize] = new_effort; // 更新最小体力消耗
                }
            }
        }
    }
    0
}
```

### [1976. 到达目的地的方案数](https://leetcode.cn/problems/number-of-ways-to-arrive-at-destination/)
#### 介绍
你在一个城市里，城市由 `n` 个路口组成，路口编号为 `0` 到 `n - 1` ，某些路口之间有 **双向** 道路。输入保证你可以从任意路口出发到达其他任意路口，且任意两个路口之间最多有一条路。

给你一个整数 `n` 和二维整数数组 `roads` ，其中 `roads[i] = [ui, vi, timei]` 表示在路口 `ui` 和 `vi` 之间有一条需要花费 `timei` 时间才能通过的道路。你想知道花费 **最少时间** 从路口 `0` 出发到达路口 `n - 1` 的方案数。

请返回花费 **最少时间** 到达目的地的 **路径数目** 。由于答案可能很大，将结果对 `109 + 7` **取余** 后返回。

#### dijkstra算法
```rust
    pub fn count_paths(n: i32, roads: Vec<Vec<i32>>) -> i32 {
            const MOD: i64 = 1_000_000_007;
    // 构建邻接表
    let mut table = vec![vec![]; n as usize];
    for item in roads.iter() {
        let u = item[0] as usize;
        let v = item[1] as usize;
        let w = item[2] as i64;
        table[u].push((w, v));
        table[v].push((w, u));
    }
    let mut dist = vec![i64::MAX; n as usize];
    let mut ways = vec![0i64; n as usize];
    ways[0] = 1;
    dist[0] = 0;
    // 小顶堆
    let mut heap = std::collections::BinaryHeap::new();
    heap.push((0, 0));

    while let Some((w, u)) = heap.pop() {
        let w = -w;
        if w > dist[u] {
            continue;
        }
        for i in table[u as usize].iter() {
            if w + i.0 < dist[i.1] {
                dist[i.1] = w + i.0;
                heap.push((-(w + i.0), i.1));
                ways[i.1] = ways[u];
            } else if w + i.0 == dist[i.1] {
                ways[i.1] = (ways[i.1] + ways[u]) % MOD;
            }
        }
    }
    ways[n as usize - 1] as i32
    }
```

### [1514. 概率最大的路径](https://leetcode.cn/problems/path-with-maximum-probability/)

#### 介绍
给你一个由 `n` 个节点（下标从 0 开始）组成的无向加权图，该图由一个描述边的列表组成，其中 `edges[i] = [a, b]` 表示连接节点 a 和 b 的一条无向边，且该边遍历成功的概率为 `succProb[i]` 。

指定两个节点分别作为起点 `start` 和终点 `end` ，请你找出从起点到终点成功概率最大的路径，并返回其成功概率。

如果不存在从 `start` 到 `end` 的路径，请 **返回 0** 。只要答案与标准答案的误差不超过 **1e-5** ，就会被视作正确答案。
> 需要注意的是rust中f64类型只实现了不安全的比较，而BinaryHeap中的比较需要实现Ord trait，我们可以通过自定义数据类型，然后通过保证不安全的比较的安全，来实现Ord
#### dijkstra
```rust
use std::{

    cmp::Reverse,

    collections::{binary_heap, BinaryHeap},

    fmt::Binary,

    vec,

};

impl Solution {

 pub fn max_probability(

    n: i32,

    edges: Vec<Vec<i32>>,

    succ_prob: Vec<f64>,

    start_node: i32,

    end_node: i32,

) -> f64 {

    // 构建邻接表

    let mut graph = vec![vec![]; n as usize];

    for (i, edge) in edges.iter().enumerate() {

        let u = edge[0] as usize; // 起点

        let v = edge[1] as usize; // 终点

        let p = succ_prob[i]; // 概率

        graph[u].push((v, p));

        graph[v].push((u, p)); // 无向图

    }

    let mut dist = vec![0.0; n as usize];

    dist[start_node as usize] = 1.0; // 起点的概率为1

    let mut heap = BinaryHeap::new();

    heap.push((MyF64(1.0), start_node as usize)); // (概率, 节点)

                                                  //

    while let Some((MyF64(prob), u)) = heap.pop() {

        if u == end_node as usize {

            return prob;

        }

        for (v, w) in graph[u].iter() {

            if prob * w > dist[*v] {

                dist[*v] = prob * w;

                heap.push((MyF64(dist[*v]), *v));

            }

        }

    }

  

    0.0

}

  
  

}#[derive(Clone, Copy, PartialEq, PartialOrd)]

struct MyF64(f64);

  

impl Eq for MyF64 {}

  

impl Ord for MyF64 {

    fn cmp(&self, other: &Self) -> std::cmp::Ordering {

        self.partial_cmp(other).unwrap()

    }

}
```

### [787. K 站中转内最便宜的航班](https://leetcode.cn/problems/cheapest-flights-within-k-stops/)

#### 描述
有 `n` 个城市通过一些航班连接。给你一个数组 `flights` ，其中 `flights[i] = [fromi, toi, pricei]` ，表示该航班都从城市 `fromi` 开始，以价格 `pricei` 抵达 `toi`。

现在给定所有的城市和航班，以及出发城市 `src` 和目的地 `dst`，你的任务是找到出一条最多经过 `k` 站中转的路线，使得从 `src` 到 `dst` 的 **价格最便宜** ，并返回该价格。 如果不存在这样的路线，则输出 `-1`。
```rust
// 创建邻接表
    let mut graph = vec![vec![]; n as usize];
    for flight in flights {
        let from = flight[0] as usize;
        let to = flight[1] as usize;
        let price = flight[2];
        graph[from].push((to, price));
    }
    // 这里维护一个二维数组，其中分别记录到达这个点花费多少
    let mut dist = vec![vec![i32::MAX; k as usize + 2]; n as usize];
    for i in 0..k as usize + 2 {
        dist[src as usize][i] = 0;
    }
    let mut binary_heap = std::collections::BinaryHeap::new();
    binary_heap.push((0, src as usize, 0));
    while let Some((price, v, count)) = binary_heap.pop() {
        if v == dst as usize {
            return -price; // 这里的price是负数，所以返回时取负值
        }
        if count > k {
            continue;
        }

        let price = -price;
        for (nv, nprice) in &graph[v] {
            let tmp = nprice + price;
            let count = count + 1;
            if tmp < dist[*nv][count as usize] {
                dist[*nv][count as usize] = tmp;
                binary_heap.push((-tmp, *nv, count));
            }
        }
    }
    // 如果没有到达目的地，返回-1
    -1
```