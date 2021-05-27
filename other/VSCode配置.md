> 以下为个人对 [Visual Studio Code](https://code.visualstudio.com/) 的插件使用情况和习惯配置，请根据自身情况进行使用

**我的插件：**

- 标签补全更新: `Auto Close TagAuto` 、`Complete TagAuto Rename Tag`
- git 版本管理: `GitLens`
- 配置本地服务器: `Live Server`
- lodash 提示: `Lodash`
- markdown 编译展示，功能比 vscode 自带编译强大: `Markdown Preview Enhanced`
- 代码格式化工具: `Prettier`
- Vue 项目支持: Vetur
- 代码补全工具: `TabNine`
- 注释辅助，/\*\*直接按回车使用: `Document this`
- 注释翻译: `Comment Translate`
- 备忘: `TODO Highlight`
- 为 Vs Code 快捷生成 ES6 的语法（ js 和 ts 同时都支持）: `javascript (ES6) code snippets`
- python 编码必备，自带检测和格式化工具: `python`
- drawio 流程图绘制工具，命名 ` .drawio` 后缀使用

**`setting.json`文件配置：**

```javascript
{
  // tab 大小为2个空格
  "editor.tabSize": 2,
  // 编辑器换行
  "editor.wordWrap": "off",
  // 保存时格式化
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  // 添加 vue 支持
  "files.associations": {
    ".eslintrc": "json",
    "*.vue": "vue"
  },
  // 开启 vscode 文件路径导航
  "breadcrumbs.enabled": true,
  // prettier 设置语句末尾加分号
  "prettier.semi": true,
  // prettier 设置强制单引号
  "prettier.singleQuote": false,
  // prettier 单行最长折行限制
  "prettier.printWidth": 80,
  // 在对象或数组最后一个元素后面是不加逗号
  "prettier.trailingComma": "none",
  // git 管理
  "git.confirmSync": false,
  "git.autofetch": true,
  // gitLens 的配置
  "gitlens.advanced.messages": {
    "suppressCommitHasNoPreviousCommitWarning": true
  },
  "gitlens.advanced.fileHistoryFollowsRenames": false,
  // 编译器缩放
  "window.zoomLevel": 0,
  "explorer.confirmDelete": false,
  // 注释翻译配置
  "commentTranslate.multiLineMerge": true,
  "commentTranslate.targetLanguage": "zh-CN",
  // Python错误检测和格式化
  "python.linting.flake8Enabled": true,
  "python.formatting.provider": "yapf",
  // 一行运行最长字符和忽略的错误检查序号
  "python.linting.flake8Args": [
    "--max-line-length=120",
    "--ignore=E501, E262, F401"
  ],
  // 两个选择器中是否换行
  "diffEditor.ignoreTrimWhitespace": false,
  // terminal默认使用zsh。我的ZSH_THEME="miloshadzic"
  "terminal.integrated.shell.osx": "/bin/zsh"
}

```
