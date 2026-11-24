# JCST 静态索引站 · GitHub Pages 发布说明

本目录只包含网页运行必需的 5 个文件，可直接上传到 GitHub 作为静态站点：

| 文件 | 作用 |
|---|---|
| `index.html` | 网页主文件（含全部 HTML/CSS/JS） |
| `articles.js` | 文章数据（`window.JCST_ARTICLES`），网页实际读取的数据源 |
| `topic_groups.js` | 领域→主题映射（`window.JCST_TOPIC_GROUPS`），用于主题下拉框分组显示，**必需** |
| `articles.json` | 同内容的 JSON 备份，便于后续维护/核对，网页不直接引用 |
| `jcst_logo_transparent.png` | 顶部透明 logo |

> 备份脚本、各类 Excel、临时目录等**不要**上传（含内部复核记录）。

---

## 方法一：GitHub 网页端上传（最简单，无需命令行）

1. 登录 GitHub → 右上角 `+` → **New repository**。
2. 仓库名二选一：
   - 想用根域名 `https://你的用户名.github.io/`：仓库名填 `你的用户名.github.io`
   - 想用子路径 `https://你的用户名.github.io/jcst/`：仓库名填 `jcst`（或任意名字）
3. 勾选 **Public**，不要勾选 "Add a README file"（保持空仓库），点 **Create repository**。
4. 在仓库页面点 **uploading an existing file**（或 "Add file → Upload files"），把这 5 个文件全部拖进去（含 `topic_groups.js`，漏掉会导致主题下拉框分组失效）。
5. 点 **Commit changes**。
6. 进入 **Settings → Pages**（左侧栏）。
7. **Build and deployment → Source** 选 **Deploy from a branch**；Branch 选 `main`，文件夹选 `/ (root)`，点 **Save**。
8. 等 1–2 分钟刷新，页面顶部会出现访问链接：
   - 根域名仓库：`https://你的用户名.github.io/`
   - 子路径仓库：`https://你的用户名.github.io/仓库名/`

## 方法二：Git 命令行推送（便于以后更新）

```bash
# 1. 克隆空仓库（仓库地址在 GitHub 页面 Code → HTTPS 处复制）
git clone https://github.com/你的用户名/jcst.git

# 2. 把本目录 5 个文件复制进克隆下来的文件夹（含 topic_groups.js）

# 3. 进入文件夹并提交
cd jcst
git add .
git commit -m "Publish JCST article index"
git push -u origin main
```

之后在 **Settings → Pages** 按方法一的第 7 步开启即可。以后数据更新：改完 `articles.js` / `index.html` 后重新 `git add . && git commit -m "update" && git push`。

---

## 提示

- 网页顶部右侧是 **Download &#9662;** 下拉菜单，含 CSV / Excel / JSON / BibTeX / RIS 五项，导出的是**当前筛选结果**。其中 `Excel` 生成带格式的 `.xlsx`（故宫红表头加粗、冻结首行、按内容自动列宽与行高、摘要自动换行、带筛选器）；CSV / Excel / JSON 三种全字段导出均含 **IEEE / ACM / JCST 三种引用文本**（与卡片内复制按钮内容一致，分别位于表格末三列、JSON 的 `citation_ieee` / `citation_acm` / `citation_jcst` 字段）；`BibTeX` / `RIS` 生成批量引文文件（`.bib` / `.ris`），字段与文章卡片上的单篇导出一致，BibTeX 引用键在批量导出时自动去重。全部由网页内置生成器完成，**无外部库依赖**，无需额外上传文件。点选后菜单自动收起；再次点击按钮或点击页面其他位置可关闭菜单。文章卡片内另有单篇 **RIS / BibTeX** 按钮，以及 IEEE / ACM / JCST 三种引文格式的复制按钮。三种引文在网页上**期刊名显示为斜体**（符合 IEEE / ACM / JCST 样式）；复制采用富文本+纯文本双格式写入剪贴板——粘贴到 Word / WPS 等富文本编辑器保留斜体，粘贴到纯文本环境（记事本、命令行等）自动降级为纯文本，不含任何标记。导出文件中的三种引用文本始终为纯文本，不含斜体标记。
- 网页里所有资源都用**相对路径**引用（`articles.js`、`topic_groups.js`、`jcst_logo_transparent.png`），所以放在仓库子路径下也能正常显示，无需改代码。
- 主题下拉框按官方领域分组显示：分组定义在 `topic_groups.js`。交叉主题会同时出现在其涉及的多个领域下（网上任一组选中都能筛到该主题）。如需调整归属，改 `build_topic_groups.py` 里的 GROUPS 后重跑，会同步重建 `topic_groups.js` / `.json` 和 `topic_domains.json`。
- 若更新后页面没变化，多为浏览器缓存，按 `Ctrl+F5` 强制刷新；或等 GitHub Pages 重新构建（约 1 分钟）。
- 如果只想要一个可访问链接、不介意非 GitHub 域名，也可以用 WorkBuddy 的"发布为应用"直接生成分享链接。
