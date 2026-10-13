# Farmer Simulator · 种田挂机小游戏

一款 2D 休闲种田挂机游戏，目标平台**微信小游戏**，通过激励视频广告（IAA）变现。

> **仓库**：https://github.com/ZSQ77ZSQ/Farmer-Simulator
> **本地目录**：`D:\My game\ZSQ`（仓库名改过，本地文件夹名保持不变）
> **当前状态**：前期准备阶段。技术链路已全部打通，玩法设计进行中，**尚未开始写游戏代码**。

---

## 核心玩法

```
买种子 → 播种 → [等待 / 离线生长] → 收获 → 卖钱 → 解锁地块 + 买更贵的种子
                                      ↓
                            订单（短期目标） · 图鉴（长线收集）
```

设计原则：**每次操作赚的钱指数增长**（8 → 30 → 105 → 360 → 900 → 2200 → 7800）。
挂机类游戏"变强"的手感全在这条曲线上。

---

## 环境要求

| 软件 | 版本 | 说明 |
|---|---|---|
| **Cocos Creator** | **3.8.8** | 必须严格一致（版本不同会导致 `.meta` 与场景格式冲突） |
| Cocos Dashboard | 2.2.2 | 启动器，可选 |
| Node.js | v24.19.0 | 核心逻辑测试用（直接跑 `.ts`） |
| Git | 2.55+ | |

> ⚠️ **Cocos Creator 版本必须两人完全一致**。不同版本打开同一工程会重写 `.meta`，产生大量无意义冲突。

---

## 怎么跑起来

### 1. 克隆

```bash
git clone <仓库地址>
cd ZSQ
```

### 2. 用 Cocos Creator 打开

启动 Cocos Creator → 「打开其他项目」→ 选择克隆下来的 `ZSQ` 目录。

首次打开会自动生成 `library/` 和 `temp/`（这两个目录**不提交**，是本地缓存，需要几分钟重建）。

### 3. 打开场景

在左下 **Assets** 面板里双击 `assets/scenes/main`。

### 4. 命令行构建（可选，用于自动化）

```bash
# Windows PowerShell
CocosCreator.exe --project "<项目路径>" --build "platform=web-mobile;debug=true;startScene=31a12b25-38df-40e3-8f7c-bb9dc57fc709"
```

产物在 `build/web-mobile/`，用任意静态服务器打开即可预览。

### 5. 跑核心逻辑测试（毫秒级，不用开编辑器）

```bash
node --test assets/core/__tests__/
```

---

## 目录结构

```
ZSQ/
├── assets/                    ← 所有游戏资源（提交）
│   ├── core/                  ← 【核心】零引擎依赖的纯 TypeScript
│   │   ├── crops.ts           ←   作物表（生长时间、成本、收益、解锁条件）
│   │   ├── economy.ts         ←   金币曲线、价格公式
│   │   ├── save.ts            ←   存档结构、离线收益结算
│   │   ├── state.ts           ←   农场状态机
│   │   ├── sim.ts             ←   数值模拟器
│   │   └── __tests__/         ←   node --test 测试
│   ├── scripts/               ← Cocos 组件（读 core 数据 → 更新节点）
│   ├── scenes/
│   │   └── main.scene         ← 起始场景
│   └── textures/              ← 图片资源
│
├── settings/                  ← 工程设置（提交）
├── library/                   ← 引擎导入缓存（不提交，自动生成）
├── temp/                      ← 编译临时文件（不提交，自动生成）
├── build/                     ← 构建产物（不提交，自动生成）
├── profiles/                  ← 个人偏好（不提交）
│
├── package.json
├── tsconfig.json
├── .gitignore
└── .gitattributes
```

### 架构要点

**「游戏大脑」和「引擎外壳」彻底分开。**

`assets/core/` 是**零引擎依赖的纯 TypeScript**，可以被 Node 直接执行：

```bash
node assets/core/sim.ts        # 毫秒级出结果
```

游戏好不好玩，**90% 取决于 `core/` 里的数值和规则**，而不是场景里摆了什么。
调数值只改一个表，秒级看结果——不必每次等 40 秒构建。

> 📌 `core/` 内的相对导入**必须带 `.ts` 后缀**：
> `import { CROPS } from './crops.ts'` ✅
> `import { CROPS } from './crops'` ❌（Node 无法解析）
> 这是实测结论：带后缀时 Node 能直跑、Cocos 也能正常打包，零额外配置。

---

## 协作

请阅读 [docs/GIT协作指南.md](docs/GIT协作指南.md)。

**最重要的一条规则**：Cocos 的 `.scene` / `.prefab` 是 JSON，但**无法安全地自动合并**。
**同一时间只能有一个人修改同一个场景或预制体。**

---

## 已知的坑

| 问题 | 原因 | 解法 |
|---|---|---|
| Cocos 命令行启动后 3 秒静默退出 | 沙箱/受限环境禁止创建命名管道，Electron 无法启动 | 需要完全文件权限 |
| `CocosCreator.exe --version` 返回 Node 版本号 | 环境变量 `ELECTRON_RUN_AS_NODE=1` 让 Electron 退化成纯 Node | 启动前 `Remove-Item Env:ELECTRON_RUN_AS_NODE` |
| 路径含中文时乱码（`桌面` → `妗岄潰`） | 参数传递编码问题 | **项目路径必须纯 ASCII** |
| Node 跑 `.ts` 报错 | 类型剥离不支持 `enum` / `namespace` / 构造函数参数属性 | 用 `const` 对象 + `as const` 代替 `enum` |

---

## 上架相关（备忘）

微信小游戏流量主开通条件（满足任一即可）：

- **累计独立访客 (UV) > 500**
- 开通内置游戏圈 + 发布官方帖 5 篇

个人主体**不能用微信虚拟支付（内购）**，与本项目 IAA 路线一致。

> 未查证：小游戏 ICP 备案的主体要求。
