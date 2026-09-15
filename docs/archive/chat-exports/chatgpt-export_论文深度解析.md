# 论文深度解析

> 深度解析这篇论文：Shaping Schema via Language Representation as the Next Frontier for LLM Intelligence Expanding

我会先核对论文的出处与内容，再按“核心问题—方法—理论贡献—局限—启发”来拆解。标题看起来可能是预印本或工作论文，我会以可检索到的原文为准。

我已找到 arXiv 原文：这是 2026 年 5 月提交的 41 页预印本。关键不在“又一种提示词技巧”，而在作者试图把“语言表示”提升为 LLM 智能扩展的一条研究路线。

## 一句话总评

这篇论文的价值不在于提出了一个新模型，而在于提出一个很有野心的研究视角：**LLM 的下一阶段智能扩展，不只是扩大参数、加工具、调 prompt，而是重新设计“语言表示”本身，让问题以更适合模型内部推理的形式被表达出来。**论文是 arXiv 预印本，2026 年 5 月 10 日提交，作者包括 Zhiqin Yang、Yuhan Liu、Jingwen Fu、Pei Fu、Bo Han、Masashi Sugiyama、Nanning Zheng，共 41 页、30 图。

## 1. 核心命题：问题不是“模型不知道”，而是“表示方式激活不了正确图式”

论文的中心观点是：自然语言虽然是 LLM 的默认交互媒介，但它对复杂任务的表达能力有限，容易造成“信息瓶颈”。作者认为，LLM 内部知识的激活与组织方式，也就是他们称为 **schema / 图式** 的东西，很大程度取决于输入任务所采用的语言表示是否结构化、符号化、逻辑化。

这里的“schema”不是数据库 schema，而更接近认知科学里的“图式”：一种内部组织框架，决定哪些知识被调用、如何关联、怎样参与推理。作者把 LLM 的 schema 理解成一种抽象的、图状的内部知识组织结构，用来解释为什么语义等价的问题，换一种表达形式后，模型表现会大幅不同。

因此，论文真正想推动的是从 **prompt engineering** 转向 **language representation design**。prompt engineering 主要改写输入问题；而 language representation design 不只是改写问题，还改变问题与答案共同所在的“语言空间”，例如从自然语言转成布尔表达式、Lisp 树、约束系统、PDDL、Lean、SQL、代码、图结构或专门 DSL。

## 2. 作者提出的四层表示框架

论文把语言表示从低到高分成四个层级：

| 层级 | 含义 | 典型形态 | 作用 |
|---|---|---|---|
| Level 0 | 自然语言 | 普通文字描述 | 灵活但模糊 |
| Level 1 | 消除歧义 | 结构化格式、模板、显式字段 | 减少实体与关系误解 |
| Level 2 | 逻辑约束 | 数学、代码、形式逻辑、规则系统 | 让推理路径更稳定 |
| Level 3 | 科学形式化 / 世界建模 | Lean、PDDL、LTL、仿真语言、领域 DSL | 表达自然语言难以承载的物理、因果、动态系统 |

作者认为 Level 1 和 Level 2 主要增强现有能力，例如提高稳定性、减少歧义、降低推理漂移；Level 3 则可能扩展能力边界，因为自然语言本身不足以精确表达高维科学规律、物理约束、因果结构和状态演化。

这一点是论文最有启发性的地方：它把“语言”看成一种 **推理基础设施**，而不是单纯的信息包装。换言之，表示形式不是外壳，而会改变模型内部怎样“看见”任务。

## 3. 理论部分：把语言设计形式化为 schema alignment 问题

论文试图做一个形式化：给定问题空间、答案空间和语言空间，LLM 只能处理语言表示，因此任务必须先被编码进某种语言表示。语言设计的目标，是找到一种语言空间，使 LLM 诱导出的输入—输出映射尽量接近理想目标映射。

更进一步，作者定义了 **schema mismatch / 图式失配**：某种语言表示诱导出的 schema 与任务所需目标 schema 之间的差异，用 KL 散度来描述。作者的主张是，预测误差受 schema mismatch 控制；所以语言设计可以被重新理解成一种受约束优化问题，其目标是让语言诱导的内部图式尽可能贴近任务所需图式。

这部分的思想很漂亮，但也最需要谨慎。它给了“为什么表示影响智能”一个数学叙述，但许多对象仍较抽象：schema space 如何严格定义？schema distribution 如何实证估计？KL 散度对应的内部机制如何验证？这些并没有被完全解决。

## 4. 实验：同一个逻辑电路任务，15 种等价表示，模型表现显著不同

实验选择的是组合逻辑电路模拟任务。这个任务有明确形式语义，且可以自动生成真值答案，因此适合控制变量。作者把同一个底层电路问题渲染成 15 种语义等价的语言表示，包括自然语言、Netlist、图邻接表、矩阵表示、Lisp 树、数据流语言、部分真值表、紧凑门符号、逆波兰表示、依赖链语言、分层执行计划、信号传播轨迹、约束满足格式、规范布尔表达式和 Petri 网表示。

结果上，作者报告：**Canonical Boolean Expression / 规范布尔表达式** 表现最好，Lisp Tree Notation 也接近；自然语言虽然适应性强，但表达和推理成本更高；图邻接表虽然显式表达结构，却因为缺乏内在因果执行顺序而表现不如更贴合信号传播顺序的表示。

作者还提出两个内部诊断指标：**KAI / Knowledge Activation Index** 用注意力熵和关键逻辑节点的注意力质量衡量“知识激活”；**KOI / Knowledge Organization Index** 用跨层 attention 相似性衡量“知识组织稳定性”。作者明确说它们是 schema 的代理指标，而不是对 schema 的直接测量。turn743384view4

附录里还有一个更有力的现象：对 Qwen3-32B 的隐藏状态做 t-SNE 后，即使底层逻辑问题相同，不同表面表示仍形成明显分离的簇。作者据此认为，模型隐藏状态几何被“问题怎么写”强烈支配，而不只是被“问题问什么”支配。

## 5. 论文的真正贡献

第一，它把很多零散经验统一起来了。过去大家知道：代码比自然语言更适合算法任务，SQL 比文字更适合数据库查询，Lean 比自然语言更适合定理验证，PDDL/LTL 比普通描述更适合规划。但这篇论文把这些现象归到一个共同机制：**不同语言表示会诱导不同内部 schema**。

第二，它区分了“提示词技巧”和“表示语言设计”。这很重要。很多 prompt 技巧仍停留在自然语言内部，例如“逐步思考”“请严谨推理”“列出步骤”。而 language representation design 是把任务转成更合适的中间语言：让因果顺序、变量依赖、逻辑约束、状态转移、可验证规则直接显式化。

第三，它把效率也纳入智能评价。作者不只看准确率，还看推理时间、prompt token、completion token，以及内部注意力组织。这个视角对实际应用很有价值，因为一个表示即使准确率高，如果让模型耗费大量 token 才能“翻译回正确结构”，也不一定是最优表示。turn743384view4

## 6. 主要局限与我的批判

**局限一：证据域较窄。**核心实验集中在逻辑电路模拟。这个任务天然偏向布尔表达式、Lisp 树、分层执行等形式，所以“规范布尔表达式最好”并不令人意外。要证明“语言表示是 LLM 智能扩展的下一前沿”，还需要跨数学、规划、医学、法律、科学仿真、软件工程、机器人等更广泛任务验证。

**局限二：schema 概念仍偏隐喻。**论文把 schema 讲得很有解释力，但真正可测量的是 attention、hidden state clustering、token/time/accuracy。KAI 和 KOI 是有用的 exploratory proxies，但它们还不能证明模型内部确实存在作者定义意义上的 schema。作者自己也承认 KAI/KOI 只是代理指标，并且中间区间不严格单调。

**局限三：形式化假设较强。**例如语言编码映射的同构性、schema 空间、schema 分布、schema mismatch 的 KL 散度，都对理论叙述有帮助，但距离可操作的实验测量还有距离。换句话说，论文提供的是一个研究纲领，而不是一个已经闭环的理论。

**局限四：还没有真正实现“AI 自动构造新语言”。**论文的远景是让 AI 从世界交互中构造最优语言表示，但实验仍主要比较人类设计的 15 种表示。真正的下一步应该是：给定任务族，让模型或搜索算法自动发现一套 DSL，并证明它可迁移、可学习、可解释、可验证。

## 7. 对 LLM 应用的启发：别只写 prompt，要设计“任务语言”

如果你做 LLM 应用，这篇论文最实用的启发是：

**不要把复杂任务直接塞进自然语言。**先问：这个任务的真实结构是什么？是流程？约束？图？表？因果链？状态机？数学表达式？代码？搜索空间？然后选择能显式表达该结构的中间表示。

几个例子：

| 任务 | 不佳表示 | 更好表示 |
|---|---|---|
| 多步骤业务流程 | 长段自然语言需求 | 状态机 / DAG / YAML workflow |
| 数据查询 | “帮我找出……” | SQL schema + 查询约束 |
| 机器人规划 | 普通指令 | PDDL / action language / LTL |
| 数学证明 | 口语化证明 | Lean / Coq / 一阶逻辑草图 |
| 代码生成 | 模糊需求 | 类型签名 + 输入输出规范 + 测试用例 |
| 复杂决策 | 散文描述 | 变量、目标函数、约束、优先级表 |
| 科学问题 | 文字解释 | 方程、仿真参数、单位系统、边界条件 |

这也是论文对“prompt 工程”的升级：**好的 prompt 不是更会说话，而是更会建模。**

## 8. 最终判断

我认为这篇论文最适合作为一篇 **position paper + 初步实验证据** 来读。它没有完全证明“语言表示就是下一前沿”，但它很好地指出了一个被低估的问题：LLM 的能力不只取决于模型参数，也取决于任务被投喂给模型时的表示几何。

更直白地说：**模型的智能上限，可能被我们给它的语言接口卡住了。**
未来真正有价值的方向，可能不是“写更长的 prompt”，而是为不同任务族设计、学习甚至自动发现更适合机器推理的表示语言。

-----

> LLM 的能力不只取决于模型参数，也取决于任务被投喂给模型时的表示几何
> 围绕这个规划一个ACL研究

可以，而且这个题目很适合做成一篇 **ACL 风格的“现象 → 机制 → 干预”论文**。我建议不要泛泛证明“格式很重要”，而是把核心问题收紧成：**语义等价的任务表示，会不会系统性改变模型的内部表示几何，并进一步因果性地改变推理能力？** 我先把相关工作和可发表的实验边界对齐，再给你一套可以直接开题的方案。

已经能看到一个很明确的切口：ACL 2026 刚有工作发现，**meaning-preserving prompts 在模型内部并不会自然聚成一团，prompt template 对 logits 的影响甚至可能大于问题本身**；另外也有 ACL 论文把推理过程直接当作 representation-space trajectory 来分析。也就是说，你这个想法不是“空中楼阁”，但如果只是再做一次 t-SNE + accuracy，会很容易被 reviewer 说成已有 prompt sensitivity 的换皮。

真正能把论文打出来的关键，是加上一个 **因果闭环**：表示改变 → 几何改变 → 能力改变；并且反过来，**只干预内部几何、不改输入语义，也能恢复/破坏能力**。

可以。这个方向要做成 ACL，我会把它从一个“观点”升级成一篇**可测量、可证伪、带因果干预的机制论文**。

我最推荐的核心命题是：

> **Semantically equivalent task representations induce different internal geometries in LLMs; reasoning succeeds when the induced geometry aligns with the task’s latent computational structure.**

也就是不再停留在“换一种表示准确率会变化”，而是回答更强的问题：

**为什么会变化？这种变化是不是由内部表示几何介导的？如果我们只改变内部几何，不改 prompt，能力会不会跟着改变？**

这一步非常关键。原来的 *Shaping Schema via Language Representation* 已经展示了不同语言表示会改变性能和内部激活，但主要证据仍集中在逻辑电路和相关性分析。 更重要的是，ACL 2026 已经出现两条与你非常接近的工作线：一篇发现 meaning-preserving prompts 在 LLM 内部并不会自动聚合，且 prompt template 对 logits 的影响可能比问题本身还大；另一篇已经把推理过程建模成 representation-space trajectory，并用几何结构预测和控制推理。turn930026view0

所以，你的 ACL novelty 最好放在：

**task representation → task-aligned geometry → reasoning performance → causal intervention**

而不是再做“prompt format sensitivity”。

---

# 我建议的论文题目

首选：

**Same Problem, Different Geometry: Causal Effects of Task Representation on LLM Reasoning**

另外两个也可以：

**Representation Shapes Reasoning: How Task Languages Reorganize LLM Internal Geometry**

**Beyond Prompt Sensitivity: Task-Aligned Representation Geometry Governs LLM Reasoning**

如果结果足够漂亮，第一个最像 ACL main 的标题。

---

# 1. 整篇论文的核心模型

设底层问题为：

$$

x

$$

它有一个真实的、与语言无关的任务结构：

$$

G_x

$$

例如：

- 数学表达式的 AST；
- 图问题的 graph；
- 约束问题的 dependency graph；
- 规划问题的 state-transition graph。

同一个问题可以通过不同 renderer 表达：

$$

p_r = \phi_r(x)

$$

其中 $r$ 是 representation，例如 natural language、JSON、prefix notation、DSL、predicate logic 等。

LLM 在第 $l$ 层产生：

$$

H_l(x,r)

$$

传统 prompt sensitivity 研究基本研究：

$$

r \rightarrow P(y|x,r)

$$

你的论文研究：

$$

r
\rightarrow
H_l(x,r)
\rightarrow
\text{Alignment}(H_l,G_x)
\rightarrow
P(y|x,r)

$$

核心假设变成：

$$

\boxed{
\text{Reasoning Performance}
\propto
\text{Alignment}
(
\text{Model Geometry},
\text{Task Geometry}
)
}

$$

这个 formulation 比“schema 被激活得更好”强很多，因为 **task geometry 和 model geometry 都可以实际测量**。

---

# 2. 四个 Research Questions

| RQ | 问题 | 真正想证明什么 |
|---|---|---|
| RQ1 | 同一问题换等价表示后，性能是否系统变化？ | Representation Effect |
| RQ2 | 哪种内部几何对应更高的推理性能？ | Geometry–Performance Link |
| RQ3 | 模型内部几何是否更接近任务真实结构时表现更好？ | Task–Geometry Alignment |
| RQ4 | 人为把 hidden states 推向“好表示”的几何，能否提升“差表示”的性能？ | Causality |

其中 **RQ4 是决定它像不像 ACL main 的关键实验**。

因为 ACL 2026 已经有大量 activation steering / causal intervention 工作，仅仅发现 hidden states 不同已经远远不够；当前文献甚至已经用 latent geometry 控制推理行为。

---

# 3. 不要只做 Boolean Circuit：构建一个 ReGeoBench

我建议自己生成一个 benchmark，可以暂时叫：

**ReGeoBench — Representation Geometry Benchmark**

每个问题首先生成一个**语言无关的 canonical object**，然后自动渲染成多个严格语义等价的版本。

最漂亮的是选择三种完全不同的计算结构：

| Task family | Ground-truth structure | Representations |
|---|---|---|
| Compositional Reasoning | Tree / AST | NL、infix、prefix、postfix、JSON AST、DSL |
| Graph Reasoning | Graph | NL relations、edge list、adjacency list、DOT-like、JSON、DSL |
| Constraint Reasoning | Constraint graph | NL、predicate list、table、logical formula、SMT-like DSL、synthetic DSL |

这样论文天然就有一个很漂亮的理论故事：

**Tree → Graph → Constraint System**

而不是“我们找了 15 种奇怪格式测一下”。

每个 latent instance $x$ 建议生成 6 个 representation：

$$

x\rightarrow
\{r_1,r_2,\ldots,r_6\}

$$

答案完全一致。

而且每个 renderer 都由程序生成，因此可以保留：

$$

\text{semantic unit}
\leftrightarrow
\text{token span}

$$

的 alignment。

这个细节后面非常重要。

---

# 4. 最大的方法创新：Task–Geometry Alignment

这是我觉得最值得做成论文核心 metric 的地方。

假设某问题包含 semantic units：

$$

u_1,u_2,\dots,u_n

$$

例如表达式：

$$

(a+b)\times(c-d)

$$

对应：

- $a$
- $b$
- $+$
- $c$
- $d$
- $-$
- $\times$

renderer 在生成每种 representation 时记录每个 semantic unit 对应的 token span。

于是对第 $l$ 层：

$$

h_i^{(l)}
=
\text{Pool}(H_l[\text{tokens}(u_i)])

$$

得到模型内部 semantic units 的 representation。

然后构造一个 pairwise geometry：

$$

K^{model}_{ij}
=
\cos(h_i,h_j)

$$

另一方面，你知道真实计算结构 $G_x$。

例如 AST 中节点之间的 structural distance：

$$

d_G(u_i,u_j)

$$

可转换为：

$$

K^{task}_{ij}
=
\exp(-d_G(u_i,u_j)/\tau)

$$

最后定义：

$$

\boxed{
TGA_l
=
\rho
\left(
\operatorname{vec}(K^{model}_l),
\operatorname{vec}(K^{task})
\right)
}

$$

可以叫：

**Task–Geometry Alignment (TGA)**。

直觉非常简单：

> 模型内部认为哪些东西“应该靠近”，是否和真正解决这个任务所需的结构一致？

---

# 5. 这比 t-SNE 强在哪里

不要让 t-SNE 成为主要证据。

t-SNE 最多放 appendix 做 visualization。

主实验应该是：

$$

TGA_l

$$

随 layer 的变化。

你可能最后会得到这种非常漂亮的图：

```text
TGA
│                         Formal DSL
│                    _____/
│               ____/
│          ____/
│     ____/                 JSON
│ ___/
│              Natural Language
│
└──────────────────────────── Layer
 early          middle        late
```

然后发现：

**高性能 representations 在 middle layers 更早形成与真实任务结构一致的 geometry。**

这就是一个非常 ACL 的 mechanistic claim。

---

# 6. 最关键实验：Geometry 能预测正确率吗？

对每个：

$$

(x,r,model)

$$

你都有：

- accuracy；
- $P(correct)$；
- TGA；
- token length；
- representation type；
- difficulty；
- input perplexity；
- model size。

然后做 mixed-effects regression：

$$

\operatorname{logit} P(correct)
=
\beta_0+
\beta_1TGA+
\beta_2Length+
\beta_3Difficulty+
\beta_4Perplexity+\dots

$$

instance 作为 random effect。

真正想看到的是：

$$

\beta_1>0

$$

而且控制 format identity、length、difficulty、模型规模后仍然显著。

换句话说：

> 不是“JSON 好”，也不是“prefix 好”。

而是：

> **只要某种表示诱导出来的内部 geometry 更匹配任务 computational structure，它就更容易成功。**

这个结论的理论价值高很多。

---

# 7. Killer Experiment：Activation Transplant

这里是整篇论文最值得押注的实验。

所有 representation 最后统一加同一个：

```text
<ANSWER>
```

或者：

```text
Answer:
```

于是对于同一个问题，你可以取得：

$$

h_l^{good}(x)

$$

以及：

$$

h_l^{bad}(x)

$$

它们都是同一个 `<ANSWER>` token 的 residual-stream representation。

假设：

Natural Language accuracy = 43%

DSL accuracy = 78%

那么实验：

$$

h_l^{NL}(x)
\leftarrow
h_l^{DSL}(x)

$$

然后从该层继续 forward。

如果：

$$

43\%\rightarrow65\%

$$

这就非常有力。

反过来：

$$

h_l^{DSL}(x)
\leftarrow
h_l^{NL}(x)

$$

如果：

$$

78\%\rightarrow52\%

$$

你的故事就闭环了：

$$

Representation
\rightarrow Geometry
\rightarrow Reasoning

$$

---

# 8. 但 reviewer 会说：你是不是把答案一起 transplant 过去了？

所以还需要第二个、更漂亮的实验：

## Representation Steering Vector

在训练集问题上：

$$

v_l
=
E_x
[
h_l^{good}(x)-h_l^{bad}(x)
]

$$

注意使用完全不同的 instances。

测试时：

$$

\tilde h_l^{bad}(x)
=
h_l^{bad}(x)+\alpha v_l

$$

如果在 unseen problems 上：

$$

Accuracy(bad+\text{steering})
>
Accuracy(bad)

$$

就很强。

因为此时你没有复制这个问题的答案。

你只是把模型推向：

> **“处理这种任务时，好的 representation 通常诱导出来的区域”。**

这可以命名为：

**Representation Geometry Steering (RGS)**。

---

# 9. 一定要加的几个 causal control

这里 reviewer 很可能会攻击，所以提前堵住。

| Intervention | 预期结果 |
|---|---:|
| good → bad steering | ↑ accuracy |
| bad → good steering | ↓ accuracy |
| random vector, norm matched | ≈ 0 |
| shuffled representation pairs | ≈ 0 |
| good representation from wrong instance | 很弱或负 |
| wrong task steering vector | ≈ 0 |
| steering middle layers | 最大 |
| steering very early/late layers | 较小 |

尤其：

$$

v_{\text{good-bad}}

$$

最好在 answer labels 上严格 balance。

进一步可以把 answer direction 从 steering vector 中投影掉：

$$

v' = v-\operatorname{Proj}_{answer}(v)

$$

如果仍然有效，就更难被 reviewer 解释为“答案泄漏”。

---

# 10. 还要解决一个非常大的 confound：预训练频率

假设：

```text
{"a": 1, "b": 2}
```

比某个 DSL 表现好。

reviewer 会马上说：

> 不是 geometry，是 JSON 在 pretraining 里见得多。

所以一定加入：

## Familiar vs Ciphered Syntax

例如：

```text
ADD(a,b)
```

变成：

```text
K7(a,b)
```

其中：

$$

ADD\rightarrow K7

$$

是完全随机的一一映射。

结构不变，只改变 vocabulary familiarity。

甚至做：

```text
NODE
EDGE
SOURCE
TARGET
```

vs

```text
ZAF
MIP
TOV
KEL
```

如果结构保存之后性能/TGA 仍然维持相当一部分优势，你才能说：

> **structure，而不仅仅是 pretraining familiarity，在起作用。**

---

# 11. 最值得做的 factorial experiment

比“随便选十几种语言”更漂亮的是主动操纵表示属性。

例如：

$$

2\times2\times2

$$

三个因素：

| Factor | - | + |
|---|---|---|
| Structure | dependencies implicit | dependencies explicit |
| Order | random order | causal/topological order |
| Familiarity | synthetic symbols | familiar syntax |

于是你能回答：

$$

\text{Performance} =
f(S,O,F)

$$

同时看：

$$

TGA=f(S,O,F)

$$

例如最后发现：

**explicit structure 和 causal ordering 提升 TGA，而 familiarity 主要影响 early layers。**

这会让论文从“format comparison”升级成真正的科学实验。

---

# 12. 模型怎么选

核心实验不需要追求十几个模型。

一个比较稳妥的配置是：

| Purpose | Models |
|---|---|
| main mechanistic analysis | 7B–14B open-weight instruction models × 3 families |
| scaling | 同一 family 的 3 个 sizes |
| post-training effect | base vs instruct / reasoning |
| optional external validation | 一个较大的 open-weight model |

原因很简单：

你需要大量 hidden-state extraction 和 activation intervention。

所以闭源 API 模型基本没有意义。

而且有一个很值得回答的问题：

$$

\text{Does scaling eliminate representation dependence?}

$$

我的假设反而是：

**模型越大，surface sensitivity 会下降，但在高 difficulty 下 task-geometry alignment 仍然决定能力。**

这会是很好的 scaling result。

---

# 13. Direct Answer 要作为主设置

主实验建议：

**禁止 CoT，只输出答案。**

原因是你首先想研究：

$$

\text{input representation}
\rightarrow
\text{internal computation}

$$

如果让模型自由生成长 CoT，就同时引入了：

$$

input representation
+
output representation
+
trajectory length

$$

会很难解释。

之后再做一个 robustness experiment：

Direct Answer vs CoT。

ACL 2026 已经证明 reasoning trajectories 本身具有复杂的几何结构，所以把 CoT 放到 secondary analysis 更干净。

---

# 14. Evaluation 一定避免 prompt-sensitivity 的老坑

EMNLP 2025 有一篇很重要的反例工作指出，相当一部分所谓 prompt sensitivity 可能来自 evaluation protocol，例如 rigid answer matching，而不是模型真的改变了答案。

所以你的 benchmark 最好使用：

$$

\{A,B,C,D\}

$$

或者其他可程序验证的 finite answers。

这样：

$$

Accuracy

$$

和：

$$

P(correct)

$$

都可以严格算。

不要依赖 LLM-as-a-judge 作为核心指标。

这是设计上的一个明显优势。

---

# 15. 整篇论文最终应该形成这条证据链

这是我认为最理想的 ACL story：

**Experiment 1 — Behavioral Effect**

同一 latent problem，不同 representation：

$$

Accuracy_r

$$

显著不同。

↓

**Experiment 2 — Geometric Effect**

相同语义不同表示产生显著不同 hidden geometry。

↓

**Experiment 3 — Structural Alignment**

高性能 representation：

$$

TGA \uparrow

$$

低性能 representation：

$$

TGA \downarrow

$$

↓

**Experiment 4 — Prediction**

TGA 可以跨 representation、instance、model 预测正确率。

↓

**Experiment 5 — Causal Transplant**

good geometry → bad representation：

$$

P(correct)\uparrow

$$

reverse：

$$

P(correct)\downarrow

$$

↓

**Experiment 6 — Generalized Steering**

在训练问题上学习 geometry direction，在 unseen problems 上仍能改善性能。

这时候你的核心 conclusion 就不再是：

> LLM 对 prompt 很敏感。

而是：

> **Task representations act as computational interfaces: they reorganize internal states, and reasoning performance depends on how well this induced geometry reflects the latent structure of the problem.**

这个 conclusion 很强。

---

# 16. 一张图就能把整篇论文讲明白

论文 Figure 1 我甚至建议直接画成：

```text
                 SAME LATENT PROBLEM x
                         │
          ┌──────────────┼──────────────┐
          ↓              ↓              ↓
      Natural Lang.     JSON          DSL
          │              │              │
          ↓              ↓              ↓
      Geometry A     Geometry B     Geometry C
          │              │              │
      low TGA        medium TGA       high TGA
          │              │              │
          ↓              ↓              ↓
        42%             61%             79%

                           ↑
                           │
                 Geometry Steering
                   42% → 67%
```

一句话：

**Same semantics ≠ same computation.**

这就是整篇论文的视觉核心。

---

# 17. 怎样和原来的 Schema 论文拉开距离

这一点尤其重要。

原论文的核心概念是：

$$

schema

$$

但 schema 比较抽象，而且 KAI/KOI 本质上仍属于 proxy。原论文的实验证据主要说明不同 representations 会带来性能和 activation 差异。

你的论文最好**弱化 schema 这个词**。

把它替换成三个严格定义的东西：

$$

\text{Latent Task Structure}

$$

$$

\text{Representation-Induced Geometry}

$$

$$

\text{Task–Geometry Alignment}

$$

于是：

**Schema paper：**

> representation may shape schema.

你的论文：

> representation $r$ changes measurable hidden geometry $H$; the degree to which $H$ recovers ground-truth computational structure $G$ predicts reasoning accuracy; intervening on $H$ causally changes accuracy.

完全是两个证据等级。

---

# 18. ACL reviewer 最可能攻击什么

| Reviewer concern | 你的防守 |
|---|---|
| “This is just prompt sensitivity.” | ground-truth task geometry + causal intervention |
| “Different formats differ in token length.” | length-controlled conditions + regression |
| “JSON/Python are simply familiar.” | ciphered DSL |
| “t-SNE proves nothing.” | RSA/TGA/CKA + causal patching |
| “Correlation ≠ causation.” | activation transplant + steering |
| “Synthetic tasks don't matter.” | three structural regimes + external benchmark |
| “Answer evaluation is format-sensitive.” | exact solver / fixed answer space |
| “Steering copies the answer.” | cross-instance vector + answer-balanced training + projection control |
| “Only works on one model.” | three model families + scaling |
| “You selected the best layer after seeing test results.” | layer chosen on dev set |

如果这些都覆盖，论文完整度会很高。

---

# 19. External validity 可以加 PlanBench

主 benchmark 一定自己控制。

然后最后加一个现实 benchmark。

PlanBench 很适合，因为它本身就包含自然语言、PDDL、Mystery Blocksworld 等不同表征条件，而且 planning 有明确的符号结构和 verifier。

也可以利用 Planetarium；它有大规模 natural-language ↔ PDDL 数据，并且专门设计了语义等价性验证。

但这部分只作为：

**External Validation**

不要让它成为论文主实验，否则 representation 的很多变量不够受控。

---

# 20. 如果现在开始，我会把工作拆成三个版本

| Version | 内容 | 投稿强度 |
|---|---|---|
| V1 | multi-representation benchmark + accuracy + geometry | Findings 水平 |
| V2 | V1 + Task–Geometry Alignment + strong controls | 有 Main potential |
| **V3** | **V2 + causal transplant + cross-instance steering** | **最值得冲 ACL Main** |

我强烈建议直接以 **V3** 为目标。

因为目前 literature 已经发展到“prompt sensitivity + geometry + activation steering”都分别有人做了。你的贡献不能只是把三者放在一篇文章里，而应该证明一个新的机制：

$$

\boxed{
\text{Representation}
\rightarrow
\text{Task-Aligned Geometry}
\rightarrow
\text{Reasoning Ability}
}

$$

---

## 我会把最终 paper claim 压缩成这句话

> **LLMs do not solve only the problem they are given; they solve the geometry induced by how the problem is represented.**

再学术一点：

> **Semantically equivalent representations are computationally non-equivalent for LLMs because they induce hidden-state geometries with different degrees of alignment to the task’s latent structure.**

这就已经很像一篇论文的中心 thesis 了。

如果把这个方向继续往下落地，**下一步最值得做的是直接把 ReGeoBench 定义出来**：我可以进一步给你具体设计 **3 个任务 × 6 种 representation × difficulty generator × TGA 公式 × activation-patching 实现方案 × 主实验表格预期布局**，基本可以直接变成 ACL 的实验 blueprint。

-----

> [User is quoting/referring to: "**下一步最值得做的是直接把 ReGeoBench 定义出来**：我可以进一步给你具体设计 **3 个任务 × 6 种 representation × difficulty generator × TGA 公式 × activation-patching 实现方案 × 主实验表格预期布局**，基本可以直接变成 ACL 的实验 blueprint。"]
>
> 继续深化研究方案，做出更好的论文

我会把它从“有趣的机制研究”进一步收紧成一篇真正有 **ACL Main 冲击力** 的论文：重点强化 **理论可证伪性、benchmark 的不可替代性、因果证据链、跨任务泛化，以及 reviewer 难以用 prompt sensitivity 一句话解释掉的核心贡献**。我先核对 2025–2026 的相关工作，再给你一版更强、更完整的实验 blueprint。

现在有一个重要变化：**ACL 2026 已经把“prompt sensitivity 的内部表示解释”和“reasoning trajectory geometry + steering”分别做得很深入了**。前者甚至发现模板对 logits 的影响往往大于问题本身；后者已经能用轨迹几何预测正确性并进行 steering。

所以更强的论文不能只是“不同表示 → 不同 geometry → steering”。我们需要把 novelty 再往前推一层：**随机化操纵表示的结构属性，并证明“任务结构对齐”是 representation effect 的因果中介变量**。这会比上一版明显更难被归类为已有工作的组合。

还有一个值得把整个项目“升级”的点：**不要把 geometry 当成起点，而把“LLM 是否能从表面序列中恢复出任务的潜在计算结构”当成起点。**

这样论文的故事会从“格式改变 hidden states”升级成：

$$

\text{Representation distortion}
\rightarrow
\text{geometry recovery}
\rightarrow
\text{reasoning success}

$$

而且可以做出**完全相同 token 集合、只改变结构排列顺序**的最小对照。这样 token 长度、词汇、答案格式、预训练熟悉度等主要混杂因素一次性被压下去。这个设计也正面回应了 2026 年已有工作对“hidden-state geometry 其实可能只是 format confound”的警告。

可以。基于现在的文献位置，我建议把上一版方案再升级一次：**不要把论文中心放在“representation 会改变 geometry”，而要研究“representation 给任务结构造成了多大失真，以及 LLM 能否在层间恢复这个结构”。**

这样，论文从一个 prompt/format 现象研究，变成一个更完整的 **representation → computation → causality** 理论。

我认为现在最值得冲的版本是：

# Same Semantics, Different Computations
### Representation Distortion and Geometry Recovery in LLM Reasoning

核心 thesis 可以写成：

> **Semantically equivalent representations impose different distortions on the latent computational structure of a task. LLM reasoning succeeds when the model recovers this structure internally, and fails when such recovery is delayed or incomplete.**

它比上一版的

$$

Representation\rightarrow Geometry\rightarrow Performance

$$

更强，因为增加了一个**模型无关、可操纵、可测量的输入变量**：

$$

\boxed{
Representation\ Distortion
\rightarrow
Structure\ Recovery
\rightarrow
Reasoning
}

$$

这会成为整篇论文真正的骨架。

---

# 一、为什么要这样升级

目前三个相邻方向已经有人占住了。

原始 *Shaping Schema via Language Representation* 已经明确提出不同语言表示会改变 LLM 的内部激活和性能，因此再做“NL vs JSON vs DSL + hidden state visualization”创新度已经不够。

ACL 2026 的 *Understanding the Prompt Sensitivity* 更进一步：meaning-preserving prompts 在内部并不自然聚合，而且 prompt template 对 logits 的影响甚至常常大于问题本身。

另一篇 ACL 2026 Main 已经把 CoT 看作 representation-space trajectory，并且进行了 trajectory steering。

甚至已经有研究直接警告：

> hidden-state separation 可能只是 task format，而非 reasoning mechanism。

在去除 source identity、option count、response length 等 format confounds 后，原本极高的 probe accuracy 可以掉到 chance。

所以我们的论文必须回答一个这些工作没有回答的问题：

> **什么样的 representation 在计算意义上更适合某个 task？为什么？**

答案不应该是“JSON 比 NL 好”。

而应该是：

$$

\boxed{
\text{representation 是否保留了 latent task structure}
}

$$

---

# 二、把论文的理论核心改成“结构恢复”

设一个任务实例不是字符串，而是一个语言无关的计算对象：

$$

x=(G_x,q_x)

$$

其中：

$$

G_x=(V,E)

$$

是解决问题真正需要的 latent computational structure。

例如：

| Task | $G_x$ |
|---|---|
| arithmetic/program evaluation | computation DAG |
| logical inference | proof DAG |
| constraint solving | factor graph |
| planning | state/action dependency graph |

不同语言表示只是 renderer：

$$

r:x\rightarrow p_r

$$

它把同一个图 $G_x$ 序列化为 token sequence。

关键问题变成：

> renderer 在把 graph 压成一维 token sequence 时，破坏了多少任务结构？

这就是第一个新概念：

## Representation Structural Distortion

如果语义单元 $u_i$ 在 prompt 中的位置为：

$$

\pi_r(u_i)

$$

可以定义最简单的 **Edge Span Distortion**：

$$

D_{\text{ES}}(x,r)=
\frac{1}{|E|}
\sum_{(u_i,u_j)\in E}
\frac{|\pi_r(u_i)-\pi_r(u_j)|}{|V|-1}

$$

直觉非常清楚。

如果两个计算上直接依赖的节点：

```text
v3 depends on v1
```

在输入里离得非常远：

$$

D_{\text{ES}}\uparrow

$$

representation 对计算结构的破坏更严重。

如果相关元素始终局部相邻：

$$

D_{\text{ES}}\downarrow

$$

representation 更忠实地保留 task topology。

于是我们第一次获得：

$$

\boxed{
\text{representation quality}
}

$$

的**模型无关指标**。

不是：

> LLM 喜不喜欢这种格式？

而是：

> 这种 representation 对真正计算结构造成了多大 distortion？

这会显著提升论文的理论层级。

---

# 三、最漂亮的实验：只 permutation，不改任何 token

这是我认为整篇论文最应该押注的实验。

假设 underlying computation 是：

```text
v1 = ...
v2 = ...
v3 = f(v1,v2)
v4 = g(v3)
v5 = h(v2,v4)
```

产生三种 representation。

### Structure-aligned

```text
v1 ...
v2 ...
v3 ...
v4 ...
v5 ...
```

### Random

```text
v3 ...
v1 ...
v5 ...
v2 ...
v4 ...
```

### Anti-aligned

特意最大化 dependency edge span：

```text
v5 ...
v1 ...
v4 ...
v2 ...
v3 ...
```

注意这三个输入：

$$

\boxed{\text{token multiset完全一样}}

$$

答案一样。

语义一样。

长度一样。

词频一样。

syntax 一样。

唯一变化就是：

$$

\pi_r

$$

即**任务结构在线性序列中的排列方式**。

于是：

$$

D_{\text{aligned}}
<
D_{\text{random}}
<
D_{\text{anti}}

$$

如果出现：

$$

Acc_{\text{aligned}}
>
Acc_{\text{random}}
>
Acc_{\text{anti}}

$$

这个结果会比：

> “DSL 比 natural language 好 17%”

有价值得多。

因为 reviewer 很难再说：

“可能 DSL 在 pretraining 中更常见。”

---

# 四、把 ReGeoBench 改造成两个层次

上一版“3 tasks × 6 representations”还可以保留，但我建议改成：

## Layer A：Controlled ReGeoBench

真正用于因果分析。

每个 latent problem 做：

$$

3\ \text{structural orderings}
\times
2\ \text{lexicons}
=
6\ representations

$$

其中：

| Factor | Conditions |
|---|---|
| Structure ordering | aligned / random / anti-aligned |
| Symbol familiarity | familiar / ciphered |

例如：

```text
AND
OR
XOR
```

对应 ciphered：

```text
KEL
ZAF
MIP
```

提前从 tokenizer 中选择相同 token 数量、最好都是 single-token 的 aliases。

这样可以问两个非常漂亮的问题：

$$

\text{Structure effect}

$$

和

$$

\text{Familiarity effect}

$$

以及：

$$

Structure\times Familiarity

$$

interaction。

如果 ciphered representation 中依然存在强烈 ordering effect，就可以排除相当大一部分：

> “模型只是熟悉某个语法”

的解释。

---

# 五、三个任务不要随便选，要让它们对应三类结构

我推荐最终使用：

| Benchmark | latent structure | 核心能力 | difficulty |
|---|---|---|---|
| **Program-DAG** | computation DAG | compositional execution | depth / fan-out / reuse |
| **Proof-DAG** | proof graph | multi-hop logical inference | proof depth / branching / distractors |
| **Constraint-Graph** | factor graph | global constraint integration | variables / density / treewidth |

## Program-DAG

例如：

$$

v_1=3,\quad
v_2=7

$$

$$

v_3=(v_1+v_2)\mod 11

$$

$$

v_4=v_3\times v_2\mod 11

$$

$$

v_5=v_4-v_1\mod 11

$$

问：

$$

v_5=?

$$

difficulty 可以精确操纵：

$$

n,\ depth,\ branching,\ fanout,\ reuse

$$

尤其加入 shared intermediate variables，使它从 tree 变成 DAG。

---

## Proof-DAG

生成：

```text
A is a Zorb.
Every Zorb that is green is a Mep.
Every Mep is a Tarl.
...
```

但底层不是自然语言，而是程序自动生成的 proof graph：

$$

fact
\rightarrow rule
\rightarrow intermediate
\rightarrow rule
\rightarrow conclusion

$$

同时加入 distractor branches。

于是你知道：

$$

G^{proof}_x

$$

到底是什么。

这比普通 LogiQA 更好，因为 ground-truth reasoning graph 是你生成出来的。

---

## Constraint-Graph

例如人物—位置—时间 assignment。

变量：

$$

X_1,\ldots,X_n

$$

constraint：

$$

X_1<X_3

$$

$$

X_2\neq X_4

$$

$$

X_5=X_1+2

$$

形成 factor graph。

difficulty 不只是“prompt 变长”，而是：

$$

\text{constraint density}

$$

$$

\text{cycle density}

$$

甚至：

$$

\text{treewidth}

$$

这点很漂亮，因为你可以把 **representation difficulty** 和 **computational difficulty** 分开。

---

# 六、自然语言、JSON、DSL 仍然要做，但降级为 external validity

Controlled ReGeoBench 负责回答：

> causally, 什么 representation 属性在起作用？

之后再建立第二组：

## Natural Representation Suite

同一 latent instance 转为：

| Representation |
|---|
| Natural Language |
| JSON |
| S-expression |
| Table / tuple representation |
| canonical DSL |
| domain-specific formal notation |

然后验证：

$$

D_{\text{representation}}

$$

是否能够预测现实 formats 的 performance。

这样论文不会陷入两种极端。

只比较真实 format：

> confounds 太多。

只做人工 permutation：

> 太 synthetic。

现在是：

$$

\boxed{
controlled\ causality
+
naturalistic\ validity
}

$$

这是比较成熟的实验结构。

---

# 七、TGA 也应该升级，不要只用 cosine correlation

上一版定义：

$$

TGA=
\rho(K^{model},K^{task})

$$

是可行的，但 reviewer 很容易质疑：

> 为什么 task graph 中相邻节点，就一定应该在 hidden space 中 cosine 相似？

这个质疑是合理的。

所以新版本建议同时使用两种 measurement。

---

## 7.1 Non-parametric Task–Geometry Alignment

对 task graph：

$$

L_G=D-A

$$

构造 diffusion kernel：

$$

K_G=\exp(-\tau L_G)

$$

它表达的是：

> 两个节点在 task topology 中有多接近。

对第 $l$ 层 semantic-unit activations：

$$

H_l=
[h_1^{(l)},...,h_n^{(l)}]

$$

得到：

$$

K_l=H_lH_l^\top

$$

经过 centering / whitening。

然后：

$$

\boxed{
TGA_l=
CKA(K_l,K_G)
}

$$

相比 t-SNE，它至少有：

rotation invariance、scale robustness，而且直接输出一个 layer-wise scalar。

---

# 八、更重要：加 Structure Decodability

不要赌“graph structure 一定体现为 cosine geometry”。

额外训练一个极轻的 relation probe：

$$

P(E_{ij}=1|h_i,h_j)

$$

例如 bilinear：

$$

s_{ij}=h_i^\top W h_j

$$

然后最关键的是：

**cross-representation evaluation。**

例如：

在 aligned representation 上训练 probe；

在 anti-aligned / ciphered / NL / DSL 上测试。

如果：

$$

\text{edge structure}

$$

仍能被解码出来，就说明你测量的是 latent task structure，而不是 format。

所以机制部分最好同时报告：

$$

TGA_l

$$

和

$$

SD_l=\text{Structure Decodability}

$$

---

# 九、同时测 Format Decodability

这一点非常关键。

训练另一个 probe：

$$

FD_l=P(\text{representation type}|h_l)

$$

于是模型每一层有两个量：

$$

SD_l=\text{task structure information}

$$

$$

FD_l=\text{surface format information}

$$

我真正期待看到的是类似：

```text
early layers:
format >> structure

middle layers:
format ↓
structure ↑

late layers:
structure dominates on correct instances
```

而失败案例可能是：

```text
format information remains dominant
task structure never becomes cleanly recoverable
```

这样你的结论就比：

> representations form different clusters

强很多。

真正的结论会变成：

> **Successful reasoning corresponds to a transition from surface-dominated representation to task-structure-dominated representation.**

这非常像一个真正的 mechanism。

---

# 十、论文最重要的新概念：Geometry Recovery

因此我甚至建议弱化“TGA”这个名字，重点提出：

## Geometry Recovery Curve

$$

\mathcal R(x,r)
=
\{TGA_1,TGA_2,\ldots,TGA_L\}

$$

关注三个指标：

$$

AUC_{TGA}
=
\frac1L\sum_lTGA_l

$$

以及 recovery onset：

$$

l^*
=
\min
\{l:TGA_l>\eta\}

$$

以及最终 alignment：

$$

TGA_{\max}
=
\max_l TGA_l

$$

核心 hypotheses：

$$

D_{\text{surface}}\uparrow
\Rightarrow
l^*\uparrow

$$

$$

D_{\text{surface}}\uparrow
\Rightarrow
TGA_{\max}\downarrow

$$

以及：

$$

TGA_{\max}\uparrow
\Rightarrow
P(correct)\uparrow

$$

这就构成一个漂亮的 computational story：

> 好的 representation 并不是给模型增加知识，而是减少模型从 surface serialization 中重建 task topology 所需的内部计算。

---

# 十一、这是一个比“schema activation”更清楚的解释

你甚至可以重新定义论文里的直觉：

自然语言问题：

$$

G
\xrightarrow{\text{serialization}}
S

$$

模型真正需要做：

$$

S
\xrightarrow{\text{Transformer}}
\hat G
\xrightarrow{\text{reasoning}}
y

$$

其中：

$$

G

$$

是真实任务结构，

$$

S

$$

是输入序列，

$$

\hat G

$$

是模型内部恢复出来的计算结构。

那么错误有两类：

### Reconstruction failure

$$

\hat G\not\approx G

$$

模型根本没把问题结构恢复出来。

### Computation failure

$$

\hat G\approx G

$$

但是仍然算错。

这个 distinction 非常有价值。

因为过去：

> “LLM reasoning failure”

通常把两种错误混在一起。

你的论文可以第一次系统地区分：

$$

\boxed{
representation failure
\neq
reasoning failure
}

$$

这个我认为甚至比单纯 TGA 更值得作为论文贡献。

---

# 十二、因果实验也要升级：不要只做 activation transplant

ACL 2026 已经有相当成熟的 activation steering 和 causal mediation 工作，因此“加一个 steering vector”本身不再新。

你需要的是：

## Structure-specific intervention

先拿完全 token-matched 的：

$$

r^{aligned}

$$

和：

$$

r^{anti}

$$

在 train set：

$$

\Delta_l(x)
=
h_l(x,r^{aligned})
-
h_l(x,r^{anti})

$$

所有 label 严格 balance。

对：

$$

\{\Delta_l(x)\}

$$

做 PCA/SVD：

$$

U_l

$$

得到一个低秩 **representation-recovery subspace**。

测试时：

$$

\tilde h_l^{anti}
=
h_l^{anti}
+
\alpha U_lU_l^\top \bar\Delta_l

$$

关键在于：

$$

x_{train}\cap x_{test}=\varnothing

$$

所以不能说你把答案 copy 过去。

---

# 十三、定义一个非常直观的 causal metric

假设：

$$

A_{aligned}=75\%

$$

$$

A_{anti}=45\%

$$

steering 后：

$$

A_{anti+steer}=63\%

$$

定义：

## Representation Gap Recovery

$$

RGR=
\frac{
A_{anti+steer}-A_{anti}
}{
A_{aligned}-A_{anti}
}

$$

这个例子：

$$

RGR=60\%

$$

论文可以说：

> manipulating the recovered-structure subspace causally restores 60% of the performance gap induced solely by representation ordering.

这个 claim 非常漂亮。

---

# 十四、还必须做 reverse intervention

否则 reviewer 会说：

> steering 只是普遍增加 activation magnitude。

所以：

$$

aligned + v_{aligned\rightarrow anti}

$$

应该下降。

你最终希望看到：

| Intervention | Accuracy |
|---|---:|
| anti | 45 |
| anti + structure steering | **63** |
| anti + random norm-matched | 46 |
| anti + shuffled-pair steering | 47 |
| anti + wrong-task vector | 46 |
| aligned | **75** |
| aligned + reverse steering | **57** |

如果出现这种表，paper 的 causal section 会非常有说服力。

---

# 十五、再做一个更强的语义单元 patching

作为 mechanism localization。

renderer 本来就保存：

$$

semantic\ unit
\leftrightarrow
token\ span

$$

所以你可以取得：

$$

h_l(u_i)

$$

然后逐个 patch：

$$

h_l^{anti}(u_i)
\leftarrow
h_l^{aligned}(u_i)

$$

观察：

$$

\Delta\log P(y^*)

$$

由此得到：

$$

layer\times semantic\ node

$$

的 causal heatmap。

如果发现：

> patch prerequisite/dependency bottleneck nodes 在 middle layers 恢复最大；

这个结果会非常好看。

而且可以把 attention 与 MLP 拆开：

$$

Attn_l

$$

vs

$$

MLP_l

$$

看究竟是谁在进行 geometry recovery。

但这部分我建议作为第二层贡献，不要让 paper 因为 circuit analysis 变得失控。

---

# 十六、统计上不要只做 correlation，要做真正的 hierarchical analysis

每个 latent instance 都有多个 representation，所以天然适合 paired design。

主模型：

$$

\operatorname{logit}
P(correct_{irm})
=
\beta_0
+\beta_1D_{ir}
+\beta_2TGA_{irm}
+\beta_3Difficulty_i
+\beta_4Familiarity_r
+\beta_5Length_r
+u_i+v_m

$$

其中：

$$

u_i

$$

是 instance random effect，

$$

v_m

$$

是 model effect。

更关键的是比较：

### Model A

$$

Correct
\sim
Distortion+controls

$$

### Model B

$$

Correct
\sim
Distortion+TGA+controls

$$

如果加入 TGA 后：

$$

|\beta_{Distortion}|

$$

明显下降，而：

$$

\beta_{TGA}>0

$$

这提供 mediation evidence。

这里措辞最好谨慎：

**mediation evidence**

而不是光靠 regression 就声称完整 causal mediation。

真正的因果证据来自后面的 intervention。

---

# 十七、模型 scaling 部分可以回答一个更大的问题

这和原始 Schema paper 的“next frontier beyond scaling”联系起来。

我们可以问：

> scaling 到底是在消除 representation bottleneck，还是只提升 computation capacity？

定义：

$$

RSI=
Acc_{aligned}-Acc_{anti}

$$

Representation Sensitivity Index。

然后在一个模型 family 中比较：

$$

4B\rightarrow8B\rightarrow14B\rightarrow32B

$$

可能出现三种情况。

如果：

$$

RSI\downarrow

$$

说明 scale 提高了结构恢复能力。

如果：

$$

RSI\approx constant

$$

说明 representation bottleneck 并没有被 scaling 消除。

如果在 hard instances：

$$

RSI\uparrow

$$

反而说明更大的模型拥有更强计算能力，但是否能释放这种能力更加取决于 interface。

无论哪个结果，只要 Geometry Recovery 能解释它，都很有价值。

---

# 十八、甚至可以提出“Representation Equivalent Scaling”

这是很适合作为最后一个 memorable result 的分析。

假设：

$$

Acc(8B,\ aligned)
\approx
Acc(32B,\ anti)

$$

那么 representation improvement 相当于：

$$

4\times

$$

参数规模差异。

可以定义：

$$

RES=
\frac{
\text{parameter size needed under poor representation}
}{
\text{parameter size needed under good representation}
}

$$

不一定作为正式理论 metric，但 Figure 6 如果能画出这种结果，非常有传播力：

> **Better representation can substitute for model scale.**

这会把论文重新接回最开始那个宏大问题：

> Intelligence scaling 是否只有 parameter scaling？

---

# 十九、Direct Answer 仍然应该作为 main setting

这里上一版判断不变。

主设置：

```text
Return only one of: A, B, C, D.
```

答案由程序自动生成、验证。

不要依靠 LLM judge。

EMNLP 2025 已经证明，相当一部分所谓 prompt sensitivity 会被不合理的 log-likelihood scoring 或 rigid matching 夸大。

我们的设置因为输出空间严格有限：

$$

Y\in\{A,B,C,D\}

$$

所以 exact accuracy 是合法的。

选项位置在 dataset level balance，但同一个 latent instance 的所有 representation 必须保持相同 answer position。

CoT 放 secondary experiment。

因为一旦 CoT 成为 main：

$$

input\ representation
\rightarrow
output\ representation
\rightarrow
trajectory

$$

因果关系又混进去了。

---

# 二十、整个 paper 最终应该形成“四级证据”

我会把实验结构压成下面这张表：

| Evidence | Question | Core experiment |
|---|---|---|
| **E1 Behavioral** | representation distortion 是否影响 performance？ | token-matched structural permutation |
| **E2 Mechanistic** | 模型是否在内部恢复 task structure？ | TGA + cross-format structural decoding |
| **E3 Predictive** | structure recovery 是否解释 success？ | hierarchical prediction + difficulty controls |
| **E4 Causal** | 改变 recovered geometry 是否改变能力？ | subspace patching + cross-instance steering |

任何单层证据都不够。

四层同时成立，才构成：

$$

\boxed{
Representation
\rightarrow
Internal Structure Recovery
\rightarrow
Reasoning
}

$$

---

# 二十一、与最接近工作的 novelty 可以写得非常清楚

论文 Introduction 中我会直接放类似这样的对照：

| Work | Representation variation | Ground-truth task structure | Format deconfounding | Causal intervention |
|---|---:|---:|---:|---:|
| Shaping Schema | ✓ | limited | △ | ✗ |
| Prompt Sensitivity | ✓ | ✗ | △ | ✗ |
| Reasoning Trajectories | ✗/secondary | ✗ | — | ✓ |
| Format Probe study | ✓ | ✗ | ✓ | ✓ |
| **Ours** | **✓ randomized** | **✓** | **✓ token-matched** | **✓ structure-specific** |

原始 Schema 工作把 language representation 提升成研究问题，但实验证据主要说明表示会改变 performance/internal features。

ACL 2026 的 reasoning trajectory 工作研究的是 **生成推理过程在 latent space 中的 trajectory**，而不是“输入 representation 如何影响 latent task reconstruction”。

ACL 2025 的 multilingual activation-patching 工作则已经证明 concept 与 surface language 可以在一定程度上被因果分离，这反而给我们的“surface vs computational structure”分解提供了很好的前置依据。

所以我们的 novelty 应该叫：

> **computational structure recovery under semantically equivalent serializations**

而不是：

> representation geometry。

---

# 二十二、External validity：PlanBench 比 Planetarium 更适合后半部分

主实验一定用 ReGeoBench。

最后再拿真实 planning benchmark 验证。

PlanBench 本身就是为了把 planning 从 commonsense/retrieval 中分离出来，并支持 formal planning domains，因此适合作为 external reasoning validation。citeturn225031academia66

Planetarium 更适合作为 auxiliary evidence，因为它重点是 natural language → PDDL translation，而且提供严格的语义等价性检测；它说明 structured representation 的 semantic correctness 本身就是一个非平凡问题。

也就是说：

**ReGeoBench：**

$$

representation\rightarrow reasoning

$$

**Planetarium/PlanBench：**

real-world formal representation validation。

不要倒过来。

---

# 二十三、论文应该避免的几个坑

这版最需要警惕的是“指标本身看起来漂亮，但 reviewer 不相信它代表 computation”。

因此不要把下面这些当主证据：

t-SNE、UMAP、attention entropy、representation clustering、单一 linear probe、某一个 layer 的 cosine similarity。

这些全部只能是 supporting evidence。

最核心的证据应该是：

$$

\text{controlled input manipulation}

$$

+

$$

\text{cross-format structure recovery}

$$

+

$$

\text{out-of-sample causal intervention}

$$

特别是 2026 已有工作已经直接显示，极漂亮的 hidden-state cluster 可能完全来自 format confound。

---

# 二十四、我建议最终做成这样的 Figure 1

```text
            SAME LATENT TASK GRAPH G
                      │
        ┌─────────────┼─────────────┐
        │             │             │
        ▼             ▼             ▼
   aligned order   random order   anti-aligned
      D = .12         D = .47         D = .81
        │             │             │
        ▼             ▼             ▼

Layer 1   surface      surface       surface
          dominated    dominated     dominated

Layer 12  G recovered  partial G     weak G
             │             │           │
           TGA .71       TGA .46     TGA .24
             │             │           │
             ▼             ▼           ▼
           82%           61%         39%

                         │
                  structure steering
                         │
                         ▼
                     39% → 65%
```

一句话 caption：

> **Representations differ in how much they distort the latent task structure; reasoning succeeds when the model reconstructs that structure internally.**

这个 Figure 1 已经基本把整篇论文讲完。

---

# 二十五、论文标题我现在更推荐这三个

首选：

**Same Semantics, Different Computations: Representation Distortion and Geometry Recovery in LLM Reasoning**

如果偏 mechanism：

**Recovering the Problem Behind the Prompt: How LLMs Reconstruct Task Structure Across Representations**

如果偏 ACL 简洁风格：

**Representation Shapes Computation: Task-Structure Recovery in Language Model Reasoning**

我最喜欢第一个。

因为：

**Same Semantics, Different Computations**

本身就是一个非常强的记忆点。

---

# 二十六、最终 Contributions 最好只压成四项

论文里可以直接围绕这四个贡献构建：

1. **ReGeoBench**：具有 ground-truth computation graph、严格语义等价、token-matched representation interventions 的 benchmark。

2. **Representation Distortion + Geometry Recovery**：把“representation quality”从 format 名称变成任务结构失真，并测量 LLM 如何逐层恢复 latent structure。

3. **Mechanistic causal evidence**：cross-format structural decoding、semantic-unit patching 和 cross-instance structure steering，证明 structure recovery 不只是 correlational geometry。

4. **Scaling/generalization**：测试这种机制跨 task、model family、model scale 和自然 representation 是否成立。

这四项足以撑起 Main paper。

---

# 二十七、主结果表我甚至已经建议提前定下来

### Table 1 — Behavioral representation effects

| Model | Task | Aligned | Random | Anti | Cipher-Aligned | Cipher-Anti |
|---|---:|---:|---:|---:|---:|---:|

证明：

$$

D\uparrow\Rightarrow Accuracy\downarrow

$$

---

### Table 2 — Structure recovery explains reasoning

| Model | Distortion → Acc | TGA → Acc | SD → Acc | +controls |
|---|---:|---:|---:|---:|

重点不是 correlation 多高，而是加入 TGA/SD 后：

$$

Distortion\rightarrow Accuracy

$$

的 effect 显著被解释掉。

---

### Table 3 — Causal intervention

| Intervention | ΔAcc | Gap Recovery |
|---|---:|---:|
| structure steering | + | + |
| reverse steering | − | — |
| random vector | ≈0 | — |
| shuffled pairs | ≈0 | — |
| wrong task | ≈0 | — |

这张表是 Main/Findings 的分水岭。

---

# 二十八、真正的 killer figure：layer-wise Structure vs Format

横轴 layer。

两条曲线：

$$

Structure\ Decodability

$$

和

$$

Format\ Decodability

$$

分别画 correct / incorrect。

理想结果：

```text
                  correct
Structure       _________
              /
            /
----------/-----------------

Format  \________
        \
         \


                  incorrect
Structure     ____
            /
----------/-----------------

Format   \____________
```

如果你真的得到：

> correct samples 完成了 format → structure transition，而 incorrect samples 没有，

我认为这会成为整篇论文最有价值的 mechanistic observation。

---

# 二十九、先别立刻做全量实验，先跑一个“生死 pilot”

我建议第一阶段只做：

$$

ProgramDAG

$$

+

一个 7B–14B 模型，

+

500 个 latent problems，

+

aligned/random/anti 三种完全 token-matched conditions。

只检验四件事：

| Pilot criterion | 建议继续做 Main 的信号 |
|---|---|
| aligned vs anti accuracy | ≥约 5–10 pp |
| Distortion–accuracy | 明显单调 |
| middle-layer structure recovery | correct > incorrect |
| causal intervention | 能恢复 ≥约 20–25% gap |

如果前两个都没有：

这个 thesis 本身就需要重构。

如果前两个有、第三个没有：

说明“graph kernel geometry”可能不是正确 mediator，可以转向 relation decoding / attention flow。

如果前三个有、steering 不成功：

论文仍然可能是很好的 empirical/mechanistic Findings，但 Main claim 要减少因果措辞。

如果四个全有：

**直接把资源压到三任务、多模型和 external validity。**

---

# 三十、我认为这已经比上一版强了一整个层级

上一版的核心是：

$$

Representation
\rightarrow Geometry
\rightarrow Accuracy

$$

现在变成：

$$

\boxed{
Latent\ Task\ Structure
\rightarrow
Representation\ Distortion
\rightarrow
Internal\ Structure\ Recovery
\rightarrow
Reasoning
}

$$

并且每个箭头都有独立实验：

$$

G\rightarrow D

$$

由 renderer 精确控制；

$$

D\rightarrow Recovery

$$

由 layer-wise TGA/structure decoding 测量；

$$

Recovery\rightarrow Accuracy

$$

由 paired regression 检验；

$$

do(Recovery)\rightarrow Accuracy

$$

由 activation intervention 检验。

这才是我认为最有希望形成一篇**有明确 scientific object、有机制、有因果、有 benchmark、有实际含义的 ACL Main 论文**的版本。

最值得抓住的核心句不是之前的：

> LLMs solve the geometry induced by the prompt.

而是更进一步：

> **Reasoning requires reconstructing the problem behind the prompt. Representations matter because they determine how difficult that reconstruction is.**

这个命题比“好的 prompt 更好”大得多，也比抽象的 “schema shaping” 更容易实证、证伪和建立累积性研究。

如果按实际开工顺序，我会先实现 **Program-DAG generator + token-matched aligned/random/anti renderer + semantic-unit alignment + layer-wise structure probe**；这一组实验就能在很低成本下判断整个论文 thesis 是否成立，再决定是否扩展到 Proof-DAG 和 Constraint-Graph。

Would you like to focus next on the pilot experiment design or the paper’s formal theory and hypotheses?