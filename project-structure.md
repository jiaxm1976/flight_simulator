# 📁 项目文件结构说明

## 📂 完整目录结构

```
flight-simulator/                    # 项目根目录
│
├── docs/                            # 文档目录
│   ├── FLIGHT_SIMULATOR.md         # 主指导文档（你正在看的）
│   ├── project-structure.md        # 本文件：项目结构说明
│   └── phase-0-tasks.md            # 阶段0任务清单
│
├── src/                             # 源代码目录
│   ├── main.js                     # 主逻辑文件
│   ├── plane.js                    # 飞机类（阶段1开始创建）
│   ├── physics/                    # 物理引擎目录（阶段2开始）
│   │   ├── aerodynamics.js        # 气动力计算
│   │   └── flight-model.js        # 飞行模型
│   ├── controls/                   # 控制系统目录（阶段1开始）
│   │   └── input.js               # 键盘/手柄输入处理
│   └── ui/                         # 用户界面目录（阶段4开始）
│       └── hud.js                 # HUD仪表显示
│
├── assets/                          # 资源目录
│   ├── models/                     # 3D模型文件
│   │   └── plane.glb              # 飞机模型（或其他格式）
│   └── textures/                   # 贴图文件
│       ├── sky.jpg                # 天空贴图
│       └── ground.jpg             # 地面贴图
│
├── index.html                       # 入口HTML文件（第1天创建）
└── README.md                        # 项目说明（可选）
```

---

## 📄 文件说明

### 1. index.html（入口文件）
**作用：** 整个项目的入口，浏览器直接打开这个文件即可运行

**内容：**
- 引入Babylon.js库
- 创建canvas画布
- 引入所有JavaScript文件
- 基础CSS样式

**创建时间：** 第1天任务

**示例结构：**
```html
<!DOCTYPE html>
<html>
<head>
    <!-- 标题、样式 -->
</head>
<body>
    <canvas id="renderCanvas"></canvas>

    <!-- 按顺序引入JS文件 -->
    <script src="src/main.js"></script>
</body>
</html>
```

---

### 2. src/main.js（主逻辑）
**作用：** 项目的核心逻辑，初始化引擎、创建场景、渲染循环

**主要内容：**
```javascript
// 1. 初始化Babylon.js引擎
// 2. 创建场景
// 3. 创建相机
// 4. 创建光源
// 5. 创建地面和天空
// 6. 创建飞机（或调用plane.js）
// 7. 渲染循环
```

**创建时间：** 第1天任务

**文件大小：** 约100-300行代码（阶段0完成时）

---

### 3. src/plane.js（飞机类）
**作用：** 封装飞机相关的所有逻辑（位置、旋转、物理、控制等）

**主要内容：**
```javascript
class Plane {
    constructor(scene) {
        // 创建飞机模型
        // 初始化属性（速度、位置等）
    }

    update(deltaTime) {
        // 更新飞机状态（每帧调用）
    }

    applyThrust(amount) {
        // 施加推力
    }

    // 其他方法...
}
```

**创建时间：** 阶段1开始（第21天左右）

**文件大小：** 约200-500行代码（完成后）

---

### 4. src/physics/aerodynamics.js（气动力计算）
**作用：** 计算升力、阻力、失速等物理效果

**主要内容：**
```javascript
// 升力计算函数
function calculateLift(velocity, wingArea, liftCoefficient) {
    // 实现升力公式
}

// 阻力计算函数
function calculateDrag(velocity, dragCoefficient) {
    // 实现阻力公式
}

// 失速检测
function checkStall(velocity, stallSpeed) {
    // 检测是否失速
}
```

**创建时间：** 阶段2（第60天左右）

**文件大小：** 约100-200行代码

---

### 5. src/physics/flight-model.js（飞行模型）
**作用：** 整合所有物理效果，更新飞机的速度和位置

**主要内容：**
```javascript
class FlightModel {
    constructor(plane) {
        // 初始化物理参数
    }

    update(deltaTime) {
        // 计算所有力（重力、升力、阻力、推力）
        // 更新速度
        // 更新位置
    }
}
```

**创建时间：** 阶段2（第70天左右）

**文件大小：** 约150-300行代码

---

### 6. src/controls/input.js（输入处理）
**作用：** 处理键盘、鼠标、手柄输入

**主要内容：**
```javascript
class InputManager {
    constructor() {
        // 监听键盘事件
        // 存储按键状态
    }

    isKeyPressed(key) {
        // 检查某个键是否按下
    }

    getAxis(axisName) {
        // 获取轴输入（-1到1）
        // 例如：Horizontal（A/D键）
    }
}
```

**创建时间：** 阶段1（第25天左右）

**文件大小：** 约50-100行代码

---

### 7. src/ui/hud.js（HUD仪表）
**作用：** 在屏幕上显示速度、高度、姿态等信息

**主要内容：**
```javascript
class HUD {
    constructor() {
        // 创建HTML元素显示信息
    }

    update(plane) {
        // 更新显示的数据
        // 例如：速度、高度、俯仰角等
    }
}
```

**创建时间：** 阶段4或阶段5

**文件大小：** 约100-150行代码

---

## 📦 资源文件说明

### assets/models/
存放3D模型文件

**支持的格式：**
- `.glb` / `.gltf` - 推荐格式，Babylon.js原生支持
- `.obj` - 通用格式
- `.babylon` - Babylon.js专用格式

**获取模型的方法：**
1. 免费下载（Sketchfab等网站）
2. 使用Babylon.js基础形状拼接（初期推荐）
3. 自己用Blender建模（高级）

### assets/textures/
存放贴图文件

**常用贴图：**
- `sky.jpg` - 天空盒贴图
- `ground.jpg` - 地面贴图
- `runway.png` - 跑道贴图（阶段4）

---

## 🔄 阶段性文件创建时间表

| 阶段 | 需要创建的文件 | 说明 |
|------|----------------|------|
| **阶段0-第1天** | `index.html`<br>`src/main.js` | 基础框架 |
| **阶段0-第3天** | 添加地面和天空代码到main.js | 无新文件 |
| **阶段0-第10天** | `assets/models/plane.glb`（可选） | 或用代码创建 |
| **阶段1-第21天** | `src/plane.js`<br>`src/controls/input.js` | 飞机类和输入 |
| **阶段2-第60天** | `src/physics/aerodynamics.js`<br>`src/physics/flight-model.js` | 物理引擎 |
| **阶段4-第180天** | `src/ui/hud.js` | HUD显示 |

---

## 📝 文件组织原则

### 原则1：单一职责
每个文件只负责一个功能模块
- ✅ 好：`input.js` 只处理输入
- ❌ 坏：`input.js` 又处理输入又计算物理

### 原则2：从简单到复杂
- 阶段0：所有代码在 `index.html` 中（单文件）
- 阶段1：分离出 `main.js` 和 `plane.js`
- 阶段2+：进一步分离物理、控制、UI

### 原则3：按需创建
不要一开始就创建所有文件夹，用到时再创建

---

## 🎯 初学者建议

### 方案A：单文件开发（推荐新手）
**阶段0-1：** 所有代码写在 `index.html` 的 `<script>` 标签中

**优点：**
- 简单，不用管理多个文件
- 调试方便
- 适合快速原型

**缺点：**
- 代码多了难以维护

**适用场景：** 前50天（阶段0-1）

### 方案B：模块化开发（中后期）
**阶段2+：** 按照上面的目录结构组织代码

**优点：**
- 代码清晰
- 易于维护
- 团队协作友好

**缺点：**
- 需要理解模块化概念

**适用场景：** 第50天之后

---

## 🛠️ 文件引入顺序（重要！）

在 `index.html` 中引入多个JS文件时，**顺序很重要**：

```html
<!-- 错误顺序：plane.js用到了InputManager，但input.js还没加载 -->
<script src="src/plane.js"></script>
<script src="src/controls/input.js"></script>  <!-- 太晚了！ -->

<!-- 正确顺序：先加载依赖，再加载使用者 -->
<script src="src/controls/input.js"></script>
<script src="src/physics/aerodynamics.js"></script>
<script src="src/physics/flight-model.js"></script>
<script src="src/plane.js"></script>
<script src="src/main.js"></script>  <!-- main.js最后，因为它用到前面所有文件 -->
```

**规则：** 被依赖的文件先加载，依赖其他文件的后加载

---

## ❓ 常见问题

### Q1: 一定要按这个结构来吗？
**答：** 不一定。这是推荐结构，你可以根据自己的习惯调整。但建议至少做到：
- 代码和资源分离（src/ 和 assets/）
- 物理、控制、UI功能分离

### Q2: 初期可以只用一个HTML文件吗？
**答：** 完全可以！前2-3周（阶段0）建议只用一个 `index.html`，所有代码写在 `<script>` 标签里。等代码超过500行再考虑拆分。

### Q3: 如何引入外部3D模型？
**答：** 使用Babylon.js的加载器：
```javascript
BABYLON.SceneLoader.ImportMesh("", "assets/models/", "plane.glb", scene, function(meshes) {
    // meshes是加载的模型数组
    const plane = meshes[0];
});
```

### Q4: 需要用打包工具（Webpack/Vite）吗？
**答：** 不需要！本项目采用零配置方案，直接在浏览器运行HTML文件即可。如果将来项目很大，可以考虑引入工具。

---

## 🎯 下一步

现在你已经理解了项目结构，接下来：

1. ✅ 阅读 `phase-0-tasks.md` 开始第一个任务
2. ✅ 创建 `index.html` 文件
3. ✅ 显示第一个立方体！

加油！🚀
