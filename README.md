# 图库展示站（GitHub Pages 版）

移动优先的图库展示站 + 隐藏式在线管理后台。部署在 GitHub Pages，零服务器成本。

## 目录结构

```
├── index.html      # 前台展示站（访客可见）
├── admin.html      # 隐藏后台（前台无入口，直接访问该文件路径）
├── data.json       # 全部数据（站点/Banner/分类/图片清单/联系方式/密码哈希）
├── uploads/        # 图片文件
│   ├── banner/     # 轮播大图
│   ├── menu/       # 分类封面
│   └── image/      # 图库图片
└── README.md
```

## 一、部署（一次性，约 10 分钟）

1. **建仓库**：登录 GitHub，点右上角 `+` → `New repository`，仓库名随意（如 `mysite`），选 Public，勾选 "Add a README file" 后创建。
2. **上传文件**：进入仓库 → `Add file` → `Upload files`，把本目录下的 `index.html`、`admin.html`、`data.json`、`uploads/` 整个拖进去上传（注意保持目录结构）。
3. **开启 Pages**：仓库 `Settings` → 左侧 `Pages` → `Build and deployment` 的 Source 选 `Deploy from a branch` → Branch 选 `main` / `/(root)` → Save。
4. **等待发布**：约 1 分钟后访问 `https://你的用户名.github.io/mysite/` 即可看到前台。

## 二、创建 GitHub Token（一次性）

后台通过 GitHub API 直接写仓库，需要一个只授权本站点的 Token：

1. GitHub 右上角头像 → `Settings` → 左侧最下方 `Developer settings` → `Personal access tokens` → `Fine-grained tokens` → `Generate new token`。
2. **Token name**：随意（如 `mysite-admin`）；**Expiration**：建议 90 天或更长。
3. **Repository access**：选 `Only select repositories` → 勾选刚才的仓库（如 `mysite`）。
4. **Permissions**：找到 `Repository permissions` → `Contents` 选 `Read and write`（其他保持默认）。
5. 点 `Generate token`，**立即复制**（形如 `github_pat_xxx`，只显示一次）。

## 三、登录后台

1. 访问 `https://你的用户名.github.io/mysite/admin.html`（前台没有任何入口，记住这个地址即可）。
2. 管理密码：`chiningning`（登录后可在「数据维护」里修改）。**登录只需要密码，不需要联网校验 Token。**
3. 粘贴 GitHub Token（勾选"记住"会保存在本机浏览器，下次免输）。Token 只在**保存数据 / 上传图片**时才用到。
4. 登录页有「测试 Token 连接」按钮，可随时验证 Token 是否有读写权限。
5. 仓库信息一般自动识别，无需修改；如果用了自定义域名，手动填写 Owner / 仓库名 / 分支。

## 四、日常维护

| 想做的事 | 在哪里操作 |
|---|---|
| 改网站标题 / 页脚 / SEO | 后台 → 站点设置 |
| 换顶部大图 | 后台 → 轮播 Banner（上传 / 替换 / 删除 / 排序） |
| 加分类、改分类名 | 后台 → 分类管理（支持一级 / 子分类） |
| 传图 / 换图 / 删图 | 后台 → 图片管理（批量上传，自动压缩，可拖拽） |
| 改电话微信地址 | 后台 → 联系方式 |
| 备份数据 | 后台 → 数据维护 → 导出 data.json |
| 改密码 | 后台 → 数据维护 → 修改管理密码 |

- 每次保存会自动提交到 GitHub 仓库，**约 1 分钟后前台生效**。
- 图片数量完全自由：Banner 可以没有，分类可以多可以少，每类图片 0 张也正常显示空态。

## 五、常见问题

- **登录提示"Token 权限不足"**：Token 生成时没勾选 Contents Read and write，或没勾选正确的仓库，重新生成一个。
- **前台图裂**：检查该图片是否真的在仓库 `uploads/` 对应目录下（删除图片后前台会短暂显示空图，属正常，等待发布完成）。
- **想换后台地址**：把 `admin.html` 重命名（如 `manage.html`），之后用新地址访问即可。
- **仓库太大**：后台上传会自动压缩图片（长边 1600px）；GitHub 仓库软限制 1GB，个人图库站足够。
- **Token 泄露风险**：Token 只存你的浏览器，不要提交到仓库或发给别人；泄露后到 GitHub 设置里 Revoke 即可。

## 六、安全说明

GitHub Pages 是纯静态托管，没有服务器，"隐藏后台"通过"无入口链接 + 密码 + Token"三层实现：
- 密码仅在前端校验（防随手打开的人）；
- **真正的写保护是 GitHub Token**——没有 Token 的人即使找到后台页面也改不了任何内容。
