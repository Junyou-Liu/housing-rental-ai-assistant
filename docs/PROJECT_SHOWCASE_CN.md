# 项目展示说明：Housing Rental AI Assistant

## 一句话介绍

Housing Rental AI Assistant 是一个面向租房咨询场景的 RAG 智能问答系统，支持上传租赁合同、房屋规则、政策文件等知识库材料，并基于文档内容生成可追溯来源的回答。

## 项目研发出的 AI 智能助手

本项目研发的 AI 智能助手不是通用闲聊机器人，而是一个“文档驱动型租房咨询助手”。管理员可以上传 PDF、Word、TXT、Markdown、HTML 等格式的资料，系统自动解析文本、切分文档片段、生成向量嵌入并建立知识库。用户提问后，后端会先进行语义检索，找到最相关的文档片段，再将检索结果与最近对话历史一起传给大语言模型生成回答，从而降低幻觉风险，并在回复末尾展示引用文档。

## 核心功能

- 用户注册与登录：基于 JWT 的身份认证，支持普通用户和管理员角色。
- 知识库管理：管理员可上传、浏览、预览和删除文档。
- 多格式解析：支持 PDF、DOC、DOCX、TXT、Markdown、HTML。
- RAG 问答：使用 OpenAI embedding 做语义检索，再调用 GPT 生成上下文增强回答。
- 多轮会话：保存聊天会话与消息历史，回答时引入最近上下文。
- 流式响应：通过 Server-Sent Events 实现 AI 回复实时输出。
- 引用展示：回答末尾列出被检索到的来源文档名称。
- 中英文界面：前端支持 English / 中文切换。

## 技术架构

- 前端：React 19、TypeScript、Vite、Tailwind CSS、React Router、Axios、React Markdown、i18next。
- 后端：Java 17、Spring Boot 3.3、Spring Security、MyBatis、Flyway。
- AI/RAG：LangChain4j、OpenAI GPT-4o-mini、text-embedding-3-small、InMemoryEmbeddingStore。
- 数据库：MySQL，存储用户、文档元数据、会话和消息。
- 部署：Docker Compose + Nginx，前端静态服务反向代理后端 API。

## 可用于简历的项目表述

项目名称：Housing Rental AI Assistant - RAG-based Rental Consultation System

项目描述：
基于 React + Spring Boot + LangChain4j 搭建租房咨询智能问答系统，支持多格式文档上传、文本解析、向量检索、上下文增强生成、流式回答和引用文档展示，面向租赁合同、房屋规则和政策材料提供可追溯的智能问答能力。

个人贡献（如你在团队中负责前端与产品管理，可使用）：

- 负责前端页面与交互流程开发，搭建登录注册、知识库管理、会话列表和聊天问答等核心界面。
- 对接后端 REST API 与 SSE 流式接口，实现 JWT 鉴权、文档上传、会话历史加载、Markdown 回复渲染和实时输出体验。
- 参与产品流程设计，将租房咨询场景拆解为“资料上传-知识库检索-多轮问答-来源追溯”的完整用户路径。
- 配合团队完成 Docker 部署、演示材料和用户流程验证，使项目从课程原型整理为可展示的全栈 AI 应用。

## 面试讲述重点

1. 为什么使用 RAG：租房咨询依赖合同和规则文档，直接让模型回答容易产生幻觉；RAG 可以把回答限制在知识库上下文中。
2. 系统如何检索：上传文档后解析文本并切块，使用 embedding 生成向量；提问时对用户问题生成向量，取 Top-K 相关片段作为上下文。
3. 如何保证用户体验：前端支持多会话、Markdown 展示、引用文档显示和流式输出，降低等待感。
4. 工程化体现：JWT、角色权限、MyBatis 数据持久化、Flyway 数据库迁移、Docker Compose 与 Nginx 反向代理。
5. 可改进方向：将内存向量库替换为 pgvector、Milvus 或 Elasticsearch；增加文档级权限；加入更系统的 RAG 评估集和自动化测试。

## 准确性边界

- 这是团队 Capstone 项目，不建议在简历中写成个人独立完成。
- 当前代码使用内存向量库，适合课程展示和小规模 Demo；生产环境应迁移到持久化向量数据库。
- GitHub 可以展示代码、文档和构建流程；完整在线体验仍需要服务器或云平台运行后端、MySQL 和 OpenAI API。

