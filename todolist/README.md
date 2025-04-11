# Vue TodoList 应用

一个基于Vue 3和TypeScript开发的高保真UI待办事项应用，具有简洁美观的界面和完整的任务管理功能。

## 📝 功能特点

- ✨ **美观的UI设计**：精心设计的用户界面，具有现代感和良好的视觉体验
- 📱 **响应式布局**：适配各种屏幕尺寸的设备（桌面端、平板和手机）
- 🌓 **明暗主题**：自动适应系统的明暗模式设置
- ✅ **完整的任务管理**：
  - 添加新的待办事项
  - 编辑现有待办事项
  - 标记完成/未完成状态
  - 删除待办事项
  - 清除所有已完成的待办事项
- 🔍 **过滤功能**：可以筛选全部、未完成和已完成的待办事项
- 🔄 **批量操作**：支持一键全选/取消全选功能
- 💾 **本地存储**：使用localStorage自动保存数据，刷新页面数据不丢失
- ⌨️ **键盘快捷键**：支持回车添加任务、编辑时Esc取消等快捷键操作
- 🎬 **平滑动画**：添加和删除任务时的流畅过渡动画

## 🛠️ 技术栈

- **前端框架**：Vue 3
- **开发语言**：TypeScript
- **构建工具**：Vite
- **CSS**：原生CSS（无需额外框架）
- **数据存储**：localStorage
- **测试工具**：Vitest（已配置）

## 📦 安装与运行

### 前提条件

- Node.js 16.0 或更高版本
- npm 或 yarn 或 pnpm

### 安装步骤

1. 克隆仓库或下载源码

```bash
git clone <仓库地址>
cd todolist
```

2. 安装依赖

```bash
npm install
# 或者
yarn
# 或者
pnpm install
```

3. 启动开发服务器

```bash
npm run dev
# 或者
yarn dev
# 或者
pnpm dev
```

4. 在浏览器中访问 `http://localhost:5173/`

### 构建生产版本

```bash
npm run build
# 或者
yarn build
# 或者
pnpm build
```

构建完成后，生产文件将位于 `dist` 目录中。

## 🧩 项目结构

```
todolist/
├── public/             # 静态资源
├── src/                # 源代码
│   ├── assets/         # 资源文件（图片、字体等）
│   ├── components/     # 组件
│   │   ├── TodoItem.vue   # 单个待办事项组件
│   │   └── TodoList.vue   # 待办事项列表组件
│   ├── App.vue         # 应用根组件
│   └── main.ts         # 入口文件
├── index.html          # HTML模板
├── package.json        # 项目配置
├── tsconfig.json       # TypeScript配置
└── vite.config.ts      # Vite配置
```

## 📖 使用指南

1. **添加待办事项**：在输入框中输入待办事项内容，点击"添加"按钮或按回车键添加
2. **编辑待办事项**：点击待办事项右侧的✏️按钮进入编辑模式，修改后按回车键或点击✓按钮保存
3. **标记完成/未完成**：点击待办事项左侧的复选框
4. **删除待办事项**：点击待办事项右侧的🗑️按钮
5. **过滤待办事项**：点击上方的"全部"、"未完成"或"已完成"按钮
6. **全选/取消全选**：点击列表右上角的"全选"按钮
7. **清除已完成**：点击列表底部的"清除已完成"按钮

## 📝 开发说明

### 组件设计

- **TodoItem.vue**：负责单个待办事项的显示、编辑和状态管理
- **TodoList.vue**：负责整体列表管理、过滤和数据持久化
- **App.vue**：应用根组件，提供全局样式和布局

### 数据持久化

应用使用浏览器的localStorage API进行数据持久化存储：

```typescript
// 保存数据
localStorage.setItem('todos', JSON.stringify(todos.value));

// 加载数据
const savedTodos = localStorage.getItem('todos');
if (savedTodos) {
  todos.value = JSON.parse(savedTodos);
}
```

## 🎨 自定义主题

应用支持系统明暗模式，无需手动切换。如需自定义颜色主题，可修改以下文件：

- `App.vue` - 全局颜色变量和主题
- `TodoList.vue` 和 `TodoItem.vue` - 组件特定样式

## 📄 许可证

[MIT](LICENSE)

## 🙏 致谢

- 感谢Vue团队提供优秀的前端框架
- 感谢所有开源项目的贡献者们
