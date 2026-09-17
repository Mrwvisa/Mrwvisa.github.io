# Mrwvisa.github.io — 论文解读作品集（GitHub Pages 用户站）

## 项目概述

- **定位**：面试名片 + paper-explainer 论文解读库。简历上印的 URL：`https://mrwvisa.github.io/`
- **形态**：纯静态单文件 HTML，零依赖、零构建链（无 Jekyll/Hugo/Node），`git push` 即部署
- **远端**：`github.com/Mrwvisa/Mrwvisa.github.io`（公开仓库，Pages 从 main 分支根目录发布）
- **本地路径**：`/Users/wutao/work/github-pages/`

## 页面结构（index.html）

左侧领域菜单 + 右侧年份分组卡片（卡片布局词汇来自 `~/work/workbench` 的 ExperimentCard，主题沿用 paper-explainer 的 GitHub Primer 基因）：

```
.layout
├── .toc        左侧领域菜单（128px sticky 侧栏，layout 左留白 60px ≈ 原居中版的一半；单级菜单比论文页 178px 窄；窄屏折叠为顶部横条）——由 PAPERS 的 domain 字段自动生成
└── .content
    ├── .year × N    年份分组（降序）：year-title + card-grid
    │   └── .card    卡片：简短论文名（蓝）+ venue（右，mono）+ 右上角星标圆钮 + 英文全称（2行占位）+ 一句话描述
    └── footer
```

交互：
- 打开页面**默认选中 `PAPERS[0].domain`**（当前是 4D重建）；想换默认就调 PAPERS 数组顺序
- 点卡片整卡 → 跳转论文解读页；星标 `★` 点击变黄（`--star:#e3b341`），状态存 localStorage（key `stars`，值为 file 路径数组）
- 点左侧领域 → 过滤右侧（`selectDomain()`）
- 无页头无个人信息（用户明确要求）：只有菜单 + 卡片 + 一行页脚

## 数据模型（index.html 顶部 script）

- `PAPERS` 数组，每条字段：`domain`（领域，新值自动出现在左侧菜单）、`title`（简短名）、`full`（英文全称）、`venue`（CVPR 2024 等）、`year`（发表年份，分组依据）、`file`（解读页相对路径，组织为 `<domain-en>/<year>-<slug>.html`，如 `4d-reconstruct/2024-unipad.html`）、`desc`（一句话，可选）

## 加一篇新论文解读的流程

1. 用 paper-explainer skill 生成解读 HTML，命名为 `<year>-<slug>.html`（如 `2024-omnire.html`）
2. 放入对应领域英文目录（如 `4d-reconstruct/`，新领域就新建目录），并**插入返回按钮**：在 `</body>` 前插入一行（固定右上角浮动 pill，`href="../index.html"`，样式内联，参照 2024-unipad.html 里现成的那行）
3. index.html 的 `PAPERS` 数组加一个条目（domain 中文领域名、file 填相对路径）
4. Chrome 本地验证（console 零报错、星标点击、领域切换），commit + push

## 设计约定

- GitHub (Primer) 主题，CSS 变量在 index.html 与各论文页 `:root`（底 `#f6f8fa`、边 `#d1d9e0`、蓝 `#0969da`、星标黄 `#e3b341`），正文 14px
- 论文页保持 skill 原生产物不动，唯一允许的改动是插入返回按钮
- index.html 的 desc 不编造数字（同 paper-explainer 契约）
- **卡片 desc 的一句话总结必须与论文页 §1 引子的总结逐字一致**（用户要求两边对得上；引子段末尾「整套方法概括起来就是：…」）

## 注意事项

- 此仓库是**公开**的：不放私密信息
- Pages 状态：`gh api repos/Mrwvisa/Mrwvisa.github.io/pages`（看 status 字段）
- 直连不稳走代理 `127.0.0.1:7897`

## TODO / 已知事项

- [ ] 更多领域随解读补充（PAPERS 里有注释示例条目；当前仅 4D重建/UniPAD）
- [ ] 可选：自定义域名
