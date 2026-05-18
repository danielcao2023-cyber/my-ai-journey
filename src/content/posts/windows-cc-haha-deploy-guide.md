---
title: "Windows 零基础国内部署 AI 编程助手（cc-haha）完整教程"
date: 2026-05-19
tags: ["教程", "工具", "Windows"]
description: "从零开始，无需科学上网，在 Windows 上部署 cc-haha（Claude Code GUI）+ DeepSeek V4 Pro 的完整指南。"
---

## 这是什么？

[cc-haha](https://github.com/NanmiCoder/cc-haha) 是一个带图形界面的 AI 编程助手，底层基于 Claude Code。它让你用中文在聊天窗口里描述需求，就能自动写代码、操作文件。

这份教程专为**完全零基础**的 Windows 用户编写，全程无需科学上网，使用国内可访问的 GitHub 镜像和 DeepSeek 模型。

> 📥 [下载完整教程（.docx）](/windows-deploy-tutorial.docx)

## 整体架构

整个部署的核心逻辑是：**cc-haha（主程序）→ LiteLLM（翻译官）→ DeepSeek（AI 大脑）**

cc-haha 默认说"Anthropic 话"，DeepSeek 说"OpenAI 话"，LiteLLM 在中间做翻译。这样你就能用支付宝/微信支付的 DeepSeek，而不需要 Anthropic 的国外信用卡。

## 教程目录

1. **前置知识篇** — 了解 cc-haha、GitHub 镜像、LiteLLM 代理的概念
2. **环境准备篇** — 安装 Git、Bun、Python 三个必需软件
3. **获取代码篇** — 通过 GitHub 镜像下载 cc-haha 源代码
4. **获取 API Key 篇** — 注册 DeepSeek 并获取密钥
5. **配置篇** — 设置 LiteLLM 代理和 cc-haha 的环境配置
6. **启动运行篇** — 双窗口启动 LiteLLM + cc-haha
7. **常见问题排查篇** — 7 个高频问题的解决方案
8. **附录** — 项目结构、有用链接、每日启动清单

## 需要准备什么

| 项目 | 说明 |
|---|---|
| 操作系统 | Windows 10 / 11 |
| 网络 | 普通国内网络即可，无需科学上网 |
| 费用 | DeepSeek API 按量计费，首次注册有免费额度，充值 10 元能用很久 |
| 时间 | 全程约 30-40 分钟 |
| 知识要求 | **零基础**，教程假设你对编程和命令行完全不了解 |

## DeepSeek 模型价格参考

| 模型 | 输入价格（每百万 token） | 输出价格（每百万 token） |
|---|---|---|
| deepseek-chat (V4 Pro) | 约 ¥2 | 约 ¥8 |
| deepseek-reasoner (R1) | 约 ¥4 | 约 ¥16 |

普通使用场景下，**10 块钱能用很久**。

## 每日启动清单

部署完成后，每次使用只需三步：

1. 打开第一个 CMD 窗口 → 启动 LiteLLM：`litellm --config litellm_config.yaml --port 4000`
2. 打开第二个 CMD 窗口 → 启动 cc-haha：`bun --env-file=.env ./src/entrypoints/cli.tsx`
3. 开始和 AI 对话

---

完整教程包含详细的图文步骤、配置文件模板和排查指南。点击下方链接下载：

> 📥 [Windows 零基础国内部署教程（.docx）](/windows-deploy-tutorial.docx)
