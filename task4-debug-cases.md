<!--
==============================================
项目：Flight Simulator（飞行模拟器）
开发者：Risen
文件：task4-debug-cases.md
功能：任务4向量学习的调试案例集
创建日期：2025-12-13
==============================================
-->

# 📚 任务4：向量学习调试案例集

## 🎯 学习目标

通过这些调试案例，你将：
1. 深入理解向量的坐标表示
2. 掌握向量加法和减法的几何意义
3. 理解向量长度和归一化
4. 学会使用线性插值（Lerp）
5. 理解点积的实际应用

---

## 📋 案例使用说明

每个案例包含：
- **学习目标**：这个案例要学什么
- **修改位置**：找到 index.html 的哪一行代码
- **修改内容**：具体改成什么
- **预期效果**：应该看到什么变化
- **思考问题**：帮助理解的问题
- **答案解析**：验证你的理解

**建议：**
1. 每次只做一个案例
2. 修改后立即刷新浏览器查看效果
3. 先思考问题，再看答案
4. 完成一个案例后，改回原代码再做下一个

---

## 🟢 案例1：改变绿球位置（理解向量坐标）

### 学习目标
理解向量的三个分量（x, y, z）如何影响物体位置

### 修改位置
找到这行代码（大约在第527行）：
```javascript
sphereB.position = new BABYLON.Vector3(-2, 3, 2);
```

### 实验A：只改变X坐标
**修改为：**
```javascript
sphereB.position = new BABYLON.Vector3(5, 3, 2);  // X: -2 → 5
```

**预期效果：**
- 绿球向右移动
- 蓝球也会跟着向右移动（因为蓝球 = 红球 + 绿球）
- "A到B距离"会变化

**思考问题：**
1. X从-2变成5，绿球向哪个方向移动？移动了多少？
2. 为什么蓝球也移动了？
3. 距离变大了还是变小了？

<details>
<summary>点击查看答案</summary>

1. 向右移动了7个单位（5 - (-2) = 7）
   - X轴：负值=左，正值=右

2. 因为蓝球 = A + B
   - A不变，B向右移动7个单位
   - 所以C（蓝球）也向右移动7个单位

3. 取决于红球的位置
   - 如果红球原本在绿球左侧，距离会变大
   - 如果红球原本在绿球右侧，距离会变小
</details>

---

### 实验B：让绿球到地面
**修改为：**
```javascript
sphereB.position = new BABYLON.Vector3(-2, 0.5, 2);  // Y: 3 → 0.5
```

**预期效果：**
- 绿球降落到地面附近
- 蓝球的高度也降低
- 绿球的"向量长度"变短

**思考问题：**
1. 为什么Y=0.5而不是Y=0？
2. 向量长度为什么变短了？
3. 蓝球降低了多少高度？

<details>
<summary>点击查看答案</summary>

1. Y=0.5是因为球体半径约0.5，让它刚好在地面上
   - 如果Y=0，球体会有一半在地下

2. 向量长度 = √(x² + y² + z²)
   - Y从3变成0.5，整体长度变短
   - (-2)² + 0.5² + 2² = 8.25，√8.25 ≈ 2.87
   - 原来：(-2)² + 3² + 2² = 17，√17 ≈ 4.12

3. 降低了 2.5 个单位（3 - 0.5 = 2.5）
</details>

---

### 实验C：让绿球到原点
**修改为：**
```javascript
sphereB.position = new BABYLON.Vector3(0, 0, 0);  // 原点
```

**预期效果：**
- 绿球在原点（0,0,0）
- 绿球半埋在地下（因为Y=0）
- 蓝球的位置 = 红球的位置
- "B长度" = 0

**思考问题：**
1. 为什么蓝球和红球重合了？
2. 向量(0,0,0)的长度是多少？
3. 这说明向量加法的什么性质？

<details>
<summary>点击查看答案</summary>

1. 因为 A + (0,0,0) = A
   - 加上零向量等于自己

2. 长度是0
   - √(0² + 0² + 0²) = 0

3. 零向量是加法的"单位元"
   - 任何向量加上零向量等于自己
   - 就像数字加0一样
</details>

---

## 🔴 案例2：改变红球运动轨迹（理解向量运算）

### 学习目标
理解如何通过改变向量分量来控制运动轨迹

### 修改位置
找到这段代码（大约在第643-645行）：
```javascript
sphereA.position.x = Math.sin(time * 0.5) * 3;
sphereA.position.y = 2 + Math.cos(time * 0.5);
sphereA.position.z = 0;
```

### 实验A：让红球只左右移动
**修改为：**
```javascript
sphereA.position.x = Math.sin(time * 0.5) * 3;
sphereA.position.y = 3;  // 固定高度
sphereA.position.z = 0;
```

**预期效果：**
- 红球在固定高度左右移动
- 蓝球也在固定高度左右移动
- 运动轨迹变成水平直线

**思考问题：**
1. 为什么红球不再上下移动了？
2. 蓝球的Y坐标会是多少？
3. 如果想让红球上下移动，应该改哪个轴？

<details>
<summary>点击查看答案</summary>

1. 因为Y被固定为常数3，不再是时间的函数

2. 红球Y=3，绿球Y=3，所以蓝球Y = 3+3 = 6

3. 应该改Y轴，让Y = f(time)
   - 例如：`sphereA.position.y = 3 + Math.sin(time * 0.5);`
</details>

---

### 实验B：画圆形轨迹
**修改为：**
```javascript
sphereA.position.x = Math.sin(time * 0.5) * 3;
sphereA.position.y = 3;  // 固定高度
sphereA.position.z = Math.cos(time * 0.5) * 3;  // 改变Z轴
```

**预期效果：**
- 红球在水平面上画圆
- 从上方俯视最明显
- 蓝球也画圆

**思考问题：**
1. 为什么用sin和cos可以画圆？
2. 如果把Z轴的系数改成5（而X轴还是3），会画出什么形状？
3. 如果把 `time * 0.5` 改成 `time * 2`，圆的大小会变吗？速度呢？

<details>
<summary>点击查看答案</summary>

1. 因为 sin²(t) + cos²(t) = 1
   - (x,z) = (3sin(t), 3cos(t)) 描述半径为3的圆

2. 会画出椭圆
   - 长轴5，短轴3

3. 圆的大小不变（半径还是3），但速度变快
   - time * 0.5：慢速
   - time * 2：快4倍
</details>

---

### 实验C：螺旋上升
**修改为：**
```javascript
sphereA.position.x = Math.sin(time * 0.5) * 3;
sphereA.position.y = 2 + time * 0.1;  // 持续上升
sphereA.position.z = Math.cos(time * 0.5) * 3;
```

**预期效果：**
- 红球沿螺旋路径上升
- 最终会飞出视野
- 蓝球也螺旋上升

**思考问题：**
1. 为什么红球会不停上升？
2. 如果想让红球下降，应该改成什么？
3. 如何让螺旋的半径逐渐变大？

<details>
<summary>点击查看答案</summary>

1. 因为Y = 2 + time * 0.1
   - time持续增长，Y就持续增长

2. 改成 `sphereA.position.y = 2 - time * 0.1;`
   - 减号表示下降

3. 让XZ的系数也随时间增长
   ```javascript
   const radius = 3 + time * 0.05;
   sphereA.position.x = Math.sin(time * 0.5) * radius;
   sphereA.position.z = Math.cos(time * 0.5) * radius;
   ```
</details>

---

## ⬛ 案例3：改变立方体运动（理解Lerp插值）

### 学习目标
理解线性插值（Lerp）如何控制平滑运动

### 修改位置
找到这段代码（大约在第684-692行）：
```javascript
const t = (Math.sin(time) + 1) / 2;

box.position = BABYLON.Vector3.Lerp(
    sphereB.position,   // 起点
    sphereA.position,   // 终点
    t                   // 插值参数
);
```

### 实验A：让立方体停在中点
**修改为：**
```javascript
const t = 0.5;  // 固定为0.5（中点）

box.position = BABYLON.Vector3.Lerp(
    sphereB.position,
    sphereA.position,
    t
);
```

**预期效果：**
- 立方体停在红球和绿球的中点
- 不再来回移动
- 红球移动时，立方体会跟着调整位置（保持在中点）

**思考问题：**
1. t=0.5时，立方体的位置公式是什么？
2. 如果t=0，立方体在哪里？
3. 如果t=1，立方体在哪里？

<details>
<summary>点击查看答案</summary>

1. 立方体位置 = (A + B) / 2
   - Lerp(A, B, 0.5) = A + (B-A) * 0.5 = A + 0.5B - 0.5A = 0.5A + 0.5B

2. t=0：立方体在起点（绿球位置）
   - Lerp(A, B, 0) = A

3. t=1：立方体在终点（红球位置）
   - Lerp(A, B, 1) = B
</details>

---

### 实验B：改变移动速度
**修改为：**
```javascript
const t = (Math.sin(time * 2) + 1) / 2;  // time * 2，变快
```

**预期效果：**
- 立方体来回移动速度变快（2倍）
- 其他不变

**思考问题：**
1. 为什么乘以2会变快？
2. 如果想慢下来，应该乘以多少？
3. 如果改成 `time * 5`，会发生什么？

<details>
<summary>点击查看答案</summary>

1. 因为sin函数的周期变短了
   - sin(time * 2) 的周期是 π（原来是2π）
   - 周期减半 = 速度翻倍

2. 乘以小于1的数，比如0.5
   - `time * 0.5` 会变慢一倍

3. 会非常快地来回移动
   - 可能看起来像在"颤抖"
</details>

---

### 实验C：偏向绿球
**修改为：**
```javascript
const t = (Math.sin(time) + 1) / 2 * 0.3;  // 乘以0.3，限制范围
```

**预期效果：**
- 立方体只在前30%的路径上移动
- 大部分时间靠近绿球
- 不会到达红球

**思考问题：**
1. 现在t的范围是多少？
2. 如果想让立方体只在70%-100%之间移动（靠近红球），应该怎么改？
3. 如何让立方体在20%-80%之间移动（排除两端）？

<details>
<summary>点击查看答案</summary>

1. t的范围是 0 到 0.3
   - (sin(time) + 1) / 2 范围是 0~1
   - 乘以0.3后范围是 0~0.3

2. 改成：`const t = (Math.sin(time) + 1) / 2 * 0.3 + 0.7;`
   - 基础值0.7，加上0~0.3的变化
   - 范围：0.7 ~ 1.0

3. 改成：`const t = (Math.sin(time) + 1) / 2 * 0.6 + 0.2;`
   - 基础值0.2，加上0~0.6的变化
   - 范围：0.2 ~ 0.8
</details>

---

## 🎯 案例4：添加第四个球（练习向量减法）

### 学习目标
理解向量减法的几何意义（方向向量）

### 修改位置
在 `sphereC` 创建代码后面（大约第550行之后），添加以下代码：

### 实验A：创建黄色球（显示A-B）
**添加代码：**
```javascript
// 黄色球体：代表向量A-B的结果（向量减法）
const sphereD = BABYLON.MeshBuilder.CreateSphere(
    "sphereD",
    { diameter: 1 },
    scene
);

const materialD = new BABYLON.StandardMaterial("matD", scene);
materialD.diffuseColor = new BABYLON.Color3(1, 1, 0);  // 黄色
materialD.emissiveColor = new BABYLON.Color3(0.3, 0.3, 0);
sphereD.material = materialD;

// 向量减法：A - B
sphereD.position = sphereA.position.subtract(sphereB.position);
```

然后在渲染循环中（大约第652行后面），添加：
```javascript
// 更新黄球位置
sphereD.position = sphereA.position.subtract(sphereB.position);
```

**预期效果：**
- 出现一个黄色球体
- 黄球位置 = 红球 - 绿球
- 黄球会跟随红球移动

**思考问题：**
1. 如果红球在(3,2,0)，绿球在(-2,3,2)，黄球在哪里？
2. 向量减法 A-B 的几何意义是什么？
3. 黄球、红球、绿球三者有什么几何关系？

<details>
<summary>点击查看答案</summary>

1. 黄球在 (5, -1, -2)
   - (3,2,0) - (-2,3,2) = (3-(-2), 2-3, 0-2) = (5,-1,-2)

2. A-B 表示从B指向A的向量
   - 起点在B，终点在A
   - 方向：从绿球指向红球

3. 如果把黄球移到绿球的位置，它会指向红球
   - B + (A-B) = A
   - 绿球 + 黄球向量 = 红球
</details>

---

### 实验B：显示方向向量
**修改黄球的位置计算：**
```javascript
// 计算方向向量（归一化的A-B）
const direction = sphereA.position.subtract(sphereB.position).normalize();

// 让黄球沿着方向移动，距离绿球2个单位
sphereD.position = sphereB.position.add(direction.scale(2));
```

**预期效果：**
- 黄球在绿球和红球之间
- 距离绿球2个单位
- 方向始终从绿球指向红球

**思考问题：**
1. normalize() 做了什么？
2. scale(2) 是什么意思？
3. 如何让黄球距离绿球5个单位？

<details>
<summary>点击查看答案</summary>

1. normalize() 把向量长度变为1，保持方向
   - 例如 (3,4,0) 长度是5
   - 归一化后是 (0.6, 0.8, 0)，长度是1

2. scale(2) 把向量各分量乘以2
   - 长度变为2倍
   - (1,0,0).scale(2) = (2,0,0)

3. 改成 direction.scale(5)
</details>

---

## 📐 案例5：实验点积（理解向量夹角）

### 学习目标
理解点积如何反映两个向量的夹角关系

### 修改位置
在信息显示代码中（大约第740行），已经显示了点积值。
我们通过移动球体来观察点积的变化。

### 实验A：让两球平行
**修改绿球位置：**
```javascript
sphereB.position = new BABYLON.Vector3(-6, 6, 0);
```

**观察：**
- 红球在 (sin*3, 2+cos, 0) 附近
- 绿球在 (-6, 6, 0)
- 观察点积值的正负

**思考问题：**
1. 什么时候点积最大？
2. 什么时候点积最小？
3. 点积为0时，两向量是什么关系？

<details>
<summary>点击查看答案</summary>

1. 当两向量方向相同时，点积最大
   - 都指向第一象限

2. 当两向量方向相反时，点积最小（负值最大）
   - 一个向右上，一个向左下

3. 点积为0时，两向量垂直（90度）
   - A · B = |A||B|cos(θ)
   - cos(90°) = 0
</details>

---

### 实验B：让两球垂直
**修改绿球位置：**
```javascript
sphereB.position = new BABYLON.Vector3(0, 0, 3);  // 纯Z方向
```

**修改红球运动：**
```javascript
sphereA.position.x = Math.sin(time * 0.5) * 3;
sphereA.position.y = Math.cos(time * 0.5) * 3;
sphereA.position.z = 0;  // XY平面
```

**预期效果：**
- 红球在XY平面画圆
- 绿球在Z轴上
- 点积始终接近0（因为垂直）

**思考问题：**
1. 为什么点积接近0？
2. 如果绿球在(3,0,0)，红球在(0,3,0)，点积是多少？
3. 点积可以判断什么？

<details>
<summary>点击查看答案</summary>

1. 因为红球在XY平面，绿球沿Z轴
   - (x,y,0) · (0,0,z) = 0
   - 互相垂直

2. 点积是0
   - (3,0,0) · (0,3,0) = 3*0 + 0*3 + 0*0 = 0

3. 可以判断：
   - 两向量是否同向（点积>0）
   - 两向量是否反向（点积<0）
   - 两向量是否垂直（点积≈0）
   - 在游戏中常用于判断角色朝向
</details>

---

## 🏆 挑战案例：综合应用

### 挑战1：创建"追踪"效果
**目标：** 让立方体始终追着红球跑

**提示：**
1. 计算从立方体到红球的方向向量
2. 沿着这个方向移动立方体一小步
3. 每帧重复

**代码框架：**
```javascript
// 在渲染循环中
const direction = sphereA.position.subtract(box.position).normalize();
box.position = box.position.add(direction.scale(0.05));  // 每帧移动0.05个单位
```

---

### 挑战2：创建"弹开"效果
**目标：** 当红球离绿球太近时，绿球"弹开"

**提示：**
1. 计算红球到绿球的距离
2. 如果距离小于阈值，计算排斥力方向
3. 移动绿球

**代码框架：**
```javascript
const diff = sphereB.position.subtract(sphereA.position);
const distance = diff.length();

if (distance < 3) {  // 太近了
    const pushDirection = diff.normalize();
    sphereB.position = sphereB.position.add(pushDirection.scale(0.1));
}
```

---

### 挑战3：创建轨道运动
**目标：** 让立方体围绕红球旋转

**提示：**
1. 使用sin/cos创建圆形轨迹
2. 圆心是红球位置
3. 半径是固定值

**代码框架：**
```javascript
const radius = 2;
const orbitOffset = new BABYLON.Vector3(
    Math.cos(time) * radius,
    0,
    Math.sin(time) * radius
);
box.position = sphereA.position.add(orbitOffset);
```

---

## 📊 学习检查清单

完成这些案例后，你应该能够：

- [ ] 理解向量的(x,y,z)三个分量的含义
- [ ] 知道如何创建和修改向量
- [ ] 理解向量加法的几何意义
- [ ] 理解向量减法表示方向
- [ ] 知道如何计算向量长度
- [ ] 理解归一化（normalize）的作用
- [ ] 会使用Lerp进行平滑过渡
- [ ] 理解点积的正负含义
- [ ] 能够用向量控制物体运动
- [ ] 能够计算物体间的距离和方向

---

## 🎓 进阶学习建议

完成这些案例后，你可以：

1. **尝试自己设计运动轨迹**
   - 8字形、心形、星形等

2. **实现物理效果**
   - 重力、弹跳、碰撞

3. **学习向量叉积（Cross Product）**
   - 计算垂直向量
   - 判断旋转方向

4. **应用到飞机控制**
   - 用方向向量控制飞机朝向
   - 用速度向量控制飞机移动

---

## 💡 常见错误提示

### 错误1：忘记归一化
```javascript
// ❌ 错误：直接使用方向向量，速度会受距离影响
const direction = target.subtract(current);
current = current.add(direction.scale(0.1));

// ✅ 正确：先归一化，速度恒定
const direction = target.subtract(current).normalize();
current = current.add(direction.scale(0.1));
```

### 错误2：混淆加法和赋值
```javascript
// ❌ 错误：add()返回新向量，不修改原向量
position.add(velocity);  // position没有改变！

// ✅ 正确：重新赋值
position = position.add(velocity);
```

### 错误3：忘记scale
```javascript
// ❌ 错误：归一化后长度是1，太慢了
position = position.add(direction.normalize());

// ✅ 正确：scale放大到合适的速度
position = position.add(direction.normalize().scale(0.5));
```

---

**完成所有案例后，你就掌握了向量的核心知识，可以继续任务5了！** 🎉

---

**开发者：Risen**
**文档创建日期：2025-12-13**
