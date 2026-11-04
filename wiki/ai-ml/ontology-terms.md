---
title: 本体论术语表（GOALSONG 附录 A）
description: 《GOALSONG本体论纲》附录 A 的术语提炼：按本体论基础概念、描述逻辑与形式化语言、构建方法论、推理、映射对齐、评估质量、设计模式、智能体基础设施、小世界网络、GOALSONG 模型十组，以「术语 | 英文 | 定义」表格收录核心术语，定义措辞取自原文。
aliases: [Ontology Glossary, 本体术语表, GOALSONG 术语表, 附录 A 术语表]
tags: [ai-ml, concept]
sources: [2026/11/03/《GOALSONG本体论纲》.pdf]
created: 2026-11-03
updated: 2026-11-03
---

# 本体论术语表（GOALSONG 附录 A）

> 本页是《GOALSONG本体论纲》附录 A 的术语提炼，定义措辞取自原文；完整词条见 PDF 第 281–299 页（书内页码 273–291）。

原文对附录 A 的定位：收录本体论与相关领域核心术语，涵盖哲学基础、计算机科学、语义网技术、本体工程方法论、智能体基础设施等维度，"术语按逻辑分组，每组内按概念关联排列"，英文术语标注于中文术语之后便于国际对照；附录末尾自述这是"本体术语表"的系统性首次整理，收录术语一百余项，中英文对照、分类索引，并注明"术语表将随本体技术发展和标准演进而持续更新"（PDF 第 273 页、第 299 页）。

本页按原文 A.1–A.11 的分组编排，定义一律取自原文；加粗术语为全书反复使用的核心词。页号标注为 PDF 物理页号（括注书内页码）。

## A.1 本体论基础概念（PDF 281–283 / 书内 273–275）

| 术语 | 英文 | 定义 |
|------|------|------|
| **本体** | Ontology | 对特定领域知识的形式化、显式化规范说明，定义概念、属性和关系，是共享的语义模式。在计算机科学中，本体是对领域知识的形式化建模；在哲学中，本体论是"关于存在的学问"。术语表中"本体"如无特别说明均指计算机科学中的本体 |
| 本体论（哲学用法） | Ontology | 哲学分支，研究"什么是存在""存在有哪些基本类别""这些类别之间是什么关系"；亚里士多德提出"十范畴"体系，康德提出"十二范畴"体系 |
| 形式化 | Formal | 本体必须用机器可读的语言表达，不能是自然语言的模糊描述；形式化是本体区别于自然语言分类体系的关键特征 |
| 显式化 | Explicit | 本体中定义的概念、属性和关系必须是明确声明的，不能隐含在代码或文档中 |
| 规范说明 | Specification | 本体是对领域知识的抽象描述，不是某一个具体系统的实现细节 |
| 共享概念模型 | Shared Conceptualization | 本体所表达的领域知识是特定群体共同认可的概念体系，不是个人化的理解 |
| **概念** | Concept | 本体中的基本知识单元，由一组特征唯一组合而成的知识单元；概念不必然绑定于特定语言 |
| 术语 | Term | 特定领域中一般概念的词语指称，是概念的言语表达形式 |
| 定义 | Definition | 用描述性陈述表示概念，以区别于相关概念 |
| 内涵定义 | Intensional Definition | 通过列举概念的本质特征来定义概念 |
| 外延定义 | Extensional Definition | 通过列举概念所涵盖的实例来定义概念 |
| 领域本体 | Domain Ontology | 针对特定领域构建的本体，定义该领域的概念、属性和公理 |
| 通用本体 | Upper Ontology / Foundational Ontology | 跨领域通用的核心本体，定义最一般的概念和关系，如 SUMO、BFO、DOLCE |
| 应用本体 | Application Ontology | 针对特定应用场景构建的本体，通常复用领域本体和通用本体的概念 |
| 任务本体 | Task Ontology | 定义特定任务或活动中涉及的概念和关系，如诊断任务本体、规划任务本体 |
| 知识表示 | Knowledge Representation | 将知识以计算机可处理的形式表达的方法和技术；本体是知识表示的核心手段之一 |
| 知识图谱 | Knowledge Graph | 用图结构表示实体和关系的知识库，核心是"实体—关系—实体"三元组；知识图谱通常只存储实例数据，不定义 Schema |
| Schema | Schema | 定义数据结构的元模型，规定有哪些类型、属性、关系、约束；本体是 Schema 的一种形式化表达 |
| **实例** | Instance / Individual | Schema 的具体实例；原文的判定口径是"本体是 Schema，知识图谱是 Instance" |
| **类** | Class | 定义一组具有相同属性的个体；在 OWL 中类用 IRI 标识，支持类层次结构和类表达式 |
| 个体 | Individual | 类的具体实例；个体用 IRI 标识，可以属于多个类 |
| **属性** | Property | 描述实体特征或实体之间关系的元素；属性分为数据属性和对象属性 |
| 数据属性 | Data Property / Datatype Property | 描述实体自身特征的属性，取值域为数据类型（如字符串、整数、日期）；数据属性不具有传递性、对称性或互逆性 |
| 对象属性 | Object Property | 描述实体之间关系的属性，取值域为个体；对象属性的角色类似于自然语言中的"动词" |
| **公理** | Axiom | 表达不能被违反的逻辑约束；公理用于定义类和属性的逻辑规则 |
| 实体类型 | Entity Type | 对具有相同属性的一组实体的抽象；GB/T 48000.3—2026 将本体的核心组成明确为实体类型、属性和公理三部分 |
| TBox | TBox（术语公理集） | 描述逻辑中术语的公理集合，定义概念和角色；对应本体中的 Schema 部分 |
| ABox | ABox（断言公理集） | 描述逻辑中断言的公理集合，定义个体的具体事实；对应本体中的 Instance 部分 |
| **IRI** | Internationalized Resource Identifier | 国际化资源标识符，URI 的推广，允许使用 Unicode 字符集；本体中的类、属性、个体均用 IRI 标识 |
| 命名空间 | Namespace | IRI 的前缀部分，用于避免命名冲突；本体中实体类型的 IRI 命名空间应统一规范 |
| 前缀 | Prefix | 命名空间的缩写形式，用于提高可读性，如 `rdf:` 代表 `http://www.w3.org/1999/02/22-rdf-syntax-ns#` |
| 三元组 | Triple | RDF 的基本数据单元，由主语、谓语、宾语构成；主语和谓语是 IRI，宾语可以是 IRI、空白节点或字面量 |
| 空白节点 | Blank Node | RDF 中没有全局标识符的节点，用于表示没有 IRI 的资源 |
| 字面量 | Literal | RDF 中的数据类型值，如字符串、整数、日期 |
| 语义网 | Semantic Web | 万维网的扩展，使数据具有语义含义，支持机器理解和推理；RDF、RDFS、OWL、SPARQL 是语义网的核心标准 |
| 本体驱动的知识图谱 | Ontology-Driven Knowledge Graph | 将本体与知识图谱结合，本体定义 Schema，知识图谱存储实例；本体提供语义约束，知识图谱提供数据查询 |

## A.2 描述逻辑与形式化语言（PDF 283–285 / 书内 275–277）

| 术语 | 英文 | 定义 |
|------|------|------|
| **描述逻辑** | Description Logic（DL） | 形式化的知识表示语言，有严格的语法和语义，支持自动推理；是 OWL 的理论基础 |
| 概念（DL 术语） | Concept | 描述逻辑中对应本体中的类，如"设备"是一个概念 |
| 角色（DL 术语） | Role | 描述逻辑中对应本体中的对象属性，如"属于"是一个角色 |
| 个体（DL 术语） | Individual | 描述逻辑中对应本体中的实例 |
| 概念表达式 | Class Expression | 描述逻辑中用于定义复杂概念的表达式，支持合取、析取、否定、存在量词、全称量词等构造子 |
| TBox 推理 | TBox Reasoning | 基于术语公理集进行的推理，如概念包含关系判定、概念可满足性判定 |
| ABox 推理 | ABox Reasoning | 基于断言公理集进行的推理，如实例类型判定、实例一致性判定 |
| 可满足性 | Satisfiability | 一个概念是否存在至少一个实例；如果概念不可满足，说明概念定义存在矛盾 |
| 包含关系 | Subsumption | 概念 A 是概念 B 的子概念，即 A 的所有实例都是 B 的实例 |
| 一致性 | Consistency | 本体中不存在矛盾；如果本体不一致，说明存在无法同时满足的公理 |
| **OWL** | Web Ontology Language | W3C 推荐的本体描述语言，基于描述逻辑；OWL 2 是当前主流版本 |
| OWL 2 DL | OWL 2 DL | OWL 2 的描述逻辑子集，基于 SROIQ(D) 描述逻辑，支持自动推理 |
| OWL 2 QL | OWL 2 QL | OWL 2 的查询语言子集，设计用于高效查询关系数据库 |
| OWL 2 EL | OWL 2 EL | OWL 2 的存在语言子集，设计用于大规模本体的高效推理 |
| OWL 2 RL | OWL 2 RL | OWL 2 的规则语言子集，可以用规则引擎实现 |
| **RDFS** | RDF Schema | RDF 的 Schema 语言，提供类、属性、类层次结构、属性定义域和值域的基本定义；表达简单，推理能力弱 |
| **RDF** | Resource Description Framework | W3C 标准，用于表示 Web 上的资源及其关系；RDF 使用三元组模型 |
| RDF/XML | RDF/XML | RDF 的 XML 序列化格式，W3C 标准格式，适合工具交换 |
| Turtle | TTL | RDF 的简洁文本序列化格式，可读性好，适合人工阅读和 Git 版本管理 |
| JSON-LD | JSON-LD | RDF 的 JSON 序列化格式，与 Web 技术兼容性好，适合 Web 应用 |
| N-Triples | N-Triples | RDF 的简单行式序列化格式，每行一个三元组，易于解析 |
| **SHACL** | Shapes Constraint Language | W3C 标准，用于定义 RDF 数据的验证规则和约束；SHACL Shape 定义 RDF 图应满足的约束 |
| SHACL Shape | Shape | SHACL 中的形状定义，描述 RDF 图应如何被验证；Shape 定义目标节点、约束和验证规则 |
| NodeShape / PropertyShape | NodeShape / PropertyShape | 前者是 SHACL 中针对节点的形状，验证节点是否满足约束；后者针对属性，验证属性值是否满足约束 |
| Target / Focus Node | Target / Focus Node | Target 是 SHACL 中指定 Shape 应验证的节点集合；Focus Node 是验证过程中被 Shape 验证的节点 |
| SHACL 约束 | `sh:minCount` / `sh:maxCount` / `sh:datatype` / `sh:in` / `sh:nodeKind` | 原文列举的五类约束：属性值的最小数量、最大数量、属性值的数据类型、属性值必须在枚举列表中、节点的类型（IRI、空白节点、字面量） |
| SHACL 验证器 | SHACL Validator | 执行 SHACL 验证的工具，输入数据图和形状图，输出验证结果 |
| **SPARQL** | SPARQL | W3C 推荐的 RDF 查询语言，支持查询、插入、删除、更新操作 |
| SPARQL 端点 | SPARQL Endpoint | 支持 SPARQL 查询的服务端点，通过 HTTP 协议接收查询请求并返回结果 |
| SPARQL 推理 | SPARQL Reasoning | 在 SPARQL 查询中结合本体推理，使查询结果包含推理产生的隐含知识 |
| SWRL | Semantic Web Rule Language | 语义网规则语言，用于定义 OWL 本体中的规则 |

## A.3 本体构建方法论（PDF 285–286 / 书内 277–278）

| 术语 | 英文 | 定义 |
|------|------|------|
| 本体工程 | Ontology Engineering | 本体构建、评估、维护的方法论和技术体系 |
| 七步法 | Seven-Step Method | 斯坦福大学开发的领域本体构建方法，包括确定领域和范围、考虑复用现有本体、列出重要术语、定义类和类层次、定义属性、定义属性的分面、创建实例七个步骤 |
| 骨架法 | Skeletal Method / Enterprise Method | 构建企业本体的专门指导方法 |
| METHONTOLOGY | METHONTOLOGY | 本体构建方法论，包括规格说明、概念化、形式化、实现、维护等阶段 |
| TOVE | TOVE | 多伦多虚拟企业本体工程方法论 |
| IDEF5 | IDEF5 | 本体描述捕获方法，用于构建和验证领域本体 |
| 概念化 / 形式化 / 实现 / 评价 / 维护 | Conceptualization / Formalization / Implementation / Evaluation / Maintenance | 概念化是把领域知识抽象为概念、属性、关系的过程；形式化是把概念模型转换为形式化语言表达的过程；实现是把形式化模型转换为计算机可执行的本体文件；评价是对本体的质量、准确性、完整性进行评估；维护是对本体的持续更新、修正和扩展 |
| **六步构建法** | Six-Step Method | 依据 GB/T 48000.3—2026 的建模路径、结合工程实践总结的本体构建方法，包括需求与范围界定、术语与标准复用、概念模型形成、语义模型设计、映射与权限配置、校验发布与维护六个步骤 |
| 六步·需求与范围界定 | — | 第一步，明确业务场景、数据对象、服务对象和互操作需求 |
| 六步·术语与标准复用 | — | 第二步，复用已有标准中的核心实体类型和术语定义 |
| 六步·概念模型形成 | — | 第三步，梳理概念、属性、关系、规则与约束，形成与具体实现无关的抽象表达 |
| 六步·语义模型设计 | — | 第四步，采用 OWL/RDFS 等形式化语言构建语义模型 |
| 六步·映射与权限配置 | — | 第五步，建立数据源与语义对象的映射关系，定义权限语义 |
| 六步·校验发布与维护 | — | 第六步，开展一致性、完整性检查，建立版本管理和变更机制 |
| 半自动构建 | Semi-Automatic Construction | 机器提议、人工定夺的本体构建方法，结合 AI 的效率和人类专家的判断力 |
| 信息抽取 | Information Extraction | 从文档、数据库中提取对象信息并剔除冗余的过程 |
| 七要素框架 | — | 中国信通院提出的工业对象信息抽取框架，包括对象、属性、关系、事件、状态、规则、行动七个要素 |
| 地下根技术 | — | 中国信通院提出的借助分层网状结构实现复杂数据多维度关联的方法 |
| 语义聚类 | Semantic Clustering | 基于熵理论自动识别概念之间相似性和差异性的技术 |
| 本体学习 | Ontology Learning（OL） | 利用本体工程技术和机器学习技术自动或半自动构建本体的方法集合 |
| 概念抽取 / 关系识别 / 属性提取 / 公理生成 | Concept Extraction / Relation Recognition / Attribute Extraction / Axiom Generation | 依次为：从文本中识别领域核心概念；判断概念之间可能存在的关系；从文本中提取概念属性；从概念和关系中生成候选公理 |
| **最小世界模型** | Minimum World Model（MWM） | 一个能跑通业务闭环的最小本体，是为解决一个具体业务问题而构建的最小业务世界 |
| SmallWorld-小世界 | SmallWorld | 高颂数科的本体建设方法论品牌，可信数据空间的最小世界模型 |
| **闪电行动** | Lightning Action | 本体建模的快速启动方法论，不等条件完备，先用最小代价跑通一个闭环，用结果说话 |

## A.4 本体推理（PDF 286–287 / 书内 278–279）

| 术语 | 英文 | 定义 |
|------|------|------|
| 推理机 | Reasoner | 执行本体推理的软件工具，输入本体和实例数据，输出推理结果 |
| HermiT / Pellet / FaCT++ / JFact / TrOWL | 同名 | HermiT 是开源 OWL 推理机，支持 OWL 2 DL 推理，能够判断本体一致性和识别类之间的包含关系；Pellet 是开源 Java 实现的 OWL 2 DL 推理机，基于 tableau 算法，提供包含关系判定、一致性检查等推理服务；FaCT++ 基于描述逻辑，在 TBox 规模较小的场景下性能优异；JFact 是基于 Java 的 OWL DL 推理机；TrOWL 支持 OWL 2 QL 推理 |
| **推理一致性** | Reasoning Consistency | 推理机检查本体是否存在逻辑不一致（原文 A.6 中的同义表述为"推理机检查本体是否通过"）；常见不一致原因包括：实例同时归入两个不相交的类、函数性属性有多个值、属性的定义域/值域冲突 |
| 不一致 | Inconsistent | 本体中存在无法同时满足的公理 |
| 包含 / 分类 / 实例化 | Subsumption / Classification / Realization | 包含是推理机识别类之间的子类关系；分类是推理机计算类的层次结构、确定类之间的包含关系；实例化是推理机确定个体所属的最具体类 |
| 等价类 | Equivalent Class | 两个类具有相同的实例集合，用 `owl:equivalentClass` 表达 |
| 不相交类 | Disjoint Class | 两个类没有共同的实例，用 `owl:disjointWith` 表达 |
| 函数性属性 | Functional Property | 一个个体在该属性上最多有一个值，用 `owl:FunctionalProperty` 表达 |
| 逆函数性属性 | Inverse Functional Property | 一个值在该属性上最多被一个个体引用 |
| 传递性属性 | Transitive Property | 如果个体 a 通过属性 P 关联到 b、b 通过 P 关联到 c，则 a 通过 P 关联到 c，用 `owl:TransitiveProperty` 表达 |
| 对称性属性 / 逆属性 | Symmetric Property / Inverse Property | 前者指若 a 通过 P 关联到 b，则 b 通过 P 关联到 a；后者指两个属性互为逆关系，如"属于"和"包含" |
| 定义域 / 值域 | Domain / Range | 前者是对象属性可以应用的主体类，用 `rdfs:domain` 表达；后者是对象属性可以应用的客体类，用 `rdfs:range` 表达 |

## A.5 本体映射与对齐（PDF 287–288 / 书内 279–280）

| 术语 | 英文 | 定义 |
|------|------|------|
| **本体映射** | Ontology Mapping | 建立两个本体之间概念、属性、关系对应关系的过程 |
| **本体对齐** | Ontology Alignment | 识别两个本体中语义相关实体的过程，输出对齐集合 |
| 对齐 | Alignment | 两个本体之间映射的集合 |
| 本体匹配 / 模式匹配 / 实例匹配 | Ontology Matching / Schema Matching / Instance Matching | 依次为：发现两个本体之间语义对应关系；发现两个 Schema 之间语义对应关系；发现两个数据集中指向同一现实对象的实例 |
| 实体对齐 | Entity Alignment | 判断不同数据集中两个实体是否指向真实世界同一对象的过程；实体解析（Entity Resolution）是其同义术语 |
| 等价映射 | Equivalent Mapping | 两个概念含义完全相同的映射关系 |
| 上下位映射 | Subsumption Mapping | 一个概念是另一个概念子类的映射关系 |
| 近似映射 | Approximate Mapping | 两个概念含义相近但不完全相同的映射关系；原文明确"近似关系不应在运行时作等价替换使用" |
| **R2RML** | RDB to RDF Mapping Language | W3C 推荐的关系数据库到 RDF 的映射语言 |
| 直接映射 | Direct Mapping（DM） | W3C 推荐的将关系数据库直接映射为 RDF 的方法 |
| OBDA | Ontology-Based Data Access | 基于本体的数据访问，本体提供全局视图，映射描述全局模式与本地模式的关系 |
| Ontop / D2RQ | 同名 | Ontop 是虚拟知识图谱系统，支持 SPARQL 查询关系型数据库；D2RQ 是将关系型数据库发布为 RDF 的工具，支持 SPARQL 查询 |
| 语义标识 | Semantic Identifier | 本体中为每个概念、属性、关系分配的唯一标识符，通常采用 IRI 形式 |
| 语义描述 | Semantic Description | 数据携带的业务含义信息，包括概念映射、口径、语义标识及本体引用等 |
| 语义互操作 | Semantic Interoperability | 不同系统之间不仅能够交换数据，还能够准确理解数据含义的互操作能力 |
| 语义对齐 / 语义映射 | Semantic Alignment / Semantic Mapping | 前者是不同来源的语义描述通过映射建立对应关系的过程；后者是建立不同来源语义描述之间对应关系的规则集合 |

## A.6 本体评估与质量（PDF 288–289 / 书内 280–281）

| 术语 | 英文 | 定义 |
|------|------|------|
| OOPS! | Ontology Pitfall Scanner! | 在线本体扫描工具，检测常见的建模陷阱，涵盖 41 种陷阱类型，分为关键、重要、次要三级 |
| 本体陷阱 / 反模式 | Ontology Pitfall / Anti-Pattern | 本体陷阱是本体建模中常见的错误或不良实践；反模式是看起来像解决方案但实际上是错误的设计模式 |
| 设计模式 | Ontology Design Pattern（ODP） | 解决本体设计问题的可复用方案 |
| 质量指标 | Quality Metric | 衡量本体质量的量化指标 |
| **孤儿概念** | Orphan Concept | 引用次数为零的概念，是本体库腐化的第一信号 |
| 属性覆盖率 | Property Coverage | 已定义 Domain/Range 的属性数占总属性数的比例 |
| 标签缺失率 | Label Missing Rate | 无 `rdfs:label` 的概念数占总概念数的比例 |
| SHACL 违规数 | SHACL Violation Count | SHACL 验证不通过的实例数 |
| 变更记录覆盖率 | Change Record Coverage | 有变更记录的概念数占总概念数的比例 |
| 结构健康度 | Structural Health | 本体自身的组织质量，包括类层次深度、属性分布、关系密度、模块化程度等 |
| 语义健康度 | Semantic Health | 本体的语义质量，包括标签完整性、注释覆盖率、术语一致性、定义准确性等 |
| 消费健康度 | Consumption Health | 本体库被使用的程度，包括消费方数量、月调用量、活跃消费方比例、场景覆盖率等 |
| 生态健康度 | Ecosystem Health | 消费方生态的活力，包括贡献者数量、行业本体包覆盖率、标准符合性等 |

## A.7 本体设计模式与模块化（PDF 289–290 / 书内 281–282）

| 术语 | 英文 | 定义 |
|------|------|------|
| 本体设计模式 | Ontology Design Pattern（ODP） | 解决本体设计问题的可复用方案；常见模式包括部件-整体模式、参与-角色模式、信息对象模式等 |
| 内容模式 / 逻辑模式 / 架构模式 | Content / Logical / Architectural Pattern | 依次解决特定内容建模问题、逻辑结构建模问题、本体整体架构问题 |
| 模块化本体 | Modular Ontology | 由本体模块集合及模块间语义关系组成的本体 |
| 本体模块 | Ontology Module | 模块化本体的组成部分，描述某一特定领域的本体，由自洽的用户或团体开发 |
| 本体模块化 | Ontology Modularization | 将大规模本体分解为更简单的本体模块的过程 |
| 本体摘要 | Ontology Summarization | 从本体中提取关键知识的方法 |
| 本体演化 | Ontology Evolution | 本体随时间变化的过程，包括原子变化、混合变化和复杂变化 |
| 版本管理 | Version Management | 本体库的版本控制，采用语义化版本规则 |
| 语义化版本 | Semantic Versioning | 版本号格式为 MAJOR.MINOR.PATCH；不兼容的变更触发主版本升级，向后兼容的功能新增触发次版本升级，向后兼容的缺陷修复触发修订版本升级 |
| 变更管理 | Change Management | 本体库变更的提案、评审、灰度生效、正式发布流程 |
| RFC | Request for Comments | 本体库变更的制度化流程，包括提案、影响分析、评审、灰度生效、正式发布五个步骤 |
| 影响分析 | Impact Analysis | 分析变更影响哪些模型、哪些下游 Agent、哪些映射规则、哪些 SHACL 约束的过程 |

## A.8 本体与智能体基础设施（PDF 290–292 / 书内 282–284）

| 术语 | 英文 | 定义 |
|------|------|------|
| 智能体 | Agent | 自主运行的软件程序，能够感知环境、进行推理、执行行动 |
| **智能体身份码** | AIC（Agent Identity Code） | 由国家 OID 注册中心授权节点分配给智能体的唯一标识符；采用 OID 分层编码体系，具备全球唯一性、可解析性和可追溯性 |
| OID | Object Identifier | 对象标识符，用于在全球范围内唯一标识对象；高颂数科的智能体身份码节点 OID 为 `1.2.156.3088.1.0` |
| **TAV** | Trusted Agent Verification（可信智能体验证） | 依据 GB/Z 185—2026 系列、GB/Z 242—2026 和 GB/T 47475—2026，构建"身份—能力—价值"三级验证框架 |
| 词元 | Token | 智能体服务的价值单元，是连接数据、模型、算力与产业应用的核心纽带 |
| **有效词元** | Valid Token | 只有同时满足时延、可用性、准确率等服务等级要求、最终被用户实际使用的输出词元，才进入商业计量 |
| 词元运营中心 | — | 实现词元登记、计量、定价、交易、结算全流程的运营平台 |
| ioC 接入连接器 | ioC Connector | Agent 接入可信数据空间的网关，支持 MCP、A2A、ANP 等协议，实现 Agent 与数据、模型、工具、其他 Agent 之间的可信互联 |
| MCP / A2A / ANP | Model Context Protocol / Agent to Agent / Agent Network Protocol | 依次为模型与上下文交互协议、智能体之间协同协议、智能体网络接入协议 |
| **OBAC** | Ontology-Based Access Control | 基于本体的访问控制，以本体中的概念、对象、属性及关系为控制对象，实现语义级访问控制 |
| RBAC / ABAC | Role-Based / Attribute-Based Access Control | 基于角色的访问控制、基于属性的访问控制（原文仅给出这一句定性） |
| 数字合约 | Digital Contract | 可信数据空间中数据流通参与方之间就数据使用达成的机器可读协议 |
| 使用控制 | Usage Control | 可信数据空间中实时监控数据使用行为、执行使用控制策略、记录使用存证的机制 |
| **语义供给** | Semantic Supply | 数据基础设施从"数据供给"升级为"语义供给"，提供可理解的数据，让数据携带业务含义 |
| 数据飞轮 | Data Flywheel | 国家数据局提出的"场景牵引数据、数据驱动模型、模型赋能应用、应用创造价值"的良性循环 |
| 数据资产运营 2.0 | — | 在数据资产运营 1.0 基础上，加上本体、加上 Agent、加上词元，实现从静态盘点到动态运营的升级 |
| 可信 AI 基础设施 | Trusted AI Infrastructure | 由身份层、验证层、计量层、连接层构成的四层架构，本体是"根" |
| 本体网络 | Ontology Network | 由多个最小世界模型通过共享对象和关系自然生长形成的本体生态 |
| 产业语义基础设施 | Industrial Semantic Infrastructure | 由通用级本体、工业级本体、行业级本体构成的跨行业语义基础设施 |
| Palantir Ontology | Palantir Ontology | Palantir 以 Ontology 为核心构建的企业级 AI 操作系统，由语义要素（对象、属性、关系）和行动要素（Functions、Actions、动态安全）构成 |
| Functions / Actions / 动态安全 / Action Log | Functions / Actions / Dynamic Security / Action Log | Functions 是围绕本体对象运行的业务计算与处理逻辑；Actions 规定对象如何被创建或修改，也可以触发外部系统；动态安全在用户和 Agent 发起交互时计算访问和执行权限；Action Log 保存行动提交及数据修改记录，为审计和复盘提供依据 |
| **语义罗盘** | Semantic Compass | 本体作为智能体理解业务世界的导航工具，提供概念地图、关系指南、行动规则 |
| 最小对象集 | — | 只建立支撑当前业务闭环的最小对象集，没有参与当前业务闭环的对象暂时不建 |
| 业务闭环 | Business Closed Loop | 从"发现异常"到"处置完成"的完整业务过程 |
| 状态 | State | 对象在业务世界中的动态属性；设备有运行中、降额、故障、维修中、停机五种状态 |
| 行动闭环 | Action Closed Loop | 从本体对象→查询→计算→规则→业务判断→Agent→行动→状态更新→重新计算的完整闭环 |
| 感知闭环 / 认知闭环 / 行动闭环 | — | 原文并列给出的三层闭环：感知闭环是从数据采集到状态更新的闭环；认知闭环是从状态变化到影响分析的闭环；行动闭环是从影响分析到行动执行的闭环 |

## A.9 小世界网络（PDF 292–293 / 书内 284–285）

A.9 起原文改为三列表格（术语 | 英文 | 定义），以下为该表全部词条。

| 术语 | 英文 | 定义 |
|------|------|------|
| **小世界网络** | Small World Network（SWN） | 由可信智能体基于共享本体、内置合约和 OBAC 使用控制矩阵，围绕场景目标自主结伴、自主协同、自主执行而形成的动态协同网络 |
| 局部高聚类 | Local High Clustering | 同一业务域内的智能体紧密协同，形成高聚类的簇 |
| 全局短路径 | Global Short Path | 不同业务域的智能体之间通过少量长程连接快速触达 |
| 场景识别 | Scenario Recognition | 小世界网络运行的第一步，基于本体中的事件定义自动识别当前场景 |
| 伙伴发现 | Partner Discovery | 小世界网络运行的第二步，沿着本体中的协同关系自动发现需要参与协同的智能体 |
| 网络构建 | Network Construction | 小世界网络运行的第三步，智能体根据本体中的协同关系自主结伴，形成小世界网络 |
| 自主执行 | Autonomous Execution | 小世界网络运行的第四步，智能体根据内置合约和 OBAC 矩阵自主执行行动 |
| **内置合约** | Built-in Contract | 小世界网络中智能体遵循的行动规则，机器可读、可自动执行 |
| 语义宪法 | Semantic Constitution | 本体在小世界网络中扮演的角色，定义业务对象、关系、状态、行动和公理 |

## A.10 GOALSONG 模型（PDF 293–294 / 书内 285–286）

| 术语 | 英文 | 定义 |
|------|------|------|
| GOALSONG 模型 | GOALSONG Model | 高颂数科提出的数据资产价值倍增模型，包含八边形框架和双螺旋钻石模型两个版本 |
| 八边形框架 | Octagon Framework | 八个机制围绕"数据资产价值倍增"中心排列：目标治理、动态优化、业务对齐、价值杠杆、生态协同、持续运营、网络效应、价值增长 |
| 双螺旋钻石模型 | Double Helix Diamond Model | 八个机制分为 GOAL 旋（价值创造层）和 SONG 旋（价值释放层），两个螺旋相互咬合、相互驱动 |
| GOAL 旋 | GOAL Spiral | 价值创造层：增益观测（GainCapture）→弦配置（OptimizeGrid）→虫洞构建（AccessHub）→纠缠连接（LinkExchange） |
| SONG 旋 | SONG Spiral | 价值释放层：防护盾（ShieldGuard）→溢洪道（OmniFlow）→导航仪（NaviCompass）→增殖堆（GrowthEngine） |
| 价值捕获 | GainCapture | 实时识别高价值数据资产 |
| 智能配置 | OptimizeGrid | 动态优化资源分配网络 |
| 动态接入 | AccessHub | 统一认证的访问枢纽 |
| 生态互联 | LinkExchange | 跨域数据价值交换市场 |
| 安全防护 | ShieldGuard | 风险智能监控与阻断 |
| 通量枢纽 | OmniFlow | 数据流量（词元）管控 |
| 决策导航 | NaviCompass | 场景化最优路径指引 |
| 增值引擎 | GrowthEngine | 自动触发价值倍增 |

## A.11 术语索引表：结构与用法（PDF 294–299 / 书内 286–291）

A.11 是**倒排索引**而非释义表：三列为「中文术语 | 英文术语 | 所属类别」，每条只给中英对照与所属类别，不复述定义。

| 结构要素 | 原文内容 | 用法 |
|----------|----------|------|
| 三列结构 | 中文术语 / 英文术语 / 所属类别 | 查一个词属于哪一组、对应哪个英文写法；定义须回 A.1–A.10 |
| 类别取值 | 基础概念、本体类型、本体元素、描述逻辑、形式化语言、推理、映射、方法论、评估、治理、智能体基础设施 | 据所属类别定位该术语在书中的位置 |
| 入表口径 | 只收录有固定中英对照的词条 | 只有中文名的词条（如"词元运营中心""最小对象集""数据资产运营 2.0"）未入索引；A.9、A.10 两组词条也未出现在 A.11 中 |
| 与原组的写法差异 | 索引把「设计模式」写作 `Design Pattern`（A.6/A.7 作 Ontology Design Pattern）；「通用本体」写作 `Upper Ontology`（A.1 作 Upper Ontology / Foundational Ontology）；「六步构建法」写作 `Six-Step Method` | 跨节引用时注意口径 |

## 本页分析

以下为跨组串联的归纳，非原文叙述：

- **A.1–A.7 是学科层术语，A.8–A.10 是命题层术语**：前者（本体、描述逻辑、OWL/RDFS/SHACL/R2RML/SPARQL、映射对齐、健康度）可与 W3C 与描述逻辑文献直接对照；A.8 起开始出现高颂数科自有资产（AIC、TAV、ioC、OBAC、语义罗盘），A.10 是唯一的商业模型分组。
- **全书核心命题在术语表里得到最短定义**：[[goalsong-ontology-thesis]] 的"本体定义世界，词元驱动未来"分别落在 A.1「本体＝形式化、显式化、共享的规范说明」与 A.8「词元＝智能体服务的价值单元」两条上；[[effective-token-economy]] 的计量口径即 A.8「有效词元」一条。
- **口径一致性**：A.3 的 MWM 定义与 [[minimal-world-model]] 一致；A.8 的 OID `1.2.156.3088.1.0` 与 [[ontology-identification]] 引用的 GB/Z 185.2—2026 说法一致；A.9 的"共享本体 + 内置合约 + OBAC 使用控制矩阵"与 [[small-world-network]] 三机制吻合。

## 原文未说明 / 边界

- **页号口径**：原任务清单把附录 A 标为 PDF 第 273–299 页，逐页核对正文后确认 PDF 第 273–280 页实际是第 15 章 15.8–15.11 节（书内 265–272），**附录 A 本体位于 PDF 第 281–299 页（书内 273–291）**。本页页号以 PDF 物理页为准并按实际内容重新标注。
- **排版**：A.1–A.7 原文是"中文术语（英文术语） 定义"的段落体，只有 A.9、A.10、A.11 是真正的三列表格；本页统一改为表格以便检索，条目措辞未改，仅对同一"英文—定义"模式的近义条目做了合并行（如 NodeShape/PropertyShape、OWL 推理机、五种属性特征）。
- **索引无页号**：原文 A.11 只有「中文术语 | 英文术语 | 所属类别」三列，**没有页号列**，原文也未说明每个词条首发于第几章。本页 A.11 因此只写结构与用法，不编造"术语→页号"映射。
- **原文未展开的词**：RBAC、ABAC、MCP、A2A、ANP 只有一句话定性，原文未给出机制细节；"骨架法"原文未展开步骤。
- **词条数量**：原文称收录"一百余项"，但未给出精确条目数；本页是核心术语提炼，不是全量搬运。
- 原文未说明术语表的编订者、评审流程与版本号。

## 相关页面

- [[goalsong-ontology-thesis]] — 本术语表所属全书的总纲；A.1「本体」与 A.8「词元」两词就是该页核心命题的术语底座。
- [[ontology]] — 本体论总概念页；A.1 对"本体 = 形式化、显式化、共享的规范说明"的拆解是其工程化版本。
- [[ontology-identification]] — 本体识别与七要素框架；A.3「七要素框架」（对象、属性、关系、事件、状态、规则、行动）与身份码 OID `1.2.156.3088.1.0` 与该页口径一致。
- [[ontology-six-step-method]] — 六步构建法；A.3 逐条给出六步名称、职责与 GB/T 48000.3—2026 出处。
- [[small-world-network]] — 小世界网络；A.9 是该机制最完整的一组定义（局部高聚类、全局短路径、四步运行、内置合约、语义宪法）。
- [[effective-token-economy]] — 词元与有效词元；A.8 的「词元」「有效词元」「词元运营中心」「通量枢纽」是该页计量逻辑的术语依据。
- [[trusted-ai-infrastructure]] — 可信 AI 基础设施；A.8 的「TAV」「ioC 接入连接器」「可信 AI 基础设施（本体是根）」是该页核心词条。
- [[goalsong-methodology-system]] — 方法论总览；A.10 的 GOALSONG 八边形框架与双螺旋钻石模型即该体系的价值倍增模型。
- [[ontology-standards-index]] — 附录 C 标准索引；A.1「实体类型」、A.3「六步构建法」、A.8「TAV」三条定义中引用的国标号可在该页核对全称与日期。
