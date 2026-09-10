# 单词记忆卡片（Word Cards）

一个基于 Vue 3 + Vite 的单词记忆小应用：以卡片形式展示英文单词与中文释义，支持"认识 / 不认识"标记，自动统计学习进度，并将学习状态保存在浏览器本地（localStorage），刷新后不丢失。

## 项目简介

- 功能：单词卡片翻卡学习、认识/不认识标记、学习进度统计、自定义单词添加
- 定位：《Web开发实践》课程个人项目（选题：单词记忆卡片）
- 作者：高扬欣（2500702531，计算机2505班）

## 技术栈

| 类别 | 技术 |
|------|------|
| 框架 | Vue 3（组合式 API，`<script setup>`） |
| 构建工具 | Vite |
| 组件化 | WordCard（卡片）/ StatsBar（统计条）双组件 + props/emit 通信 |
| 数据持久化 | localStorage（键：`vue_card_known`、`vue_card_custom`） |
| 版本管理 | Git + 腾讯云 CODING 远程仓库 |

## 运行方式

环境要求：Node.js ≥ 18、npm ≥ 9（推荐 Node 20+）。

```bash
# 1. 安装依赖
npm install

# 2. 本地启动开发服务器
npm run dev
# 浏览器访问终端输出的地址（默认 http://localhost:5173/）

# 3. 构建生产版本
npm run build
# 产物输出到 dist/ 目录
```

## 目录结构

```
my-vue-app/
├── index.html               # 入口 HTML
├── package.json             # 项目依赖与脚本
├── vite.config.js           # Vite 构建配置
├── .gitignore               # 版本管理忽略规则（node_modules、dist 等）
├── README.md                # 项目说明（本文件）
├── public/                  # 静态资源（favicon、图标）
│   ├── favicon.svg
│   └── icons.svg
└── src/                     # 源代码
    ├── main.js              # 应用入口（挂载 Vue 实例）
    ├── App.vue              # 根组件（数据层 + 业务逻辑）
    ├── style.css            # 全局样式
    ├── assets/              # 静态图片资源
    └── components/          # 功能组件
        ├── WordCard.vue     # 单词卡片组件（翻卡 / 认识 / 不认识）
        └── StatsBar.vue     # 学习进度统计组件
```

## 线上地址

- 腾讯云 COS 静态网站：<https://word-cards-2500702531-1483176760.cos-website.ap-guangzhou.myqcloud.com>
- 腾讯云 EdgeOne Pages：<https://word-cards-vue-tjufgoks.edgeone.cool/>

## 版本记录

- v1.0.0：完成单词记忆卡片应用开发并部署上线（Vue 3 组件化版本）
