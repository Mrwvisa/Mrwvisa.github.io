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
├── .toc        左侧领域菜单（128px sticky 侧栏，窄屏折叠为顶部横条）——由 PAPERS 的 domain 字段自动生成
└── .content
    ├── .year × N    技术路线分组（替换原年份分组；路线内按年份从近到远排）：route-title + card-grid
    │   └── .card    卡片：简短论文名（蓝）+ venue 含年份（右，mono）+ 右下角星标 + 英文全称（2行占位）+ 一句话描述
    └── footer
```

三条路线（2026-09 定，按监督空间/表征形态划分，右侧分组标题）：
1. **渲染式预训练**——掩码 + 神经渲染当解码器，训 backbone：MIM4D、UniPAD
2. **占用世界模型**——显式/隐式占用，预测未来，接规划：SparseWorld、CascadeOcc、Drive-OccWorld、DIO
3. **Gaussian 世界模型**——参数化基元当场景状态：GaussianWorld

交互：
- 打开页面**默认选中 `PAPERS[0].domain`**（当前是 4D重建）；想换默认就调 PAPERS 数组顺序
- 点卡片整卡 → 跳转论文解读页；星标 `★`：数据里 `star:true` 的默认变黄（`--star:#e3b341`），点击切换并存 localStorage 覆盖值（key `starOv`，`{file: bool}`）
- 点左侧领域 → 过滤右侧（`selectDomain()`）
- 无页头无个人信息（用户明确要求）：只有菜单 + 卡片 + 一行页脚

## 数据模型（index.html 顶部 script）

- `PAPERS` 数组，每条字段：`domain`（领域，左侧菜单）、`route`（技术路线，右侧分组，上面三条之一或新路线名）、`title`（简短名）、`full`（英文全称）、`venue`（CVPR 2024 等，含年份）、`year`（发表年份，路线内排序依据）、`file`（解读页相对路径，组织为 `<domain-en>/<year>-<slug>.html`，如 `4d-reconstruct/2024-unipad.html`）、`desc`（一句话，可选）、`star`（true = 默认星标，可选）

## 加一篇新论文解读的流程

1. 用 paper-explainer skill 生成解读 HTML，命名为 `<year>-<slug>.html`（如 `2024-omnire.html`）
2. 放入对应领域英文目录（如 `4d-reconstruct/`，新领域就新建目录），并**插入返回按钮**：在 `<nav class="toc">` 内、`<h3>` 上方插一行「← 返回」（`href="../index.html"`，样式内联，参照 2024-unipad.html 目录顶部现成的那行）
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

- [ ] 更多领域随解读补充（PAPERS 里有注释示例条目；当前 4D重建 域 7 篇：UniPAD/MIM4D/GaussianWorld/Drive-OccWorld/DIO/CascadeOcc/SparseWorld）
- [ ] 可选：自定义域名

## 2026-09 批次备注（paper-explainer 批量产出）

- 6 篇解读由主会话直接生成（2026-09-21）：MIM4D/GaussianWorld（代码核对版）、Drive-OccWorld/DIO/CascadeOcc（纯论文模式，无代码或仓库仅 README）、SparseWorld（论文为主）
- 材料存 ~/work/e2e/explainers-202609/（各论文 PDF/txt + 代码仓），规格书 SPEC.md 同目录
- 教训：后台 agent 批量生成会被 600s 流看门狗反复掐死（本环境网络流不稳），一夜零产出；改为主会话亲自分段生成后单日完成 6 篇。再跑批量时直接用主会话 + 分段落盘（Write 首段 → bash heredoc 追加 ≤12KB/段）
- push 直连不稳时走代理：git -c http.proxy=http://127.0.0.1:7897 push；Pages 构建状态轮询 gh api repos/Mrwvisa/Mrwvisa.github.io/pages --jq .status（注意 built 可能是上一次构建的旧状态，push 后 sleep 20 再轮询）
- 每篇验证流程：chrome-devtools 打开本地文件 → console 零报错（file:// 的 unique origin 报错是 MCP 工具噪声可忽略）→ flowrow 卡片数/无溢出 → 无【】占位 → 目录顶部「← 返回」存在 → push 后 curl 线上 200
