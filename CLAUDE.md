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

路线分组（右侧分组标题，按监督空间/表征形态划分）：
— 4D重建 域（2026-09 定）：
1. **渲染式预训练**——掩码 + 神经渲染当解码器，训 backbone：MIM4D、UniPAD
2. **占用世界模型**——显式/隐式占用，预测未来，接规划：SparseWorld、CascadeOcc、Drive-OccWorld、DIO
3. **Gaussian 世界模型**——参数化基元当场景状态：GaussianWorld
— 前馈式重建 域：
4. **前馈几何基座**（原「点图回归」，2026-10-11 与路线图对齐改名）——每像素直接回归 3D 点、两张图共用一个坐标系，对应关系是副产品：DUSt3R（同日从 4D重建 域移入，置数组首位，保证路线序：路线一在前）
5. **前馈 4D 高斯**（原「前馈泛化重建」，同日对齐路线三改名）：DrivingRecon

交互：
- 打开页面**默认选中 `PAPERS[0].domain`**（当前是 前馈式重建，DUSt3R 在数组首位；该域 2026-10 由「场景级重建」改名）；想换默认就调 PAPERS 数组顺序
- **领域专属路线图（2026-10 新增）**：每个领域可配自己的路线图，切领域时跟着换——机制是 body 里 `<template class="rmap-tpl" data-domain="领域名">` 存 HTML，`renderGroups()` 开头按 `currentDomain` 查询并注入 `#groups` 顶部；没配的领域不显示。当前仅「前馈式重建」配了三路线时间轴（前馈几何基座 DUSt3R→VGGT→…／前馈 3D 高斯 SplatterImage→MVSplat→…／前馈 4D 高斯 L4GM→VG3S 汇合），样式类 `.rmap/.mtl/.mnode`（节点上下交替挂标签，`.mtl` 高度须容纳三行标签否则溢出遮挡，现 134px）；新领域配图复制一个 template 改 data-domain 即可
- **已读标记（2026-10-11 改版，只读）**：读者读完告知 Claude → Claude 在源码标（PAPERS 条目加 `read: true`；路线图节点加 `read` 类）→ 页面只读展示（卡片绿 ✓、节点带勾绿点），**访问者不能点击/更改**；旧的 localStorage 点选机制（readOv）已移除，别再实现前端切换。当前已标：DUSt3R、DrivingRecon
- 点卡片整卡 → 跳转论文解读页；星标 `★`：数据里 `star:true` 的默认变黄（`--star:#e3b341`），点击切换并存 localStorage 覆盖值（key `starOv`，`{file: bool}`）
- 点左侧领域 → 过滤右侧（`selectDomain()`）
- 无页头无个人信息（用户明确要求）：只有菜单 + 卡片 + 一行页脚

## 数据模型（index.html 顶部 script）

- `PAPERS` 数组，每条字段：`domain`（领域，左侧菜单）、`route`（技术路线，右侧分组，上面三条之一或新路线名）、`title`（简短名）、`full`（英文全称）、`venue`（CVPR 2024 等，含年份）、`year`（发表年份，路线内排序依据）、`file`（解读页相对路径，`<目录>/<year>-<slug>.html`；目录按页面最早归属的域命名，**不强制与 domain 字段一一对应**——如「前馈式重建」域两篇分处 `scene-reconstruct/` 与 `4d-reconstruct/`）、`desc`（一句话，可选）、`star`（true = 默认星标，可选）

## 加一篇新论文解读的流程

0. **先读 `paper-template` skill（论文模板）**（`.claude/skills/paper-template/SKILL.md`），全程按其结构与规范做
1. 用 paper-explainer skill 生成解读 HTML（作为底稿），命名为 `<year>-<slug>.html`（如 `2024-omnire.html`）
2. 放入对应领域英文目录（如 `4d-reconstruct/`，新领域就新建目录），并**插入返回按钮**：在 `<nav class="toc">` 内、`<h3>` 上方插一行「← 返回」（`href="../index.html"`，样式内联，参照 2024-unipad.html 目录顶部现成的那行）
3. index.html 的 `PAPERS` 数组加一个条目（domain 中文领域名、file 填相对路径）
4. Chrome 本地验证（console 零报错、星标点击、领域切换），commit + push

## 设计约定

- **解读的验收线（2026-10-11 用户点名的目的）**：读者是来看「值不值得复现、值不值得深入」的——任何表述不允许留思维盲点：怎么做、好细节在哪、坑在哪都要显式写清；符号（X¹,¹ 等）、输入输出尺寸、配对（谁对谁）、归属（在哪个坐标系）等锚点必须落地，读者靠它们把正文 / 图 / 结果串起来（配合 renhua skill 的完整性铁律）
- GitHub (Primer) 主题，CSS 变量在 index.html 与各论文页 `:root`（底 `#f6f8fa`、边 `#d1d9e0`、蓝 `#0969da`、星标黄 `#e3b341`），正文 14px
- 论文页 = paper-explainer 原生产物作底稿，按 `paper-template` 模板改造（速览卡、训练/推理双图、Loss 写法等）；「← 返回」按钮插入照旧
- **论文解读模板 = `paper-template` skill（2026-10-11 定稿，写/改论文页前必读）**：速览卡结构（核心思想 + 目标/训练/推理/效果/结论五条 + 机制总图 + QA 档案）、训练/推理各画一张流程图且放进各自条目、Loss 公式进文字并逐符号解释、shape 与记号锚点、图内只放符号、去 AI 腔——**全部细则以 `.claude/skills/paper-template/SKILL.md` 为准**；黄金样本 = `4d-reconstruct/2024-dust3r.html`。项目特有补充：§0 速览卡整区极浅黄底（`--c-scan-bg` #fffbe9 + `--c-scan-line` 边框，便签纸感）与下方白色逐节区分；五步动图已删（2026-10-11 用户拍板）；QA 档案只收 Q 与讲透后的 A，盲点原始数据只进 `jiangtou` 的 blindspots-log 不上仓库；旧页是否按新模板改造待定
- **对话记录归档（2026-10-11 起）**：每篇论文的完整对话记录（用户消息 + Claude 回复正文，工具细节略去）存到该论文所在目录的 `memory/<year>-<slug>.md`（与页面同名，如 `4d-reconstruct/memory/2024-dust3r.md`，131KB 量级）；会话结束前由 Claude 从会话记录文件导出/更新；**随代码一起提交上传**——将来分支删掉，记录仍在远端可访问。所有论文都按此规则（各自目录下的 memory/）
- **文案风格（2026-10-11 用户点名）**：所有页面文案与对话表述先过全局 `renhua` skill（说人话）：短句直说、去修辞、因果明写，写完念一遍。与 `jiangtou` 配套——jiangtou 管讲解顺序，renhua 管文字形状
- index.html 的 desc 不编造数字（同 paper-explainer 契约）
- **卡片 desc 的一句话总结必须与论文页 §1 引子的总结逐字一致**（用户要求两边对得上；引子段末尾「整套方法概括起来就是：…」）

## 注意事项

- 此仓库是**公开**的：不放私密信息
- Pages 状态：`gh api repos/Mrwvisa/Mrwvisa.github.io/pages`（看 status 字段）
- 直连不稳走代理 `127.0.0.1:7897`

## TODO / 已知事项

- [ ] 更多领域随解读补充（PAPERS 里有注释示例条目；4D重建 域 7 篇：UniPAD/MIM4D/GaussianWorld/Drive-OccWorld/DIO/CascadeOcc/SparseWorld；前馈式重建 域 2 篇：DrivingRecon、DUSt3R）
- [ ] 可选：自定义域名

## 2026-09 批次备注（paper-explainer 批量产出）

- 6 篇解读由主会话直接生成（2026-09-21）：MIM4D/GaussianWorld（代码核对版）、Drive-OccWorld/DIO/CascadeOcc（纯论文模式，无代码或仓库仅 README）、SparseWorld（论文为主）
- 材料存 ~/work/e2e/explainers-202609/（各论文 PDF/txt + 代码仓），规格书 SPEC.md 同目录
- 教训：后台 agent 批量生成会被 600s 流看门狗反复掐死（本环境网络流不稳），一夜零产出；改为主会话亲自分段生成后单日完成 6 篇。再跑批量时直接用主会话 + 分段落盘（Write 首段 → bash heredoc 追加 ≤12KB/段）
- push 直连不稳时走代理：git -c http.proxy=http://127.0.0.1:7897 push；Pages 构建状态轮询 gh api repos/Mrwvisa/Mrwvisa.github.io/pages --jq .status（注意 built 可能是上一次构建的旧状态，push 后 sleep 20 再轮询）
- 每篇验证流程：chrome-devtools 打开本地文件 → console 零报错（file:// 的 unique origin 报错是 MCP 工具噪声可忽略）→ flowrow 卡片数/无溢出 → 无【】占位 → 目录顶部「← 返回」存在 → push 后 curl 线上 200

## 2026-10 批次备注

- DUSt3R 解读（代码核对版）：材料在 /tmp/dust3r/（paper.txt=arXiv v2 全文转纯文本 + naver/dust3r 浅 clone）；论文正文表 1~4 + 附录表 5~8 的数字已逐条核对（表 7 是三段训练课程 224-linear 50ep → 512-linear 100ep → 512-dpt 90ep，别写成两阶段）
- chrome-devtools MCP 报 "browser already running" 时：先 `pkill -f "chrome-devtools-mcp/chrome-profile"` 清残留实例再 new_page（只杀 MCP 专用 profile，不影响用户自己的 Chrome）
