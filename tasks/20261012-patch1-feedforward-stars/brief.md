# Brief：前馈式 3D/4D 路线图 ★ 里程碑论文解读批量生成

> 2026-10-12 通宵任务（用户睡前下发；中途收窄范围至 ★ 节点）。本文件按全局 /goal 工作流第 2 节要求自建（用户未及喂 /goal，任务由主会话直接执行，执行经过记 report.md）。

## 1. Goal 条件

VGGT、MVSplat、L4GM、MoVieS、VG3S 五篇解读全部按 paper-template 完成并接入 index.html（卡片 read:true + 路线图节点打勾），Chrome 本地验证零报错、无占位符，全部提交并 push 到 origin/main。

## 2. 背景与目的

- **【背景】** 用户原话：「将前馈式 3D / 4D 路线图所有的论文都用模板生成一遍，生成好之后在 main 分支上提交，我要睡觉了」；随后收窄：「暂时先生成标 🌟 的这些论文吧，太费 token 了」。盘点 `index.html` 的 `.rmap-tpl[data-domain="前馈式重建"]`：共 21 个节点，已完成 4 篇（DUSt3R / MASt3R / MonST3R / DrivingRecon），未完成带 ★ 的 5 篇：路线一 VGGT（CVPR 25 最佳）；路线二 MVSplat（ECCV 24）；路线三 L4GM（NeurIPS 24）/ MoVieS（CVPR 26）/ VG3S（IROS 26，路线一×三汇合）。
- **【最终目的】** 三个路线的 ★ 里程碑全部点亮（卡片 + 路线图双勾），读者可判断值不值得复现/深入。

## 3. 交付物

- [ ] 5 个解读页（放 `scene-reconstruct/`，该目录 = 「前馈式重建」域的域目录；文件名按发表年）：
  - `2025-vggt.html`（arXiv 2503.11651 · CVPR 2025 最佳论文）
  - `2024-mvsplat.html`（arXiv 2403.14627 · ECCV 2024）
  - `2024-l4gm.html`（arXiv 2406.10324 · NeurIPS 2024）
  - `2026-movies.html`（arXiv 2507.10065 · CVPR 2026）
  - `2026-vg3s.html`（arXiv 2603.06210 · IROS 2026，venue 待源包核实）
- [ ] `index.html`：PAPERS +5 条（read:true、route/domain 归位、desc 与页内 §1 引子总结**逐字一致**）；路线图 5 个节点加 `read` 类
- [ ] 每页 Chrome 验证记录（console 零报错、无【】、目录顶部「← 返回」、图无溢出）
- [ ] `CLAUDE.md` 批次备注更新（已标列表、TODO、材料路径、教训）
- [ ] 本目录 `report.md`（验收报告）；`TASKS.md` 一行索引
- [ ] git 提交（每篇一笔）+ 最终 push origin main

## 4. 验收标准

- [ ] 每页结构 = 黄金样本 `4d-reconstruct/2024-dust3r.html`：hero / §0 速览卡（核心解读 + 五条 + 机制图 0-1 + 训练图 0-2 + 推理图 0-3 + QA 档案）/ §1 Motivation / §2 Architecture（总图 + 子节）/ §3 Loss（公式 + 逐符号）/ §4 Results（标论文表号）/ §5 Conclusion
- [ ] 全篇无「【】」占位符；`← 返回` 指向 `../index.html`
- [ ] 事实验收：数字、超参、公式全部来自 arXiv e-print 源包 / 官方代码仓，同类页有代码核对版惯例（论文与代码不一致时标注「以代码为准」）
- [ ] `index.html` 切「前馈式重建」域：路线图 5 节点绿勾、卡片 5 张绿勾、desc 与页内引子逐字一致
- [ ] `git log origin/main` 含 5 篇提交；`git status` 干净

**自检命令**：
```bash
grep -L '速览卡' scene-reconstruct/2025-vggt.html scene-reconstruct/2024-mvsplat.html scene-reconstruct/2024-l4gm.html scene-reconstruct/2026-movies.html scene-reconstruct/2026-vg3s.html   # 应无输出
grep -l '【' scene-reconstruct/*.html || echo "no placeholders"
node -e 'JSON' # (无构建链；改用 chrome-devtools console 验证)
```

## 5. 约束与禁止事项

- 🚫 不删除、不改动任何现有文件之外的既有内容；不动其他领域页面、不改既有 4 篇论文页（除 index.html/CLAUDE.md 的必要增量）
- 🚫 不 force push；公开仓库，不放私密信息
- 🚫 不编造论文数字；不确定的写「论文未说明」或标注来源级别（论文/代码）
- 🔁 网络直连不稳走代理 `127.0.0.1:7897`；失败重试 2 次后记录并继续

## 6. 自主权限与 Review 闸门

- ✅ 可自主：创建/编辑文件、chrome 本地验证、本地 git commit、`git push origin main`（用户明确要求「在 main 分支上提交」）
- ✅ 卡点处理：换代理/换源重试 → 仍不通记录到 report.md 跳过，继续下一篇
- **Review 闸门**：5 篇全部完成、push 前跑 `/code-review high` 审全批 diff（main..HEAD + 工作区），findings 逐条处理后记入 report.md（数量/严重度/修复 N/跳过 M+理由）；纯归档 commit 免审

## 7. 遗留问题与交接

→ 见 report.md（任务结束时填写）

## 8. 备注

- 材料：`/tmp/mat/{vggt,mvsplat,l4gm,movies,vg3s}/`（src/ = arXiv e-print 源码，repo/ = 官方代码浅克隆）
- 黄金样本 + 模板：`.claude/skills/paper-template/SKILL.md` + `4d-reconstruct/2024-dust3r.html`（两者冲突以 skill 为准）
- 历史教训：arXiv HTML 版公式是 SVG，pandoc 转出为噪声——公式以 e-print 源码为准；PDF 直连易截断，走代理
- 写作：全部过 renhua（说人话）+ jiangtou（讲透）原则——短句直说、符号配对写全、不留盲点
