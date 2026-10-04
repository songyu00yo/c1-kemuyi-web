# C1 手动挡科目一通关助手

这是一个 C1 科目一刷题和模拟考试工具。前端使用原生 HTML、CSS 和 JavaScript，桌面版使用 Electron。

[![version](https://img.shields.io/badge/version-1.0.0-blue.svg)](https://github.com/songyu00yo/c1-kemuyi-web)
[![license](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
![platform](https://img.shields.io/badge/platform-Windows%20x64%20%7C%20Web-lightgrey)
![Electron](https://img.shields.io/badge/Electron-43-47848F)

仓库内置 2,194 道全国通用单选题和判断题，其中 787 道带图片。桌面版使用本地题库和图片，Electron 会拦截 HTTP/HTTPS 请求。

学习档案、错题和模拟考试记录保存在当前设备的 `localStorage`。

## 功能

- 顺序练习：按题库顺序做题，并保存当前位置。
- 随机练习：随机打乱题目顺序。
- 模拟考试：随机抽取 100 题，限时 45 分钟，90 分及格。
- 本地档案：支持多个昵称档案，分别保存做题进度、正确次数、错题和考试记录。
- 离线桌面版：题库和 787 张题图随应用打包，不依赖在线接口。

## 浏览器运行

需要 Node.js 和 npm。

```bash
git clone https://github.com/songyu00yo/c1-kemuyi-web.git
cd c1-kemuyi-web
npm run dev
```

默认地址是 <http://127.0.0.1:4173>。可以用环境变量 `PORT` 修改端口。

浏览器版会优先读取 `src/data/questions.offline.json`。如果该文件不可用，才回退到 `src/data/questions.json`。

## Windows 桌面版

仓库目前没有可直接下载的 GitHub Release。需要 Windows 安装包时，可以在本地构建。

先安装依赖：

```powershell
npm install
```

启动 Electron 开发版：

```powershell
npm run desktop:dev
```

生成 Windows x64 安装包：

```powershell
npm run dist:win
```

构建命令会依次生成图标、准备离线题图、检查离线资源、运行测试，再用 `electron-builder` 生成 NSIS 安装程序。安装包输出到 `release/`。

如果系统缺少离线题图，`offline:prepare` 会从原题图地址下载缺失文件，因此这一步需要网络。

生成的安装程序没有商业代码签名证书。Windows SmartScreen 可能显示“未知发布者”或“Windows 已保护你的电脑”。

`release/SHA256SUMS.txt` 用于校验构建产物。

## 题库与离线资源

原始题库位于 `src/data/questions.json`。`scripts/prepare-offline.mjs` 会筛选全国通用科目一题目，并把远程题图改成本地路径。

离线检查要求：

- 题目总数为 2,194。
- 带图题目为 787 道。
- 离线题库中不能保留 HTTP/HTTPS 地址。
- 每张题图都必须存在，且文件大小大于 100 字节。

运行检查：

```bash
npm run offline:check
```

## 测试

```bash
npm test
```

当前测试入口为 `tests/run-tests.mjs`，会检查题库、存储、模拟考试、静态资源和 Electron 桌面配置。

## 项目结构

```text
c1-kemuyi-web/
├── index.html
├── server.mjs
├── package.json
├── desktop/
│   └── main.cjs
├── src/
│   ├── css/
│   ├── js/
│   ├── data/
│   └── assets/question-images/
├── scripts/
├── tests/
├── build/
└── release/
```

`release/*.exe` 不提交到 Git。`release/` 中只保留校验文件和安装说明等文本文件。

## 技术栈

| 技术 | 用途 |
| --- | --- |
| HTML、CSS、JavaScript | 页面、练习逻辑和本地状态 |
| Node.js | 本地开发服务器和构建脚本 |
| Electron 43 | Windows 桌面运行时 |
| electron-builder 26 | NSIS 安装包构建 |

## 许可证

代码依据 [MIT License](LICENSE) 发布。
