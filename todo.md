# 博客迁移 Hugo - 待办事项

> 项目目录：`D:\work\hugo-blog`
> 已完成：Hugo 项目结构、105 篇文章转换、PaperMod 主题、GitHub Actions 配置

---

## 你需要做的 3 件事

### 1. 推送到 GitHub

打开终端，执行：

```bash
cd D:\work\hugo-blog
git add -A
git commit -m "Migrate from Jekyll to Hugo + PaperMod"
git remote add origin https://github.com/jidzhang/jidzhang.github.io.git
git push origin master --force
```

> ⚠️ `--force` 会覆盖原有仓库内容，建议先备份原仓库（如果在意的话）。
> ⚠️ 需要 GitHub Personal Access Token 或 SSH key 才能推送。

### 2. 配置 GitHub Pages

打开 https://github.com/jidzhang/jidzhang.github.io/settings/pages

- **Source** 改为 **GitHub Actions**（不再是 "Deploy from a branch"）
- 保存

### 3. 验证效果

推送后等 1-2 分钟，访问 https://jidzhang.github.io 查看。

---

## 可选：配置 SSH Key（免密码 push）

如果不想每次 push 都输 token，可以配 SSH key。明天找我帮你搞定。

## 可选：绑定自定义域名

如果有自己的域名，需要：
1. `config.yml` 里改 `baseurl` 为你的域名
2. 仓库根目录加 `CNAME` 文件，写你的域名
3. 域名商加 CNAME 记录指向 `jidzhang.github.io`
