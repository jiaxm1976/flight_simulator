# 🛩️ 飞行模拟器开发项目 - 完整指导文档

## 📌 项目概述

### 项目目标
从零开始开发一个基于Web的3D飞行模拟器，核心体验：**完成飞机的起飞和降落**

### 技术栈
- **3D引擎**：Babylon.js 6.x
- **编程语言**：JavaScript（通过超详细注释边做边学）
- **物理引擎**：Cannon.js（初期）+ 自定义气动力学（后期）
- **开发方式**：单HTML文件，零配置快速启动

### 开发模式
- **每天投入**：30分钟
- **学习方式**：任务驱动，通过超详细代码注释理解原理
- **物理复杂度**：模拟级（副翼、升降舵、方向舵独立控制）

---

## ⏱️ 项目时间线

| 阶段 | 内容 | 预计时间 | 累计天数 |
|------|------|----------|----------|
| **阶段0** | Babylon.js基础 + 显示飞机模型 | 15-20天 | 20天 |
| **阶段1** | 键盘控制基础飞行 | 25-30天 | 50天 |
| **阶段2** | 真实物理引擎（升力、阻力、重力） | 30-40天 | 90天 |
| **阶段3** | 飞行控制面（副翼、升降舵、方向舵） | 40-50天 | 140天 |
| **阶段4** | 跑道系统 + 起飞降落 | 50-70天 | 210天 |
| **阶段5** | 细节打磨（仪表盘、音效、优化） | 持续迭代 | - |

**总计：约 5.5-7 个月**（每天30分钟）

---

## 📚 阶段详细说明

### 阶段0：Babylon.js基础 + 显示飞机（15-20天）

#### 学习目标
1. 理解Babylon.js的基本概念（场景、相机、光源、网格）
2. 掌握3D坐标系（X/Y/Z轴）
3. 学会加载和显示3D模型
4. 理解渲染循环的概念

#### 关键任务
- **第1-2天**：创建基础场景，显示立方体
- **第3-4天**：添加地面和天空
- **第5-7天**：学习3D坐标系和向量基础
- **第8-10天**：加载简单飞机模型（或用基础形状拼接）
- **第11-15天**：相机跟随飞机，第三人称视角
- **第16-20天**：添加基础环境（山脉、云层等）

#### 验收标准
- ✅ 能显示一个飞机模型在3D场景中
- ✅ 相机能跟随飞机视角
- ✅ 有地面和天空

#### 本阶段需要的最小JavaScript知识
```javascript
// 1. 变量声明
const speed = 10;      // 常量（不会变）
let height = 0;        // 变量（会变）

// 2. 函数
function update() {    // 定义函数
    // 代码
}
update();              // 调用函数

// 3. 对象
plane.position.x = 10; // 访问对象的属性

// 4. 条件判断
if (speed > 100) {     // 如果速度大于100
    // 做某事
}

// 5. 基础运算
x = x + 1;             // 加法
x += 1;                // 简写形式，等同于 x = x + 1
```

---

### 阶段1：键盘控制基础飞行（25-30天）

#### 学习目标
1. 理解键盘输入处理
2. 学会通过代码移动和旋转物体
3. 理解欧拉角（pitch/yaw/roll）
4. 掌握时间增量（deltaTime）的概念

#### 关键任务
- **第21-25天**：键盘输入监听（WASD控制）
- **第26-30天**：前进/后退控制
- **第31-35天**：左右转向控制
- **第36-40天**：爬升/下降控制
- **第41-45天**：平滑运动和速度限制
- **第46-50天**：基础碰撞检测（防止穿地）

#### 验收标准
- ✅ 能用键盘控制飞机前进、转向、爬升、下降
- ✅ 运动流畅，速度合理
- ✅ 飞机不会穿过地面

#### 关键概念

**3D旋转的三个轴：**
```
俯仰角 (Pitch)：飞机抬头/低头
偏航角 (Yaw)：飞机左转/右转
滚转角 (Roll)：飞机侧翻
```

**deltaTime（时间增量）：**
```javascript
// 为什么需要deltaTime？
// 不同电脑运行速度不同，有的每秒60帧，有的120帧
// 使用deltaTime确保在所有电脑上运动速度一致

const deltaTime = engine.getDeltaTime() / 1000; // 转换为秒
const moveDistance = speed * deltaTime;          // 距离 = 速度 × 时间
plane.position.z += moveDistance;                // 更新位置
```

---

### 阶段2：真实物理引擎（30-40天）

#### 学习目标
1. 理解四种基本力（重力、升力、阻力、推力）
2. 学会使用Rigidbody（刚体）
3. 理解速度和加速度的关系
4. 掌握失速的概念

#### 关键任务
- **第51-60天**：引入重力，飞机会自然下落
- **第61-70天**：实现升力公式（速度越快升力越大）
- **第71-80天**：实现阻力（速度越快阻力越大）
- **第81-90天**：实现推力（油门控制）
- **第91-100天**：失速检测（速度太慢会掉高度）

#### 验收标准
- ✅ 飞机不给油门会因重力下落
- ✅ 飞得越快，升力越大，越容易爬升
- ✅ 速度太慢会失速下坠
- ✅ 阻力会自然减速

#### 核心物理公式

**1. 升力公式（简化版）：**
```
L = 0.5 × ρ × V² × S × CL

L  = 升力（Lift）
ρ  = 空气密度（取固定值 1.225）
V  = 速度（Velocity）
S  = 机翼面积（固定值，比如 20）
CL = 升力系数（固定值，比如 0.5）
```

**JavaScript实现：**
```javascript
// 计算升力
const airDensity = 1.225;           // 空气密度（海平面）
const wingArea = 20;                 // 机翼面积（平方米）
const liftCoefficient = 0.5;         // 升力系数

// 获取飞机当前速度
const velocity = plane.velocity.length(); // 速度的大小

// 套用公式
const lift = 0.5 * airDensity * velocity * velocity * wingArea * liftCoefficient;

// 升力施加在Y轴（向上）
plane.applyForce(new BABYLON.Vector3(0, lift, 0));
```

**2. 阻力公式（简化版）：**
```
D = 0.5 × ρ × V² × S × CD

D  = 阻力（Drag）
CD = 阻力系数（比如 0.02）
其他参数同升力公式
```

**3. 失速条件：**
```javascript
// 失速速度：最低安全飞行速度
const stallSpeed = 30; // 米/秒

if (velocity < stallSpeed) {
    // 升力急剧下降
    lift = lift * 0.3; // 只剩30%升力
    // 飞机会掉高度
}
```

---

### 阶段3：飞行控制面（40-50天）

#### 学习目标
1. 理解飞机控制面的作用（副翼/升降舵/方向舵）
2. 学会施加旋转力矩
3. 掌握角速度的概念
4. 理解协调转弯

#### 关键任务
- **第101-115天**：升降舵控制（俯仰角）
- **第116-130天**：方向舵控制（偏航角）
- **第131-145天**：副翼控制（滚转角）
- **第146-160天**：控制面协调（同时使用多个控制面）

#### 验收标准
- ✅ 能独立控制俯仰、偏航、滚转
- ✅ 能做出特技动作（如滚转360度）
- ✅ 转弯时自然倾斜

#### 三个控制面详解

**1. 升降舵（Elevator）- 控制俯仰**
```
位置：尾翼水平面
作用：让飞机抬头或低头
操作：键盘 ↑↓ 或 W/S
```

```javascript
// 升降舵输入（-1到1）
const elevatorInput = Input.GetAxis("Vertical"); // ↑=1, ↓=-1

// 产生俯仰力矩
const pitchTorque = elevatorInput * pitchPower; // pitchPower = 力矩强度
plane.applyTorque(new BABYLON.Vector3(pitchTorque, 0, 0)); // X轴旋转
```

**2. 方向舵（Rudder）- 控制偏航**
```
位置：尾翼垂直面
作用：让飞机机头左转或右转
操作：键盘 Q/E
```

```javascript
// 方向舵输入
const rudderInput = Input.GetKey("E") ? 1 : (Input.GetKey("Q") ? -1 : 0);

// 产生偏航力矩
const yawTorque = rudderInput * yawPower;
plane.applyTorque(new BABYLON.Vector3(0, yawTorque, 0)); // Y轴旋转
```

**3. 副翼（Aileron）- 控制滚转**
```
位置：主翼两侧
作用：让飞机向左或向右倾斜
操作：键盘 ←→ 或 A/D
```

```javascript
// 副翼输入
const aileronInput = Input.GetAxis("Horizontal"); // →=1, ←=-1

// 产生滚转力矩
const rollTorque = aileronInput * rollPower;
plane.applyTorque(new BABYLON.Vector3(0, 0, rollTorque)); // Z轴旋转
```

---

### 阶段4：跑道系统 + 起飞降落（50-70天）

#### 学习目标
1. 理解地面摩擦力
2. 学会检测起落架接地
3. 掌握起飞速度和降落速度
4. 理解进近角度（approach angle）

#### 关键任务
- **第161-175天**：创建跑道场景
- **第176-190天**：起落架系统（检测接地）
- **第191-205天**：地面滑行和转向
- **第206-220天**：起飞流程（加速 → 拉起）
- **第221-240天**：降落流程（对准 → 下降 → 着陆）

#### 验收标准
- ✅ 能从跑道起飞
- ✅ 能绕圈飞行
- ✅ 能安全降落在跑道上
- ✅ 降落时不会摔机

#### 起飞流程

**标准起飞步骤：**
```
1. 对准跑道中心线
2. 全油门加速
3. 速度达到起飞速度（VR）时，拉升降舵
4. 离地后保持爬升姿态
5. 收起起落架（可选）
```

**代码示例：**
```javascript
// 起飞速度（Rotation Speed）
const VR = 40; // 米/秒

// 检查当前速度
if (velocity >= VR && isOnGround) {
    // 提示玩家可以拉起
    showMessage("速度足够，按S拉起");
}

// 检测离地
if (!isOnGround && wasOnGround) {
    showMessage("起飞成功！");
}
```

#### 降落流程

**标准降落步骤：**
```
1. 下降到进近高度（约300米）
2. 对准跑道延长线
3. 保持下降率（约3米/秒）
4. 保持进近速度（失速速度 × 1.3）
5. 跑道头前拉起，平飘
6. 主轮接地
7. 放下前轮
8. 刹车减速
```

**关键参数：**
```javascript
// 进近速度（Approach Speed）
const approachSpeed = stallSpeed * 1.3; // 失速速度的1.3倍

// 下降率（Descent Rate）
const targetDescentRate = -3; // 米/秒，负数表示下降

// 进近角度
const approachAngle = 3; // 度，一般为3度
```

---

### 阶段5：细节打磨（持续迭代）

#### 可选功能
- HUD仪表显示（速度、高度、姿态指示器）
- 音效（引擎声、风声、失速警告）
- 天气系统（风、降雨）
- 多种飞机型号
- 场景扩展（多个机场）
- 重播系统
- 第一人称驾驶舱视角

---

## 🛠️ 技术参考

### Babylon.js 核心类

**场景管理：**
```javascript
BABYLON.Engine          // 渲染引擎
BABYLON.Scene           // 场景容器
```

**相机：**
```javascript
BABYLON.ArcRotateCamera      // 弧形旋转相机（第三人称）
BABYLON.FollowCamera         // 跟随相机
BABYLON.UniversalCamera      // 通用相机（第一人称）
```

**光源：**
```javascript
BABYLON.HemisphericLight     // 半球光（环境光）
BABYLON.DirectionalLight     // 平行光（太阳光）
BABYLON.PointLight           // 点光源
```

**几何体：**
```javascript
BABYLON.MeshBuilder.CreateBox()      // 立方体
BABYLON.MeshBuilder.CreateSphere()   // 球体
BABYLON.MeshBuilder.CreateGround()   // 地面
BABYLON.MeshBuilder.CreatePlane()    // 平面
```

**数学类：**
```javascript
BABYLON.Vector3         // 三维向量 (x, y, z)
BABYLON.Quaternion      // 四元数（旋转）
BABYLON.Color3          // 颜色 (r, g, b)
BABYLON.Matrix          // 矩阵
```

---

## 📖 附录A：HTML基础速查

### 标签的基本结构
```html
<标签名 属性名="属性值">内容</标签名>
```

### 常用标签
```html
<html>          网页根标签
<head>          头部信息（不显示）
<body>          页面内容（显示）
<div>           容器标签
<canvas>        画布标签（用于绘图）
<script>        JavaScript代码
<style>         CSS样式
```

### 标签的属性
```html
id="名称"       唯一标识符
class="名称"    类名（可重复）
style="样式"    内联样式
src="路径"      资源路径
```

### 注释写法
```html
<!-- 这是HTML注释 -->
```

---

## 📖 附录B：CSS基础速查

### 选择器
```css
body { }        /* 标签选择器 */
#id { }         /* ID选择器（#开头） */
.class { }      /* 类选择器（.开头） */
```

### 常用属性
```css
/* 盒模型 */
margin: 0;              /* 外边距 */
padding: 0;             /* 内边距 */
width: 100%;            /* 宽度 */
height: 100vh;          /* 高度（vh=视口高度） */

/* 显示 */
display: block;         /* 块级元素 */
display: none;          /* 隐藏 */

/* 定位 */
position: absolute;     /* 绝对定位 */
top: 0;                 /* 距顶部距离 */
left: 0;                /* 距左侧距离 */

/* 文本 */
color: white;           /* 文字颜色 */
font-size: 16px;        /* 字体大小 */
```

### 注释写法
```css
/* 这是CSS注释 */
```

---

## 📖 附录C：JavaScript基础速查

### 变量声明
```javascript
const x = 10;           // 常量（不会改变）
let y = 20;             // 变量（可以改变）
var z = 30;             // 旧语法（不推荐）
```

### 数据类型
```javascript
// 数字
const age = 25;
const speed = 10.5;

// 字符串
const name = "飞机";
const message = '起飞成功';

// 布尔值
const isFlying = true;
const isOnGround = false;

// 对象
const plane = {
    speed: 100,
    height: 500
};

// 数组
const speeds = [10, 20, 30];
```

### 运算符
```javascript
// 算术运算
x + y               // 加
x - y               // 减
x * y               // 乘
x / y               // 除
x % y               // 取余

// 赋值运算
x = 10              // 赋值
x += 5              // x = x + 5
x -= 5              // x = x - 5
x *= 2              // x = x * 2

// 比较运算
x === y             // 等于（严格）
x !== y             // 不等于
x > y               // 大于
x < y               // 小于
x >= y              // 大于等于
x <= y              // 小于等于

// 逻辑运算
a && b              // 与（and）
a || b              // 或（or）
!a                  // 非（not）
```

### 条件判断
```javascript
// if语句
if (speed > 100) {
    console.log("速度太快");
} else if (speed > 50) {
    console.log("速度正常");
} else {
    console.log("速度太慢");
}

// 三元运算符
const status = speed > 100 ? "快" : "慢";
```

### 循环
```javascript
// for循环
for (let i = 0; i < 10; i++) {
    console.log(i);
}

// while循环
while (speed > 0) {
    speed -= 1;
}
```

### 函数
```javascript
// 函数声明
function add(a, b) {
    return a + b;
}

// 函数调用
const result = add(5, 3);

// 箭头函数（简写）
const multiply = (a, b) => a * b;
```

### 对象
```javascript
// 创建对象
const plane = {
    speed: 100,
    height: 500,
    fly: function() {
        console.log("飞行中");
    }
};

// 访问属性
console.log(plane.speed);       // 点号访问
console.log(plane["speed"]);    // 方括号访问

// 修改属性
plane.speed = 120;

// 调用方法
plane.fly();
```

### 数组
```javascript
// 创建数组
const numbers = [1, 2, 3, 4, 5];

// 访问元素
console.log(numbers[0]);        // 第一个元素（索引从0开始）

// 修改元素
numbers[0] = 10;

// 常用方法
numbers.push(6);                // 添加到末尾
numbers.pop();                  // 删除最后一个
numbers.length;                 // 数组长度
```

### 注释写法
```javascript
// 这是单行注释

/*
这是
多行注释
*/
```

---

## 📖 附录D：3D数学基础

### 向量（Vector）

**什么是向量？**
向量是有方向和大小的量，在3D空间中用(x, y, z)表示。

```javascript
// 创建向量
const position = new BABYLON.Vector3(10, 5, 0);
// x=10（左右）, y=5（上下）, z=0（前后）

// 向量加法
const v1 = new BABYLON.Vector3(1, 0, 0);
const v2 = new BABYLON.Vector3(0, 1, 0);
const result = v1.add(v2);  // (1, 1, 0)

// 向量减法
const direction = targetPos.subtract(currentPos);

// 向量长度（大小）
const distance = direction.length();

// 单位向量（长度为1的向量）
const normalized = direction.normalize();

// 向量缩放
const scaled = direction.scale(2);  // 长度变为2倍
```

**常用向量：**
```javascript
BABYLON.Vector3.Zero()          // (0, 0, 0) 原点
BABYLON.Vector3.Up()            // (0, 1, 0) 向上
BABYLON.Vector3.Down()          // (0, -1, 0) 向下
BABYLON.Vector3.Left()          // (-1, 0, 0) 向左
BABYLON.Vector3.Right()         // (1, 0, 0) 向右
BABYLON.Vector3.Forward()       // (0, 0, 1) 向前
BABYLON.Vector3.Backward()      // (0, 0, -1) 向后
```

### 坐标系

**Babylon.js 坐标系（左手系）：**
```
Y轴：向上（高度）
X轴：向右（横向）
Z轴：向前（深度）

     Y(上)
     |
     |
     |_____ X(右)
    /
   /
  Z(前)
```

### 旋转

**欧拉角（Euler Angles）：**
```javascript
// 三个旋转角度
plane.rotation.x  // 俯仰角（Pitch）：抬头/低头
plane.rotation.y  // 偏航角（Yaw）：左转/右转
plane.rotation.z  // 滚转角（Roll）：侧翻

// 角度和弧度转换
const degrees = 45;                    // 45度
const radians = degrees * Math.PI / 180; // 转换为弧度
```

**常用三角函数：**
```javascript
Math.sin(angle)     // 正弦
Math.cos(angle)     // 余弦
Math.tan(angle)     // 正切

Math.PI             // π ≈ 3.14159
Math.PI / 2         // 90度
Math.PI             // 180度
Math.PI * 2         // 360度
```

---

## ❓ 常见问题与解决方案

### Q1: 飞机抖动/不稳定
**原因：**
- 物理更新频率太高
- 力的大小不合理
- deltaTime使用错误

**解决方案：**
```javascript
// 限制力的大小
const maxForce = 1000;
force = Math.min(force, maxForce);

// 添加阻尼
velocity *= 0.98;  // 每帧衰减2%

// 使用固定时间步长
const fixedDeltaTime = 1/60;  // 固定为60FPS
```

### Q2: 飞机穿过地面
**原因：**
- 没有碰撞检测
- 速度太快，跳过了地面

**解决方案：**
```javascript
// 简单的地面检测
if (plane.position.y < groundHeight) {
    plane.position.y = groundHeight;  // 强制设置在地面上
    plane.velocity.y = 0;              // 垂直速度清零
}
```

### Q3: 转弯时飞机侧翻
**原因：**
- 这是正确的！真实飞机转弯时会倾斜
- 如果想要街机式（不倾斜），需要锁定滚转角

**解决方案（如果想锁定）：**
```javascript
// 每帧重置滚转角
plane.rotation.z = 0;
```

### Q4: 找不到错误在哪里
**调试技巧：**
```javascript
// 在浏览器按F12打开控制台，使用console.log
console.log("飞机位置:", plane.position);
console.log("速度:", velocity);

// 在页面上显示信息
document.getElementById("debug").innerHTML =
    "速度: " + velocity + "<br>" +
    "高度: " + plane.position.y;
```

### Q5: 性能太差/卡顿
**优化建议：**
```javascript
// 1. 降低模型精度
// 使用低多边形模型

// 2. 减少光源数量
// 只保留1-2个光源

// 3. 禁用阴影（初期）
light.shadowEnabled = false;

// 4. 降低渲染分辨率
engine.setHardwareScalingLevel(2);  // 分辨率减半
```

---

## 🔗 学习资源

### 官方文档
- [Babylon.js 官方文档](https://doc.babylonjs.com/)
- [Babylon.js Playground](https://playground.babylonjs.com/) - 在线代码编辑器

### 推荐教程
- Babylon.js 入门系列（YouTube）
- MDN Web Docs - JavaScript 教程

### 免费3D模型资源
- [Sketchfab](https://sketchfab.com/) - 搜索 "airplane" 免费模型
- [Free3D](https://free3d.com/)
- [TurboSquid Free](https://www.turbosquid.com/Search/3D-Models/free)

### 航空知识
- 维基百科：飞行原理
- YouTube: "How do airplanes fly" 相关视频

---

## 📝 项目检查清单

### 阶段0 完成标准
- [ ] 创建了基础HTML文件
- [ ] 显示了3D场景（天空+地面）
- [ ] 加载了飞机模型（或用形状拼接）
- [ ] 相机能跟随飞机

### 阶段1 完成标准
- [ ] 键盘WASD能控制飞机移动
- [ ] 上下键能控制爬升/下降
- [ ] 左右键能控制转向
- [ ] 运动流畅，不会突然瞬移
- [ ] 飞机不会穿过地面

### 阶段2 完成标准
- [ ] 飞机不给油门会因重力下落
- [ ] 飞得越快，升力越大
- [ ] 速度太慢会失速掉高度
- [ ] 速度太快会因阻力自然减速

### 阶段3 完成标准
- [ ] 能独立控制俯仰（升降舵）
- [ ] 能独立控制偏航（方向舵）
- [ ] 能独立控制滚转（副翼）
- [ ] 能做出360度滚转动作
- [ ] 转弯时自然倾斜

### 阶段4 完成标准
- [ ] 有一条完整的跑道
- [ ] 能从跑道起飞
- [ ] 能绕圈飞行
- [ ] 能降落在跑道上
- [ ] 降落时不会摔机

---

## 💬 开发建议

1. **不要跳步骤**：每个阶段扎实完成再进入下一个
2. **遇到bug先看控制台**：99%的错误会显示在F12控制台
3. **每天提交代码**：养成版本管理习惯（可选，使用git）
4. **记录问题**：遇到的问题和解决方法记下来
5. **适时调整难度**：如果卡太久，可以简化目标
6. **享受过程**：做游戏本身就很有趣！

---

## 🎯 下一步行动

1. 阅读 `project-structure.md` 了解项目文件组织
2. 阅读 `phase-0-tasks.md` 开始第一个任务
3. 创建第一个HTML文件，显示立方体
4. 遇到问题随时询问

祝你开发顺利！✈️
