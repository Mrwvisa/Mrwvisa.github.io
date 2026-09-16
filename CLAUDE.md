# Mrwvisa.github.io — 面试名片站（GitHub Pages 用户站）

## 项目概述

- **定位**：面试名片 + paper-explainer 论文解读作品集。简历上印的 URL：`https://mrwvisa.github.io/`
- **形态**：纯静态单文件 HTML，零依赖、零构建链（无 Jekyll/Hugo/Node），`git push` 即部署
- **远端**：`github.com/Mrwvisa/Mrwvisa.github.io`（公开仓库，Pages 从 main 分支根目录发布）
- **本地路径**：`/Users/wutao/work/github-pages/`

## 结构

```
index.html     名片首页：PROFILE（个人信息）+ PAPERS（论文列表）两个 JS 对象驱动渲染
papers/        论文解读产物（<slug>_paper_explained.html，来自 paper-explainer skill）
CLAUDE.md      本文件
```

## 设计约定

- **视觉基因**与 paper-explainer 产物（`~/work/e2e/unipad_paper_explained.html`）同源：
  GitHub (Primer) 主题，CSS 变量在 index.html `:root`（底 `#f6f8fa`、边 `#d1d9e0`、蓝 `#0969da`、绿 `#1a7f37`、橙 `#9a6700`、红 `#cf222e`），正文 14px，中文字体 PingFang SC 系
- 标签语义色轮换顺序：blue → green → orange → gray
- 改名片只动 `PROFILE`，加论文只动 `PAPERS`，不碰渲染代码

## 加一篇新论文解读的流程

1. 用 paper-explainer skill 生成 `<slug>_paper_explained.html`（产物在 `~/work/e2e/` 或其他工作目录）
2. 拷入 `papers/`
3. 在 index.html 的 `PAPERS` 数组加一个条目（file/title/meta/desc/tags/date）
4. Chrome 打开本地 index.html 验证渲染无报错，commit + push

## 注意事项

- 解读页里的数字必须有出处（config 行号/论文表号），index.html 的 desc 同样不编造数字
- 此仓库是**公开**的：不要放私密信息（真实邮箱视本人意愿、电话、内部分支名）
- Pages 构建状态：`gh api repos/Mrwvisa/Mrwvisa.github.io/pages`（看 status 字段）
- 本机 git 推送走 `gh` 的 https 凭据；若直连不稳走代理 `127.0.0.1:7897`

## TODO / 已知事项

- [ ] PROFILE 占位符（【】标出）待填真实姓名、职位方向、tagline、联系方式
- [ ] 更多论文解读陆续补充
- [ ] 可选：自定义域名（Settings → Pages → Custom domain）
