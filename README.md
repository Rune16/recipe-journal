# 食谱集 · 菜谱平台

一个零依赖的图文菜谱平台。**双击 `index.html` 即可打开使用**，不需要安装任何环境、不需要启动服务器、也不需要联网。

- 首页精选：自动生成的轮播图（自动播放 / 悬停暂停 / 左右键 / 触摸滑动）
- 按类型浏览：菜谱分类卡片（川菜 / 粤菜 / 家常菜 / 汤羹 / 甜品 / 素食），点击即筛选
- 热门菜谱：按热度排名的菜谱卡片网格，支持关键词搜索（菜名 / 食材 / 标签）
- 查看做法：用料清单 + 带序号的图文分步做法、难度/时长/份量、点击图片放大
- 记录菜谱：新增或编辑菜谱，动态增删用料行与步骤行，上传封面图与步骤图
- 所有数据保存在浏览器本地，关掉页面再打开依然在

---

## 快速开始

1. 双击 `index.html`（或右键 →「用浏览器打开」）
2. 首页顶部是精选轮播图，往下依次是菜谱分类卡片与热门菜谱（共 10 道内置示例）
3. 点分类卡片可以只看某一类菜谱，点右上角搜索框可以搜菜名 / 食材 / 标签
4. 点「新建菜谱」录入自己的菜谱，保存后自动跳转到详情页

> 推荐使用 Chrome / Edge / Firefox 等现代浏览器。

### 手机访问（永久网址）

**https://rune16.github.io/recipe-journal/**

手机浏览器直接打开上面的网址即可，也可以扫描仓库根目录的 `phone-qr.svg` 二维码。该地址由 **GitHub Pages** 托管，长期有效，在任意网络下都能访问（校园 WiFi / 家用宽带 / 移动数据），不要求电脑开机，也不要求和电脑处于同一局域网。

- 已验证：三个入口 `index.html`、`detail.html?id=seed-01`、`edit.html` 在 Chromium 中均正常渲染（菜谱、用料、步骤齐全）
- 页面按屏幕宽度自动切换为手机布局：单列网格、轮播 4:3、隐藏左右箭头、操作栏吸底
- iOS Safari / 安卓 Chrome 可用「添加到主屏幕」，之后以独立 App 形式全屏打开
- 数据依旧保存在当前浏览器本地（`localStorage`），手机与电脑各自独立

### 发布到 GitHub Pages

仓库里已经准备好了发布包，直接在网页上操作即可，不需要安装 Git：

1. 打开 <https://github.com/new>，仓库名填 `recipe-journal`，可见性选 **Public**，点 **Create repository**
2. 在空仓库页点 **Add file → Upload files**，把 `site-deploy/` 下的 **3 个 HTML 文件** 拖进上传区，点 **Commit changes**
3. 进入 **Settings → Pages**，Source 选 **Deploy from a branch**，Branch 选 `main`、文件夹选 `/ (root)`，点 **Save**

约 1 分钟后即可通过 `https://<用户名>.github.io/recipe-journal/` 访问。

> **为什么只需要 3 个文件？** GitHub 网页上传拖拽文件夹经常失败（文件会被静默丢弃，页面打开后 0 道菜谱）。所以 `site-deploy/` 里的 HTML 已经把 CSS 与 JS 全部内联，**没有 `css/`、`js/` 子目录**，上传时不可能丢结构。
>
> 仓库根目录的 `index.html` / `css/` / `js/` 是源码（本地双击用），`site-deploy/` 是发布版（上传用），两者内容一致。
>
> 之后改了代码：重新生成 `site-deploy/` 的 3 个 HTML，在仓库里逐个点开同名文件 → 右上角铅笔图标 **Edit** → 全选替换 → **Commit changes**，网址保持不变。

---

## 页面结构

| 页面 | 说明 |
| --- | --- |
| `index.html` | 首页：精选轮播图 → 菜谱分类卡片 → 热门菜谱网格，含空状态与数据重置 |
| `detail.html?id=xxx` | 详情页：封面大图、元信息、用料清单、图文步骤、编辑与删除 |
| `edit.html` | 新建菜谱表单 |
| `edit.html?id=xxx` | 编辑指定菜谱 |

页面之间通过 URL 查询参数传递菜谱 id，例如 `detail.html?id=seed-03`。

---

## 目录结构

```
miniCRM/
├── index.html                # 首页：轮播图 / 菜谱分类 / 热门菜谱
├── detail.html               # 菜谱详情
├── edit.html                 # 新增 / 编辑菜谱
├── .nojekyll                 # 让 GitHub Pages 跳过 Jekyll 构建
├── phone-qr.svg              # 手机扫码访问用的二维码
├── site-deploy/              # 发布目录：与根目录同构，直接上传到 GitHub
├── recipe-site-deploy.zip    # site-deploy 的压缩包（备份用）
├── css/
│   ├── base.css              # 设计令牌、Reset、全局排版、工具类
│   ├── components.css        # 按钮、卡片、轮播图、分类卡片、胶囊标签、徽章、表单、上传区、时间轴、Toast、弹窗、灯箱
│   └── layout.css            # 导航、首页区块、网格、详情/表单布局、响应式
└── js/
    ├── data/recipes.js       # 内置示例菜谱数据
    ├── utils.js              # 转义、格式化、防抖、内联图标、封面渲染、Toast、弹窗、灯箱
    ├── storage.js            # 存储层（内置数据 + localStorage 合并读写）
    ├── image.js              # 图片校验、canvas 等比压缩、上传区绑定
    ├── list.js               # 首页逻辑（轮播控制器 / 分类筛选 / 热度排序 / 搜索）
    ├── detail.js             # 详情页逻辑
    └── edit.js               # 表单页逻辑
```

---

## 数据说明

### 数据模型

```js
{
  id: 'seed-01',            // 内置：seed-xx；用户创建：usr-xxxx
  title: '麻婆豆腐',
  emoji: '🥘',              // 没有封面图时显示的图标
  cover: '',                // 封面图：URL 或 base64 DataURL
  category: '川菜',         // 川菜 / 粤菜 / 家常菜 / 汤羹 / 甜品 / 素食
  tags: ['下饭', '经典'],
  difficulty: '中等',       // 简单 / 中等 / 困难
  timeMinutes: 25,
  servings: 2,
  description: '一句话简介',
  ingredients: [{ name: '嫩豆腐', amount: '400 克' }],
  steps: [{ text: '第一步做什么', image: '' }],   // 数组顺序即步骤序号
  tips: '小贴士',
  createdAt: 1767225600000,
  isCustom: false           // true 表示用户创建，可删除
}
```

### 存储位置

- 用户创建 / 编辑过的菜谱（含压缩后的图片）保存在浏览器 `localStorage`
- 键名：`minicrm.recipes.custom.v1`
- 首页页脚的「恢复初始数据」可以一键清空本地数据，回到内置示例状态

### 两种菜谱的区别

| | 内置示例菜谱 | 用户创建的菜谱 |
| --- | --- | --- |
| 来源 | `js/data/recipes.js` | `localStorage` |
| 能否编辑 | 可以，保存后会生成一条覆盖记录 | 可以 |
| 能否删除 | 不提供删除入口 | 可以，有二次确认 |

---

## 关于图片

- **上传的图片不会离开本机**：先用 `canvas` 等比压缩（最长边 1200px、JPEG 质量 0.78），再以 base64 存入 `localStorage`
- 单张图片输入上限 8MB，超过或格式不合法会给出明确提示
- `localStorage` 通常只有约 5MB，图片较多时会提示空间已满，可删除部分自建菜谱后重试
- 没有图片时不会出现破图：会自动渲染一张「渐变底色 + 菜品图标 + 菜名」的封面；图片加载失败时同样回退到它

### 给内置菜谱换上真实照片

编辑 `js/data/recipes.js`，把 `cover` 字段从空字符串改成图片地址即可：

```js
{
  id: 'seed-01',
  title: '麻婆豆腐',
  cover: 'assets/images/mapo-tofu.jpg',   // 相对路径或 https 链接都支持
  // ...
}
```

也可以在页面上点「编辑」，直接上传图片——这种方式会把图片存进浏览器本地。

---

## 技术说明

- **纯 HTML5 + 原生 CSS3 + 原生 JavaScript（ES6）**，无框架、无构建工具、无 CDN
- 脚本全部使用经典 `<script>` 标签并按顺序加载，各自用 IIFE 包裹后挂到全局命名空间 `window.App`（`App.Utils` / `App.Storage` / `App.Image`）
- 内置数据放在 `js/data/recipes.js` 而不是 `.json` 文件：因为 `file://` 协议下浏览器会拦截 `fetch()`，用 JS 常量承载数据才能保证双击打开就能用
- 所有用户输入在插入 DOM 前统一经过 `escapeHtml` 转义
- 搜索输入做了 200ms 防抖，列表一次性拼接 HTML 后赋值，减少重排
- 轮播图只改 `transform`（合成层动画），不重建 DOM、不重新请求图片；`prefers-reduced-motion: reduce` 时停止自动播放，并提供播放/暂停按钮
- 首页没有浏览量字段，「热门」由 `js/list.js` 的 `heatScore()` 计算：标签权重（经典/宴客/硬菜/下饭/快手…）+ 步骤与用料丰富度 + 是否本人创建，同分按创建时间倒序
- 响应式断点：≥1024px 三列菜谱网格 + 分类三列 + 详情双栏；768~1023px 两列（轮播 16:9）；<768px 单列、轮播 4:3 且隐藏左右箭头、操作栏吸底

---

## 常见问题

**页面打开后是空白的？**
请确认 `css/` 与 `js/` 目录和 HTML 文件在同一层级，且没有被移动或改名。

**上传图片提示空间已满？**
`localStorage` 容量有限。可以在首页页脚点「恢复初始数据」清空，或改用更小的图片；也可以把图片放到 `assets/images/` 下，然后在 `recipes.js` 的 `cover` 字段里引用相对路径。

**内置菜谱被我改坏了怎么办？**
在首页页脚点「恢复初始数据」，内置菜谱会恢复原样。
