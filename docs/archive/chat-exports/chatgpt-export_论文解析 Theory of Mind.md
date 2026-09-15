# 论文解析 Theory of Mind

> Audit note (2026-07-07): this exported chat contains poster/photo-derived readouts for
> **Believing is Seeing**. The full text/PDF was still unavailable in the checked public
> sources during the repo audit. Treat the mechanism language and all numeric readouts below
> as archived motivation only; do not use them as project evidence. See `EVIDENCE_LEDGER.md`.

> [图片内容不支持导出]
> 解析这篇论文

我先按海报来解读：这篇是 ACL 2026 Student Research Workshop 接收论文 **“Believing is Seeing: How Token Inflation Mechanistically Erodes Theory of Mind in Large Language Models”**，作者是 **Zhizhi Wang / Ruochen Zhang**。我目前只查到官方接收列表，没有找到公开全文/PDF，所以以下主要基于你拍到的海报内容来解析。

## 一句话总结

这篇论文的核心结论是：**大模型在短上下文里能做对“他人错误信念”推理，但当上下文被无关 token 拉长后，早期层里的“信念状态表征”会被破坏；把短上下文中的 belief-token 激活 patch 回长上下文，可以恢复 Theory of Mind 准确率。**

也就是说，长上下文不是简单让模型“忘了答案”，而是让模型内部早期表征阶段就没有稳住“角色相信什么”。

## 它研究的任务是什么？

论文用的是类似 **Sally-Anne false-belief task** 的 ToM 任务：Sally 把球放进篮子，离开后 Anne 把球移到盒子里；问模型 Sally 回来会去哪里找球。正确的 ToM 答案是 **basket**，因为 Sally 的信念停留在旧位置；错误但常见的现实答案是 **box**，因为球实际在那里。Sally-Anne 任务是经典 false-belief 测试，用来考察主体是否能区分“他人的信念”和“现实状态”。

这类 false-belief 任务也常被用来评估大模型的 Theory of Mind 能力；比如 Kosinski 的 ToM 评测就使用了多组 false-belief prompts 来测试 LLM。

## 实验设计很关键：固定故事，只增加无关上下文

海报第 2 部分显示，他们不是改变故事本身，而是：

**同一个故事，同一个问题，同一个正确答案，只增加上下文长度。**

例如目标故事可能是：

> Alex puts key in drawer, then leaves.
> Mia moves key to cupboard.
> Q: Where will Alex look?

正确 belief answer 是 **drawer**，现实 answer 是 **cupboard**。

然后他们通过 padding / Wikipedia / shuffled Wikipedia / dots 等方式把上下文拉长到 256、512、1024、2048 tokens。这样可以把变量隔离出来：**性能下降不是因为故事更难，而是因为 token inflation，也就是上下文变长。**

## 主要发现

**第一，长上下文会降低 ToM 准确率。**
海报里显示，在 2048 tokens 的情况下，模型更容易从“角色相信的位置”滑向“现实位置”。这说明 LLM 的 ToM 表现对上下文长度很脆弱。

**第二，真正有用的是早期层的 belief-token 激活。**
他们做了 activation patching：把短上下文里某些层、某些 token 的内部激活替换到长上下文运行中，看能不能救回答案。activation patching 是机制可解释性里常见的因果干预方法，用来测试某些中间激活是否对最终行为有因果作用。

结果是：**早期层，尤其海报上写的 layers 0–8，救援效果最好**。海报写到 256 → 2048 的 patching 能把准确率从约 **66% 提升到 82%**，提升约 **16 个百分点**。

**第三，只有 patch belief-token 有效。**
海报第 5 部分对比了 belief、reality、random、base。结论是：
**belief-token patching 是唯一明显提升准确率的干预；reality token 和 random patching 救不回 ToM。**

这很重要，因为它说明 patching 不是随便注入一点短上下文信息就有用，而是特定的“信念状态表征”有用。

**第四，失败不是“信念编码变晚了”。**
第 6 部分的 back-patching 结果显示：没有 late-to-early pair 能超过 2048-token baseline。海报结论是：**token inflation does not merely delay belief encoding**。
换句话说，长上下文不是让模型“晚一点才想起来信念”，而是早期 belief encoding 本身就被削弱了。

**第五，机制可能是 attention dilution。**
海报第 7 部分写到 belief-critical heads lose selective attention。大意是：一些对 belief 关键的 attention head，在上下文变长后，对 belief token 的选择性注意下降，注意力被更多无关 token 稀释。最终模型更容易根据现实状态回答，而不是根据角色信念回答。

**第六，现象跨语言存在。**
海报第 8 部分显示，英文、中文、日文、韩文都有正向 recovery。日文柱子最高，中文、韩文、英文也都有提升。作者的解释是，不同语言的 tokenizer efficiency 和 baseline ToM strength 会影响幅度，但机制不是英语特有的。

**第七，padding 格式不是主要原因。**
第 9 部分比较了 dots padding、Wikipedia text、shuffled Wikipedia。三者造成的下降相近，所以作者认为机制主要由 **context length** 驱动，而不是某种特殊 filler artifact。

## 这篇论文的贡献

我觉得它最有价值的地方不是说“LLM 长上下文会变差”——这个大家多少都知道；而是它进一步定位了：

**长上下文破坏的是早期 belief-state representation。**

这比普通 benchmark 分数更有解释力。它把一个行为现象：

> 长上下文下 ToM 答错了

推进到机制解释：

> belief-critical heads 的选择性注意被稀释 → 早期 belief 表征被污染 → 模型更容易输出 reality answer。

## 可以怎么理解标题 “Believing is Seeing”

标题很妙。ToM 要求模型回答“角色相信什么”，而不是“现实是什么”。但 token inflation 让模型内部更像是被现实状态牵引，仿佛“看到现实”覆盖了“建模他人信念”。所以 **Believing is Seeing** 可以理解成：当 belief representation 被破坏后，模型把“现实可见状态”当成了角色信念。

## 局限与我会追问的问题

这篇海报很漂亮，但从照片看还有几处需要全文确认：

1. **模型是哪一个？**
   海报没有清楚显示模型族、参数量、上下文窗口机制。不同模型的长上下文训练、RoPE scaling、attention 实现会影响结果。

2. **任务是否过窄？**
   Sally-Anne / false-belief 是经典 ToM 测试，但它仍只是 ToM 的一个切面。false-belief 做对不等于真正拥有完整社会认知模型。

3. **activation patching 解释需要谨慎。**
   Patching 能提供因果线索，但具体 patching metric、corruption construction、token 对齐方式都会影响结论；已有工作也提醒 activation patching 的方法细节会显著影响解释。

4. **attention dilution 是否是唯一机制？**
   可能还混有位置编码、recency bias、KV cache 中干扰、长程检索失败、实体绑定衰减等因素。海报把 attention dilution 作为核心解释，但全文里应该还需要更细的 ablation。

## 实际启发

对长上下文应用很有用：不要以为把全部聊天记录、全部文档、全部 memory 塞进去就一定更聪明。对于涉及“某人知道什么、相信什么、误解什么”的任务，模型可能在长上下文下反而更容易丢掉心理状态。

工程上可以考虑：

**把 belief state 显式结构化**，例如维护表格：人物 / 已知信息 / 错误信念 / 真实状态。
**把关键心理状态放近问题处**，不要埋在几千 token 前。
**做上下文压缩时保留 belief-critical tokens**，而不是只保留事实摘要。
**对 agent memory 做 ToM 专项测试**，尤其是长会话、多人对话、客服、法律、医疗问诊等场景。

这篇论文的 takeaway 可以概括成一句话：

> **可靠的长上下文社会推理，不只是需要更长窗口，而是需要保护“信念状态表征”不被上下文噪声稀释。**

-----

> 请你搜索这篇论文：Insufficient Task Description can Impair In-context Learning: A Study from Information Perspective，详细解释一下这篇论文

我找到了这篇：**Meidai Xuanyuan, Tao Yang, Jingwen Fu, Sicheng Zhao, Yuwang Wang, “Insufficient task description can impair in-context learning: A study from information perspective,” Information Fusion, Volume 120, August 2025, Article 103116, DOI: 10.1016/j.inffus.2025.103116**。公开页面能看到摘要、引言和部分章节片段；全文似乎需要机构访问，所以我下面不会编造具体图表数值，而是基于可访问内容和相关预印本/二手论文线索来解析。

## 一句话总结

这篇论文研究的是：**在 in-context learning 里，任务描述不是越有越好；如果任务描述给的信息不够充分，反而会让 Transformer 忽略 few-shot examples，导致 ICL 性能下降。只有当任务描述的信息量超过某个阈值后，它才从“干扰项”变成“有益提示”。** 论文把这个现象叫作任务描述的“双重作用”：低信息任务描述会损害 ICL，高信息任务描述会帮助 ICL，并且中间存在类似“性能涌现”的转折。

## 这篇论文到底在问什么？

传统 ICL 研究通常关注 **examples / demonstrations**：比如给模型 4 个例子、8 个例子，换不同示例顺序、不同示例质量，性能会怎么变。

但真实 prompt 往往不只有例子，还会有一句或一段 **task description**，例如：

> 判断下面句子的情感是正面还是负面。
> Example 1: ...
> Example 2: ...
> Query: ...

这篇论文的核心问题是：

> **当 prompt 同时包含任务描述和上下文示例时，模型如何融合这两类信息？任务描述的信息量不足时，会不会反而伤害模型从 examples 中学习的能力？**

作者认为，ICL 依赖两类上下文信息：**任务描述**和**in-context examples**；已有研究更多看 examples，任务描述的作用相对没有被系统研究。

## “信息视角”是什么意思？

论文把任务描述看成一种关于真实任务的“信息源”。设真实任务是 $t$，任务描述是 $d$，那么任务描述有多有用，可以用它和真实任务之间的互信息来理解：

$$

I(t; d)

$$

直观地说：

| 任务描述类型 | 信息量 | 对模型的作用 |
|---|---:|---|
| 没有描述或几乎没信息 | 很低 | 模型主要靠 examples 推断任务 |
| 含糊但看似有用的描述 | 低到中等 | 可能误导模型，让它不充分利用 examples |
| 足够精确的描述 | 高 | 缩小任务空间，帮助模型更快推断答案 |

最关键的是中间那种：**描述不是完全没用，而是“不够用”**。它给了模型一个粗糙方向，但不足以唯一确定任务；模型却可能过度依赖它，反而少看 examples。

一个生活化例子：

> 任务描述：请判断文本类别。
> Examples 显示任务其实是“判断是否包含法律风险”。
> Query 需要输出“高风险/低风险”。

如果描述只说“判断文本类别”，它太泛了；模型可能按照常见分类任务去理解，而没有认真从 examples 里归纳真正的分类标准。于是，**弱描述压制了 examples 的信号**。

## 合成实验怎么设计？

为了精确控制“任务描述的信息量”，作者设计了一个可控的算术任务。相关预印本/综述中给出的形式是：

$$

((a \cdot x) \circ (b \cdot y)) \bmod p = r

$$

其中 $(x,y)$ 是输入，$r$ 是输出，$\circ$ 是运算符，例如 $+$、$-$、$/$，任务由参数 $(a,b,\circ)$ 定义；任务描述则用类似 $(a_l,a_u,b_l,b_u,\circ)$ 的形式给出，即给出 $a,b$ 的范围和运算符，从而约束可能任务空间。

举个简化例子。真实任务可能是：

$$

r = (2x + 7y) \bmod 11

$$

高信息描述可以直接或几乎直接告诉模型：

> $a=2, b=7, \circ=+, p=11$

低信息描述可能只说：

> $a$ 在 1 到 10 之间，$b$ 在 1 到 10 之间，运算是加法。

这时模型还需要从 few-shot examples 推出 $a=2,b=7$。作者通过改变描述里给出的范围宽窄，系统控制任务描述携带的信息量。范围越窄，描述越有信息；范围越宽，描述越模糊。

这个设计非常聪明，因为它把真实 NLP 任务里很难量化的“prompt 清楚不清楚”，变成了可以控制的互信息问题。

## 主要发现 1：低信息任务描述会让模型忽略 examples

论文最重要的发现是：**当任务描述信息不足时，模型的 ICL 表现会变差，因为它没有充分利用上下文示例。** ScienceDirect 摘要明确写到，insufficient task description 会让模型 overlook in-context examples，从而导致较差的 ICL 表现。

这和很多人的直觉相反。我们通常以为：

> 多给一句任务描述，总比不给好。

但论文说：不一定。因为任务描述不是独立加分项，它会和 examples 争夺模型注意力与推断权重。一个不够具体的描述，可能会把模型带到错误的“任务先验”里。

可以理解成：

> **没有描述时，模型被迫从 examples 中学习；
> 描述很清楚时，模型能结合描述和 examples；
> 描述半清不楚时，模型以为自己懂了，于是少看 examples。**

## 主要发现 2：存在“阈值”或“转折点”

论文不是简单说“任务描述有害”，而是说任务描述有双重作用：

**信息量低时，有害；信息量足够高时，有益。**

当任务描述中的信息超过某个阈值后，它的作用从负面变成正面，并出现 performance emergence，也就是性能突然明显提升的现象。

这可以用“假设空间”来解释：

假设模型不知道真实任务是哪一个，只知道可能有很多候选任务。任务描述的作用，是缩小候选任务空间。

如果描述只把候选空间从 1000 个缩到 800 个，其实没帮太多忙，但它可能已经足够显眼，让模型错误地依赖它。

如果描述把候选空间从 1000 个缩到 5 个，那它就很有用了；examples 只需要在这 5 个候选里做最终辨别。

所以这里的核心不是“描述长度”，而是**描述信息量是否足够识别任务结构**。

## 主要发现 3：更多 examples 可以缓解低信息描述的负面影响

论文还发现，低信息任务描述的负面影响不是不可救的。有两种情况可以缓解：

第一，**提供大量 in-context examples**。当 examples 足够多时，模型能从示例中积累足够证据，抵消含糊描述带来的干扰。

第二，**提高任务描述的信息量**。当描述足够具体，模型不再被模糊信息误导，反而能更好地融合描述和示例。

这对 prompt engineering 很有启发：如果任务描述很难写清楚，那就需要更多、更覆盖边界情况的 examples；如果 examples 很少，那任务描述必须足够精确。

## 主要发现 4：任务预测作为辅助任务有帮助

论文还研究了 **task prediction as an auxiliary task**，即让模型不仅预测 query 的答案，也学习预测/识别当前任务本身。公开摘要里提到，他们发现任务预测通常能增强 ICL。

这个结果很好理解：如果模型被训练或引导去显式识别“我现在到底在做什么任务”，它就更不容易被模糊描述带偏，也更可能认真整合 examples。

类比到大模型 prompt，就是可以加一步：

> 先根据任务描述和示例，总结你认为当前任务的规则；再回答 query。

这其实是在强迫模型做“任务识别”，而不是直接跳到答案。

## 主要发现 5：现象在真实 NLP 任务和 GPT-4 上也出现

作者不只做了合成任务，还把发现扩展到若干真实 NLP 任务，并在 GPT-4 上复现了类似的双重作用：任务描述不足时会伤害 ICL，信息量足够时会帮助 ICL。

这点很重要，因为合成任务虽然可控，但容易被质疑“不够真实”。GPT-4 和真实 NLP 任务上的验证说明，这不是只存在于模运算玩具任务里的现象，而是可能普遍影响实际 prompt 设计。

不过，公开预览没有给出完整任务列表和详细数值，所以我不在这里编造具体 benchmark 名称或提升幅度。

## 这篇论文的机制解释

论文的机制解释可以概括为：

> **任务描述和 examples 是两种信息源。模型必须融合它们；但当任务描述低信息、不充分时，它可能成为一个强但不可靠的先验，使模型减少对 examples 的依赖。**

从 Transformer 的角度看，这可能表现为 attention 分配问题：任务描述 token 很靠前、语义上很“权威”，模型可能把它当作任务锚点。如果这个锚点很模糊，模型就会在错误或过宽的任务空间里推断；examples 本该帮助修正这个推断，但模型没有充分利用它们。

作者也承认，虽然做了一些 attention-based analysis，但还没有给出完整的转折机制解释；他们在附录中尝试用简化的一层 Transformer attention 机制解释该现象。

## 和普通 prompt engineering 的关系

这篇论文很适合转化成实用原则。

很多人写 prompt 时会这样写：

> 请完成以下分类任务。
> 下面是几个例子。
> 请输出答案。

这类描述看起来“有任务说明”，但可能信息量不足。根据这篇论文，这种模糊描述有时不如明确告诉模型：

> 你要判断文本是否表达了购买意图。
> 只在用户明确表示想买、询价、下单、比较商品时输出 YES。
> 只是浏览、抱怨、闲聊、售后问题时输出 NO。
> 请优先遵循 examples 中展示的边界规则。

也就是说，任务描述要提供**判别规则、标签定义、边界条件、输出格式、优先级**，而不是只给一个任务名字。

## 我对这篇论文的评价

我觉得这篇论文最有意思的点，是它挑战了一个非常常见的假设：

> prompt 里多写一点任务说明总是有帮助的。

它说明：**任务说明本身也是一种信息融合信号，质量不足时会干扰模型从示例中学习。**

这对 ICL 理论和实际使用都有价值。理论上，它把 task description 从“prompt 前缀”提升为一个可分析的信息源；实践上，它解释了为什么有些 few-shot prompt 明明给了示例，模型还是“不按例子来”。

## 局限性

这篇论文也有几个明显限制。

第一，合成任务虽然可控，但和真实语言任务仍有差距。模运算任务能很好地控制互信息，但真实任务里的“描述信息量”更难量化。

第二，公开预览显示作者承认还没有完整解释 transition effect，也就是为什么从负面到正面的转折会突然发生；attention 分析只是部分解释。

第三，GPT-4 上的复现说明现象有现实意义，但不同模型、不同指令模板、不同领域任务里阈值可能不同。

## 最核心 takeaway

这篇论文可以用一句话记住：

> **few-shot examples 不会自动被模型充分利用；如果任务描述太模糊，模型可能反而被描述带偏，忽略示例。**

所以做 ICL 时，最好的 prompt 不是“模糊说明 + 一堆例子”，而是：

> **足够具体的任务描述 + 高质量覆盖边界的 examples + 必要时让模型先显式归纳任务规则。**

-----

> 这两篇论文怎么进行结合

可以把这两篇结合成一个非常自然的研究问题：

> **长上下文下，模型为什么会丢失社会推理能力？是不是因为“任务描述信息不足”使模型无法稳定识别当前任务，而 token inflation 又进一步稀释了 belief-critical tokens，最终导致模型忽略示例、混淆“他人信念”和“现实状态”？**

换句话说，第一篇论文关注 **任务描述的信息量如何影响 ICL**，第二篇关注 **长上下文如何破坏 Theory of Mind 中的 belief representation**。两者可以合成一个更完整的框架：

> **ICL 中的社会推理失败 = 任务识别不充分 + 长上下文导致信念表征被稀释。**

---

## 1. 两篇论文的共同核心

这两篇看起来研究对象不同，但其实都在问同一个底层问题：

**模型到底有没有正确使用上下文里的关键信息？**

第一篇 **Insufficient Task Description can Impair In-context Learning** 说的是：

> 如果任务描述不够明确，模型可能会过度依赖模糊描述，反而忽略 examples。

第二篇 **Believing is Seeing** 说的是：

> 如果上下文太长，模型的 belief-state representation 会被 token inflation 稀释，导致它不能正确区分“角色相信什么”和“事实是什么”。

所以它们的共同点是：

| 论文 | 干扰源 | 被破坏的信息 | 表现出的错误 |
|---|---|---|---|
| Task Description 论文 | 任务描述信息不足 | task identity / task rule | 忽略 examples |
| Believing is Seeing 论文 | 上下文 token 过多 | belief-state representation | 把现实状态当作他人信念 |

结合后可以研究：

> **当任务描述不充分时，长上下文是否会更严重地破坏模型对 belief examples 的使用？**

---

## 2. 一个统一解释：信息竞争框架

可以把 prompt 里的信息分成四类：

1. **任务描述 D**：告诉模型应该做什么。
2. **示例 E**：通过 few-shot examples 展示任务规则。
3. **信念关键 token B**：比如 “Sally left the room before Anne moved the ball”。
4. **无关上下文 C**：padding、Wikipedia、长对话历史、背景噪声等。

模型要回答 ToM 问题，本质上需要推断：

> 当前任务 T 是什么？
> 角色的 belief state 是什么？
> 输出应该遵循 belief 还是 reality？

第一篇论文说明：
如果 $D$ 的信息量不足，模型对 $T$ 的推断会变差。

第二篇论文说明：
如果 $C$ 太长，模型对 $B$ 的表征会变差。

合起来就是：

$$

\text{ToM success} \approx f(\text{task information}, \text{belief representation}, \text{context length})

$$

更直观地说：

> **任务描述负责告诉模型“要看什么”；belief tokens 负责提供“该看的内容”；长上下文负责制造干扰。**

如果任务描述不清楚，模型不知道应该优先看 belief。
如果上下文太长，就算模型知道要看 belief，也可能找不到或表征不稳。
如果两者同时发生，ToM 会出现叠加甚至非线性崩溃。

---

## 3. 最有价值的结合方向：研究“任务描述能否保护 belief representation”

可以提出一个新的核心假设：

> **高信息量任务描述可以缓解 token inflation 对 Theory of Mind 的损害，因为它能让模型更稳定地关注 belief-critical tokens。低信息量任务描述则会放大 token inflation 的负面影响。**

这就把两篇论文真正连起来了。

### 假设 1：低信息任务描述 + 长上下文，会造成最严重的 ToM 崩溃

比如下面这种 prompt：

> Read the story and answer the question.

这个任务描述信息量很低。它没有告诉模型：

- 要区分 belief 和 reality；
- 要根据角色看到的信息回答；
- 角色不知道后续发生的事情；
- 不能用真实物体位置回答。

在短上下文里，模型可能还能从 examples 里学出来。
但在长上下文里，examples 和 belief tokens 被稀释，模型更容易退回到“现实答案”。

所以预期结果是：

| 任务描述 | 短上下文 | 长上下文 |
|---|---:|---:|
| 无描述 | 中等 | 下降 |
| 低信息描述 | 可能比无描述更差 | 最差 |
| 高信息描述 | 最好 | 下降较小 |

这里最关键的现象是：**低信息描述可能不如没有描述。**

这正好对应第一篇论文的结论。

---

## 4. 可以设计一个新的实验矩阵

实验可以直接基于第二篇的 Sally-Anne / false-belief task，但加入第一篇的变量：**任务描述信息量**。

### 自变量 1：任务描述信息量

可以设置四档。

**Level 0：无任务描述**

> Answer the question.

**Level 1：低信息任务描述**

> Read the story and answer where the character will look.

这句话看似有用，但没有明确 belief/reality 区分。

**Level 2：中信息任务描述**

> Answer based on what the character knows, not necessarily where the object really is.

这已经提示了 false belief，但还没有系统规则。

**Level 3：高信息任务描述**

> Track each character’s belief state. If a character leaves before an object is moved, they do not know the new location. When asked where the character will look, answer using that character’s belief, not the real object location.

这个描述信息量很高，明确规定了任务规则。

---

### 自变量 2：上下文长度

沿用第二篇的 token inflation 设置：

- 256 tokens
- 512 tokens
- 1024 tokens
- 2048 tokens
- 4096 tokens

中间插入无关文本、Wikipedia padding、dots padding、长对话历史等。

---

### 自变量 3：examples 数量

沿用 ICL 视角：

- 0-shot
- 2-shot
- 4-shot
- 8-shot
- 16-shot

这样就可以观察：

> 高信息任务描述和更多 examples，哪个更能抵抗长上下文损害？

也可以验证第一篇论文的结论：

> 当任务描述不足时，增加 examples 是否能缓解损害？

---

## 5. 因变量应该怎么测？

不只是看准确率，还可以测更细的东西。

### 1. ToM accuracy

模型是否回答 belief answer，而不是 reality answer。

比如：

- 正确：Sally will look in the basket.
- 错误：Sally will look in the box.

### 2. Reality-bias rate

模型有多大比例输出现实位置。

这个指标很重要，因为 ToM 错误不是随机错，而是系统性滑向 reality answer。

### 3. Example usage score

测试模型是否真正使用了 few-shot examples。

可以构造一些 examples 中才体现的规则，比如：

- 角色是否看到移动事件；
- 角色是否被告知新位置；
- 角色是否误以为物体被移动；
- 二阶信念：A 认为 B 知道什么。

如果模型答题只靠任务描述，而不利用 examples，它会在这些边界案例上出错。

### 4. Belief representation probe

在中间层训练 probe，看模型内部是否编码了：

- 角色相信的位置；
- 物体真实位置；
- 角色是否知道移动事件；
- 当前问题应该根据 belief 还是 reality 回答。

这能把 behavioral result 和 mechanistic result 连起来。

---

## 6. 机制实验：把两篇论文的 activation patching 结合起来

第二篇用了 activation patching 来证明：短上下文中的 belief-token activation 能救回长上下文 ToM。

结合第一篇后，可以做更有意思的 patching：

### 实验 A：高信息描述 → 低信息描述 patching

把高信息任务描述条件下的某些激活，patch 到低信息任务描述条件下。

如果性能恢复，说明：

> 高信息任务描述确实在模型内部形成了更好的 task representation。

### 实验 B：短上下文 belief activation → 长上下文 patching

沿用第二篇：

> 把短上下文里的 belief-token activation patch 到长上下文。

看它在不同任务描述信息量下的效果。

关键预测是：

| 条件 | patching 效果 |
|---|---|
| 高信息描述 + 长上下文 | patching 恢复较小，因为原本已经保护得较好 |
| 低信息描述 + 长上下文 | patching 恢复最大，因为 belief representation 损坏最严重 |
| 无描述 + 长上下文 | 中等恢复 |
| 错误描述 + 长上下文 | 可能难以恢复，因为 task representation 本身错了 |

### 实验 C：description token vs belief token patching

可以比较 patch 哪类 token 最有用：

- patch task-description tokens；
- patch example-label tokens；
- patch belief-critical tokens；
- patch reality tokens；
- patch random tokens。

如果任务描述论文的机制成立，那么高信息 description token 的 patching 应该能改善任务识别。
如果第二篇机制成立，那么 belief-critical token 的 patching 应该能恢复 ToM。
如果两者结合成立，那么最强的恢复可能来自：

> **task-description activation + belief-token activation 同时 patch。**

---

## 7. 可以提出一个新模型：Task-Belief Information Bottleneck

这两篇结合后，可以提出一个漂亮的理论框架：

> **Task-Belief Information Bottleneck**

模型在 ICL 中需要经过两个瓶颈：

### 第一层瓶颈：任务识别瓶颈

模型要先知道：

> 我现在是在做什么任务？

这是第一篇论文关心的。

任务描述不足时，模型无法稳定识别任务，或者识别成一个过宽、错误的任务。

### 第二层瓶颈：信念表征瓶颈

模型要再知道：

> 在这个任务里，哪个角色相信什么？

这是第二篇论文关心的。

上下文过长时，belief-critical tokens 的表征被稀释，模型不能稳定维护角色信念。

所以 ToM ICL 成功需要同时通过两个瓶颈：

$$

\text{Correct Answer}
=
\text{Correct Task Identification}
+
\text{Correct Belief State Tracking}

$$

失败模式也有两类：

| 失败类型 | 来源 | 表现 |
|---|---|---|
| 任务识别失败 | 描述信息不足 | 模型不知道要按 belief 回答 |
| 信念追踪失败 | token inflation | 模型知道规则，但找不到/保不住 belief state |

这个框架非常适合写成一篇后续论文。

---

## 8. 可能的论文题目

如果你要基于这两篇做一个新工作，可以考虑这些题目：

**Task Descriptions Protect Belief States: An Information Perspective on Long-context Theory-of-Mind In-context Learning**

或者：

**When Instructions Fail to Protect Beliefs: Task Information and Token Inflation in Long-context Social Reasoning**

或者更简洁一点：

**Task Information Mitigates Token Inflation in Theory-of-Mind In-context Learning**

---

## 9. 预期贡献可以写成三点

### 贡献 1：统一两个现象

之前的研究分别说明：

- 任务描述信息不足会损害 ICL；
- 长上下文会损害 ToM belief representation。

你的工作可以说明：

> **这两个现象不是独立的，而是在长上下文社会推理中相互作用。**

---

### 贡献 2：发现任务描述的信息阈值会随上下文长度变化

这是很有潜力的发现。

第一篇说任务描述存在信息阈值。
第二篇说长上下文会带来 token inflation。

结合后可以提出：

> **上下文越长，任务描述需要越高的信息量，才能保护 ICL。**

也就是：

$$

\text{Required task information} \uparrow
\quad \text{as} \quad
\text{context length} \uparrow

$$

这会是一个很强的结论。

实际含义是：

> 在短 prompt 里，一句简单说明可能够用；
> 在长上下文 agent 或长文档问答里，任务描述必须更明确、更结构化。

---

### 贡献 3：提出一种实用干预方法

你可以提出一种 prompt 或模型干预方法：

**Belief-aware task description**

例如在长上下文 ToM prompt 前加入：

> Your task is to answer from the queried character’s belief state.
> Track what each character has observed.
> Do not use facts that the queried character did not witness or learn.
> Before answering, identify the character’s belief about the object location.

然后和普通 prompt 对比。

如果这个 prompt 能显著缓解长上下文 ToM 下降，就说明：

> 高信息任务描述不仅提升 ICL，而且能保护 belief representation。

---

## 10. 最自然的研究假设

可以把最终 research hypotheses 写成这样：

### H1：低信息任务描述会放大 token inflation 对 ToM 的损害

在长上下文中，低信息任务描述比无任务描述更容易导致模型输出 reality answer。

### H2：高信息任务描述能提高 belief-critical attention

高信息任务描述会让模型在中间层/早期层对 belief-critical tokens 保持更高注意力。

### H3：上下文越长，任务描述的信息阈值越高

任务描述从有害变有益的转折点，会随着 context length 增加而右移。

### H4：activation patching 可以区分 task failure 和 belief failure

如果 patch task-description activations 有效，说明错误来自任务识别。
如果 patch belief-token activations 有效，说明错误来自信念表征。
如果两者都需要，说明长上下文 ToM 失败是双重瓶颈造成的。

---

## 11. 一个很具体的实验例子

比如你构造如下任务。

### 高信息描述条件

> In this task, answer based on what the target character believes.
> A character only knows events they observed or were told.
> If an object moves while the character is absent, the character still believes it is in the old location.

### 低信息描述条件

> Read the story and answer the question.

### 故事

> Sally put the key in the drawer.
> Sally left the room.
> Anne moved the key to the cupboard.
> Sally came back.
> Where will Sally look for the key?

然后在故事前后加入 256、512、1024、2048 token 的无关上下文。

预期结果：

| 描述类型 | 256 tokens | 2048 tokens |
|---|---:|---:|
| 无描述 | 可以做对一些 | 明显下降 |
| 低信息描述 | 短上下文可能还行 | 严重下降 |
| 高信息描述 | 最稳 | 下降最小 |

更进一步，如果加 few-shot examples：

| examples 数量 | 低信息描述下的表现 |
|---:|---|
| 0-shot | 差 |
| 2-shot | 仍不稳定 |
| 8-shot | 开始恢复 |
| 16-shot | 明显恢复 |

这就能复现第一篇的 “更多 examples 可以缓解 insufficient task description”，同时放到第二篇的长上下文 ToM 场景里。

---

## 12. 这项结合研究的最终结论可能是什么？

最理想的结论是：

> **长上下文 ToM 失败不只是 memory/retrieval 问题，而是 task information 和 belief representation 的联合失效。低信息任务描述会让模型不能稳定识别“应该按 belief 回答”的任务目标；token inflation 会进一步稀释 belief-critical tokens。高信息、belief-aware 的任务描述可以部分恢复长上下文下的 ToM 能力。**

这比单独两篇都更进一步。

第一篇告诉我们：任务描述信息量影响 ICL。
第二篇告诉我们：token inflation 破坏 ToM 表征。
结合后可以告诉我们：

> **任务描述的信息量决定了模型能否在长上下文中保护正确的社会认知表征。**

这个方向非常有潜力，既有理论价值，也有 prompt engineering 和 long-context agent 的实际价值。
