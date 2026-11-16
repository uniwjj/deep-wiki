---
title: AI 输出格式阶梯：从「说出来」到「做出来」
description: Karpathy 提出的输出介质升级路径——文字 → 图 → 网页 → 视频，每上一级理解成本更低；终点是可丢弃的软件制品，判断标准从「这东西值不值得做」变成「能不能帮我把它想明白」
aliases: [输出格式阶梯, output format ladder, discardable software artifacts, 可丢弃的软件制品, Karpathy 输出格式]
tags: [ai-agent, concept, practice]
sources: [2026/11/13/Karpathy大神又发新文！这次是教会你用AI学习……/index.html]
created: 2026-11-13
updated: 2026-11-13
---

# AI 输出格式阶梯：从「说出来」到「做出来」

同一个问题，让 AI 直接说给你听，和让 AI 做给你看，是两种量级不同的结果。Karpathy 这条推文表面上在分享几个提示词技巧，实际给出的是一条**输出介质的升级阶梯**：文字 → 图 → 网页 → 视频，每往上一级，人付出的理解成本就低一分。

> 本页依据的是微信公号文章对这条推文的转述与解读（原文含引文与翻译），不是推文原文本身。文中标注为「原文说」的内容出自该文引用，其余为文章的整理，本页已分开标注。

## 前提：未来大部分时间花在「看懂 AI 的产出」上

推文开篇定调：

> We'll be spending a lot more time trying to understand the outputs of language models.

模型越强，它自主完成的活儿越多，人的工作就越往上游走，变成**监督（oversight）与理解（understanding）**：不必亲手写每一行，但必须看懂它在干什么。问题在于「看懂」本身有成本——四个技巧本质上都在解决同一件事：怎样让 AI 把同一个内容表达成你更容易吸收的形式。

## 四级阶梯

| 级别 | 介质 | 为什么理解成本更低 | 适用内容 |
|------|------|-------------------|---------|
| 1 | 干净的文字 | 歧义被压到最低，不需要反复重读 | 技术原理、操作规程、避坑指南 |
| 2 | 图 | 文字是线性的（必须从头读到尾），图是并行的（层级、依赖、分支同时进入视野） | 结构与层级，而不是一条流程 |
| 3 | 可交互网页 | 文字和图都是「读」的，网页是「操作」的 | 参数敏感型问题，改一个系数看整条曲线怎么变 |
| 4 | 讲解视频 | 动态、有时间轴的内容用运动表达 | 抽象数学、底层算法、高维拓扑概念 |

### 第一级：让文字先「干净」下来

做法是让模型**用 ASD-STE100 来解释**。ASD-STE100 全称 Simplified Technical English（简化技术英语），是一套受控语言规范：上世纪 70 年代末由欧洲航空航天工业协会（AECMA，今 ASD）应飞机制造商之需开发——飞机维修手册里一句话理解错，后果是灾难性的，所以必须有一套硬性规则把歧义压到最低。

这套规范的核心约束（文章列的六条）：

- 一句话只讲一件事：程序性句子不超过 20 个词，描述性句子不超过 25 个词
- 一个词只有一个意思：词典里约 900 个词被批准使用，另有约 1200 个词被明确禁用并给出替代词
- 用主动语态，不用被动语态
- 一段只讲一个主题：描述性段落不超过 6 句

对 LLM 有效的原因：**「请写清楚一点」是模糊指令，「按 ASD-STE100 写」是成文、公开、有条目的硬约束**，模型训练数据里见过大量这类文本，所以真能照做。实操上不必逐条卡死，Karpathy 自己常用的是「80% of the way to ASD-STE100」——八成符合就够了。

### 第二级：别让它写，让它画

原文理由是这些内容 "can be a lot easier to process, parse, and understand"。文字必须顺序解码，图可以一眼看见结构。所以当要理解的是**结构**而不是**流程**时，图几乎是碾压性选择。

### 第三级：要一个能点的网页

原话很朴素：Ask for output "in HTML"。一句「用 HTML 输出」，拿到的不是描述，而是能打开的网页——可拖的滑块、可点的按钮、会动的动画。质变在于交互：面对参数敏感的问题，写十段文字解释不如给一根滑杆自己拖两下。Karpathy 顺带判断：LLM 的前端能力正在快速变好，已经能做出 "beautiful" 的体验和动画。

### 第四级：要一支讲解视频

这是 Karpathy 明确说最看好的输出格式（the output format I am most bullish on）：针对任何临时想到的题目，现做一支完全定制的讲解视频。原文示范：

```
Create a 3b1b style video explainer on X. Use my ElevenLabs API key for audio narration.
```

（3b1b 指 3Blue1Brown——Grant Sanderson 用动画讲数学的频道。）做法是让模型写 Manim 动画脚本（3Blue1Brown 本人就用它做视频），再配语音旁白；旁白需 TTS 服务，推文提的是 ElevenLabs 的 API key，没有则让模型自己找跑在本地算力上的免费替代。整条推文最有分量的是收尾那句感叹：

> This is actually starting to work!

## 可复制的三个提示词后缀

文章把上面的阶梯整理成可直接粘贴的版本，并说明：**推文原话只到「in HTML」和「3b1b style … ElevenLabs API key」，下面第二、三条是文章补全成可执行的完整指令**，不是推文原文。

```
# ① 文本极简
Explain this in ASD-STE100, or 80% of the way to ASD-STE100 specification.

# ② 交互网页（单文件）
Output as a single self-contained HTML file with Tailwind CSS and interactive JS sliders/animations, no build step.

# ③ 动态讲解视频
Create a 3b1b-style video explainer on this topic using Manim Python script. Use my ElevenLabs API key for audio narration or suggest a local TTS alternative.
```

## 落点：可丢弃的软件制品

前面是技巧，这条推文的落点是判断。原文：

> As intelligence and code are increasingly abundant, you can ask for large, custom, discardable software artifacts (e.g. web apps, video explainers) that would have never made sense to create before.

翻译过来：当智能和代码都变得过剩，你就可以索要那种大型的、定制的、用完就扔的软件制品——网页应用也好、讲解视频也好——而在过去，做这些东西根本不合理。

「不合理」在过去是因为成本：做一个小工具要先立项、选型、配环境、写、测、维护，成本远大于收益，所以「为了搞懂一个问题专门做一个软件」从来不划算。现在制作成本趋近于零，这笔账反过来，判断标准随之改变：

| 以前 | 现在 |
|------|------|
| 这东西值不值得做？（不值得，成本太高） | 这东西能不能帮我现在把这件事想明白？（能，那就做） |

**「用完就扔」不是浪费**：它本来就是一次性的理解工具，做出来是为了看懂，看懂了使命就结束。这也接回开篇那句——人的工作往上升变成监督与理解，而理解恰恰最稀缺，所以让 AI 批量制造理解工具是当前性价比最高的用法。

文章的收尾数据（发布一天多）：552 万次浏览、4.69 万次点赞、5,335 次转发，而收藏 6.76 万**高于**点赞——「收藏 > 点赞」是典型工具帖特征：不是觉得说得好，是打算回头照着做。

## 三个可立刻用起来的动作

1. 下次提问别只描述任务，**指定输出格式**：结尾加一句「用 HTML 输出」或「用图说明」，成本一个句子。
2. **按内容类型选格式**：流程与规范 → ASD-STE100；结构与层级 → 图；调参数看变化 → 网页；抽象、动态、有时间轴 → 视频。
3. **允许自己做一个用完就扔的东西**：花十分钟生成一个只服务当前问题的网页，看完删掉——把算力换成理解力。

## 与本站其他主题的关系

- 这条阶梯讲的是**输出侧**的上下文工程；输入端如何组织材料见 [[prompt-engineering]]。
- 「做完就删」与 Harness 的 Build-to-Delete 是同一个思路的两个尺度：那边删的是工程脚手架，这边扔的是一次性理解制品，见 [[harness-build-to-delete]]。
- 一次性网页/视频的单价与可行性，取决于推理成本，见 [[token-consumption-economics]]。

## 相关页面

- [[prompt-engineering]] — 提示词工程：指令如何写得更可控
- [[harness-build-to-delete]] — 该删的不是模型能力，是补丁式脚手架
- [[token-consumption-economics]] — Token 成本结构决定「造一个一次性制品」是否划算
- [[ai-native-team-refactor]] — 同一个趋势的组织侧表现：单点提效、全局不增

## 边界与留白

- 本页未收录推文的互动数据之外的时间线信息；推文原文的点赞/收藏数字由文章给出，未独立复核。
- ASD-STE100 的六条约束是文章转述的版本，规范全文见 asd-ste100.org（文章注明可免费下载），本页未逐条核对原文条款。
