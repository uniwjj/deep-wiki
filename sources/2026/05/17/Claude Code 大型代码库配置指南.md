---
ingested: 2026-05-17
wiki_pages: [ai-agent/claude-code/claude-code-configuration-guide]
---

# Claude Code 大型代码库配置指南

> 整理自 Anthropic 官方文章 _How Claude Code works in large codebases: Best practices and where to start_（2026-05-14）

---

## 一、核心原则：Agentic Search，不是 RAG

Claude Code 不像其他 AI 工具那样对整个代码库做 embedding 索引。它像工程师一样工作——遍历文件系统、读文件、grep、跟踪引用。

- **好处**：永远工作在实时代码上，没有索引过期的问题
- **代价**：必须有足够的起始上下文让它知道往哪找，否则几十亿行代码中盲目搜索会直接撞上下文窗口

**代码库配置的质量直接决定了 Claude Code 的能力上限。**

---

## 二、五层扩展架构（按顺序建设，不要跳）

```
CLAUDE.md  →  Hooks  →  Skills  →  Plugins  →  MCP
  (基础)      (自动化)   (按需加载)   (分发)      (外部连接)
```

每层依赖前一层。**最常见错误**：基础没搭好就急着搞 MCP。

### 第 1 层：CLAUDE.md —— 代码库的"说明书"

**加载机制**：Claude 从当前目录向上遍历，加载路径上所有 CLAUDE.md，叠加生效。

**根目录 CLAUDE.md**：只放全局信息

```markdown
# 项目概述
这是一个电商平台单体仓库，包含 12 个微服务，主要语言 Java + TypeScript。

# 关键约定
- 所有 API 遵循 RESTful 规范，路径前缀 /api/v2/
- 数据库迁移用 Flyway，脚本放在各服务的 db/migration/
- 禁止直接引入第三方 HTTP 客户端，统一用 shared/http-client

# 常见坑点
- shared/ 目录的改动会影响所有服务，务必跑全量测试
- 生产配置在 config-prod/ 下，不要修改 config-dev/ 假定生效
```

**子目录 CLAUDE.md**：放该模块特有的命令和约定

```markdown
# payment-service 特有信息
- 构建：./gradlew :payment-service:build
- 单元测试：./gradlew :payment-service:test
- 集成测试：./gradlew :payment-service:integrationTest
- 本地启动：docker-compose -f payment-service/docker-compose.yml up

# 注意事项
- 退款逻辑涉及状态机，修改 OrderState 前务必检查所有转换路径
- 金额计算用 BigDecimal，禁止用 float/double
```

**关键原则**：
- 根文件只放**指针和关键坑点**，其余都是噪音
- **不要在子目录的 CLAUDE.md 重复根文件的内容**
- 每行内容问自己：这对所有任务都适用吗？如果只适用特定任务类型，应该放 Skill 里

---

### 第 2 层：Hooks —— 让配置自我进化

**自动化执行**（确定性行为，不要靠 Claude 记忆）：

```json
{
  "hooks": {
    "preToolUse": [
      {
        "matcher": "Write|Edit",
        "command": "p4 edit \"$CLAUDE_FILE_PATH\""
      }
    ],
    "postToolUse": [
      {
        "matcher": "Write|Edit",
        "command": "./scripts/run-lint.sh \"$CLAUDE_FILE_PATH\""
      }
    ]
  }
}
```

**持续改进**（更高价值的使用方式）：
- **Stop hook**：会话结束时，趁上下文新鲜，自动分析本次会话发现的问题，提议更新 CLAUDE.md
- **Start hook**：根据开发者所在的团队/模块，动态注入对应上下文，无需手动配置

---

### 第 3 层：Skills —— 按需加载专业知识

核心机制是**渐进式披露**：大代码库有几十种任务类型，不需要每次会话都加载全部专业知识。

**按路径限定生效范围**：

```yaml
# .claude/skills/payment-deploy.md
---
scope: ["payment-service/**"]
---
# 支付服务部署流程
1. 确认灰度比例：prod-canary 配置
2. 执行部署：./deploy.sh payment-service <version>
3. 监控验证：检查 payment-service 的 QPS 和错误率
```

这个 skill 只在操作 `payment-service/` 路径时加载，其他模块完全不受影响。

**典型 Skill 类型**：

| 场景 | Skill 内容 |
|------|-----------|
| 安全审查 | 漏洞检测规则、常见安全问题 checklist |
| 文档更新 | API 文档生成规范、变更日志格式 |
| 代码迁移 | 框架升级步骤、兼容性检查清单 |
| 性能优化 | 基准测试命令、profiling 方法 |

---

### 第 4 层：Plugins —— 一键分发最佳实践

把 skills + hooks + MCP 配置打包为一个可安装单元。

```bash
# 新工程师第一天
claude plugins install payment-toolkit
```

立刻获得和老手完全相同的上下文和能力。通过托管 marketplace 在全组织分发更新。

---

### 第 5 层：MCP 服务器 —— 连接内部工具

连接 Claude 无法直接访问的数据源：
- 内部文档系统
- 工单系统
- 监控平台
- 结构化搜索 API

---

## 三、实操配置的四个核心模式

### 模式 1：初始化在子目录，不要站在仓库根目录

```
❌ 错误：cd /monorepo && claude
   → 上下文窗口塞满不相关的项目信息

✅ 正确：cd /monorepo/payment-service && claude
   → Claude 只加载 payment-service/ 的 CLAUDE.md
   → 同时自动向上加载 monorepo/ 的根 CLAUDE.md
   → 全局上下文不丢失，局部信息精简
```

### 模式 2：按子目录限定 lint/test 命令

让子目录的 CLAUDE.md 只声明该目录适用的命令。

Claude 改了 `payment-service` 一个文件，就应该跑 `./gradlew :payment-service:test`，而不是全量测试超时、输出淹没上下文。

> **例外**：编译型单体仓库（C/C++）跨目录依赖深，按子目录限定更难，需要项目特定的构建配置。

### 模式 3：清理噪音 —— .ignore + 权限控制

```json
{
  "permissions": {
    "deny": [
      "**/node_modules/**",
      "**/build/**",
      "**/target/**",
      "**/*.generated.*"
    ]
  }
}
```

该配置纳入版本控制，全团队生效。开发代码生成器本身的开发者可在本地 settings 覆盖项目级排除规则。

### 模式 4：代码地图 —— 当目录结构不能自解释时

在仓库根目录放置文件列表：

```markdown
# 仓库结构
- api-gateway/      —— API 网关，所有请求入口
- user-service/     —— 用户服务，注册/登录/权限
- order-service/    —— 订单服务，下单/支付/退款
- inventory-service/ —— 库存服务
- shared/           —— 跨服务共享库（DTO、工具类、HTTP 客户端）
- infra/            —— 基础设施配置（K8s、Terraform）
```

对几百个顶级文件夹的仓库，用分层方式：根文件描述最高层结构，子目录 CLAUDE.md 提供下一层细节。

---

## 四、LSP 集成 —— 让 Claude 按符号搜索而非字符串

```bash
# 安装对应语言的 code intelligence 插件和 language server
claude plugins install @anthropic/claude-code-lsp
```

**价值**：grep 一个通用函数名在大代码库里返回上千匹配，Claude 浪费上下文逐个打开文件判断。LSP 只返回指向同一符号的引用，过滤发生在 Claude 读取之前。

**尤其适合**：多语言代码库、C/C++ 大型项目。这是文章中强调的"最高回报投资之一"。

---

## 五、Subagents —— 探索和编辑分离

```markdown
# 典型用法
1. 用只读 subagent 映射子系统结构，写入 markdown 文件
2. 主 agent 根据完整图景执行编辑
```

Subagent 是独立的 Claude 实例，有各自的上下文窗口。完成子任务后仅返回最终结果，不污染主 agent 上下文。

---

## 六、随模型演进主动维护

**不要写死过去需要的规则。** 旧模型需要拆成单文件才能做好重构，你写了规则让它每次只改一个文件——新模型能做好跨文件协同编辑时，这条规则反而限制了它。

**Hooks/Skills 为弥补模型特定短板而建的，短板消失后就会变成纯开销。**

**审查节奏**：
- 每 3-6 个月做一次配置审查
- 大模型版本发布后立即审视
- 发现效果停滞不前时，先检查是否有过期配置在拖后腿

**典型案例**：Perforce 代码库中用于拦截文件写入执行 `p4 edit` 的 hook，在 Claude Code 原生支持 Perforce 模式后变得冗余。

---

## 七、组织推广：技术配置不够，还得有人牵头

### 最低配置：一个 DRI（直接责任人）

这个人有权威决策：
- settings 配置
- 权限策略
- plugin marketplace 管理
- CLAUDE.md 编写约定

### 理想配置：小团队先行

在所有开发者接触 Claude Code 之前，先把基础设施搭好。开发者第一次体验是高效的，而不是踩坑的，自然会扩散。

**实际案例**：
- 某公司：两个人提前搭建了一套 plugins + MCPs，全员普及当天即可用
- 另一家公司：一个专职 AI 工具团队在推广前就完成了基础设施建设

### 新兴角色：Agent Manager

混合 PM/工程师职能，负责管理整个 Claude Code 生态系统。

### 监管行业的额外建议

1. 从批准过的 skills 集合起步
2. 强制 AI 生成代码走相同的 Review 流程
3. 限制初始访问范围，随信心增长扩展
4. 尽早组建包含**工程 + 信息安全 + 合规**代表的跨职能工作组

---

## 八、总结：一张检查清单

| 阶段 | 动作 |
|------|------|
| **立即** | 编写根目录 CLAUDE.md（全局约定 + 关键坑点） |
| **立即** | 为核心服务/模块编写子目录 CLAUDE.md（本地命令 + 本地约定） |
| **立即** | 配置 .ignore 排除 node_modules / build / target 等噪音目录 |
| **第 1 周** | 搭建 postToolUse hook 自动跑 lint |
| **第 1 周** | 安装 LSP 插件，启用符号级导航 |
| **第 2 周** | 将重复性任务流程封装为 Skills（部署、代码迁移等） |
| **第 1 月** | 将验证过的 skills + hooks 打包为 Plugin，团队内分发 |
| **第 1 月** | 指定 DRI，统一配置标准和推广节奏 |
| **持续** | 每 3-6 个月或大模型发布后审查 CLAUDE.md 和 Skills |
| **需要时** | 搭建 MCP 连接内部系统（工单、文档、监控） |
| **需要时** | 用 subagents 处理大规模代码探索任务 |
