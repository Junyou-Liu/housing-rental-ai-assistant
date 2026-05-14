# GitHub 发布步骤

当前目录已经初始化为本地 Git 仓库，并已暂存展示版项目文件。由于本机没有安装 GitHub CLI，且 Git 用户名/邮箱没有配置，远程仓库创建与提交需要你补充账号信息后完成。

## 1. 配置本仓库提交身份

在 `housing-rental-ai-assistant` 目录执行：

```bash
git config user.name "你的英文名或 GitHub 用户名"
git config user.email "你的 GitHub 邮箱或 noreply 邮箱"
```

如使用 GitHub noreply 邮箱，格式通常类似：

```text
your-github-username@users.noreply.github.com
```

## 2. 创建首个提交

```bash
git commit -m "Restore housing rental AI assistant showcase"
```

## 3. 在 GitHub 创建空仓库

建议仓库名：

```text
housing-rental-ai-assistant
```

创建时不要勾选自动生成 README、.gitignore 或 License，因为本地项目已经包含这些文件。

## 4. 绑定远程仓库并推送

把 `<your-github-username>` 替换为你的 GitHub 用户名：

```bash
git remote add origin https://github.com/<your-github-username>/housing-rental-ai-assistant.git
git push -u origin main
```

## 5. GitHub 仓库展示建议

- About 描述：`RAG-based Housing Rental AI Assistant with React, Spring Boot, LangChain4j, MySQL, and Docker.`
- Topics：`rag`, `spring-boot`, `react`, `typescript`, `langchain4j`, `openai`, `mysql`, `docker`, `capstone-project`
- README 首页保留架构图、功能说明、快速启动和 Recruiter-Facing Highlights。
- 不要上传 `.env`，也不要在 README 中暴露 OpenAI API key。

