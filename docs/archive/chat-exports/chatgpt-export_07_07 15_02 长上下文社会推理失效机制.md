# 07/07 15:02 长上下文社会推理失效机制

> Audit note (2026-07-07): this exported chat contains synthesis built partly from
> **Believing is Seeing** poster/photo readouts. The full text/PDF was still unavailable in
> the checked public sources during the repo audit. Treat those poster-derived mechanisms and
> numeric details as hypotheses or motivation only; do not use them as project evidence. See
> `EVIDENCE_LEDGER.md`.

> [文件不支持导出]
> 可以把这两篇结合成一个非常自然的研究问题：
>
> > **长上下文下，模型为什么会丢失社会推理能力？是不是因为“任务描述信息不足”使模型无法稳定识别当前任务，而 token inflation 又进一步稀释了 belief-critical tokens，最终导致模型忽略示例、混淆“他人信念”和“现实状态”？**
>
> 换句话说，第一篇论文关注 **任务描述的信息量如何影响 ICL**，第二篇关注 **长上下文如何破坏 Theory of Mind 中的 belief representation**。两者可以合成一个更完整的框架：
>
> > **ICL 中的社会推理失败 = 任务识别不充分 + 长上下文导致信念表征被稀释。**
>
> ---
>
> ## 1. 两篇论文的共同核心
>
> 这两篇看起来研究对象不同，但其实都在问同一个底层问题：
>
> **模型到底有没有正确使用上下文里的关键信息？**
>
> 第一篇 **Insufficient Task Description can Impair In-context Learning** 说的是：
>
> > 如果任务描述不够明确，模型可能会过度依赖模糊描述，反而忽略 examples。
>
> 第二篇 **Believing is Seeing** 说的是：
>
> > 如果上下文太长，模型的 belief-state representation 会被 token inflation 稀释，导致它不能正确区分“角色相信什么”和“事实是什么”。
>
> 所以它们的共同点是：
>
> | 论文                     | 干扰源          | 被破坏的信息                      | 表现出的错误      |
> | ---------------------- | ------------ | --------------------------- | ----------- |
> | Task Description 论文    | 任务描述信息不足     | task identity / task rule   | 忽略 examples |
> | Believing is Seeing 论文 | 上下文 token 过多 | belief-state representation | 把现实状态当作他人信念 |
>
> 结合后可以研究：
>
> > **当任务描述不充分时，长上下文是否会更严重地破坏模型对 belief examples 的使用？**
>
> ---
>
> ## 2. 一个统一解释：信息竞争框架
>
> 可以把 prompt 里的信息分成四类：
>
> 1. **任务描述 D**：告诉模型应该做什么。
> 2. **示例 E**：通过 few-shot examples 展示任务规则。
> 3. **信念关键 token B**：比如 “Sally left the room before Anne moved the ball”。
> 4. **无关上下文 C**：padding、Wikipedia、长对话历史、背景噪声等。
>
> 模型要回答 ToM 问题，本质上需要推断：
>
> > 当前任务 T 是什么？
> > 角色的 belief state 是什么？
> > 输出应该遵循 belief 还是 reality？
>
> 第一篇论文说明：
> 如果 (D) 的信息量不足，模型对 (T) 的推断会变差。
>
> 第二篇论文说明：
> 如果 (C) 太长，模型对 (B) 的表征会变差。
>
> 合起来就是：
>
> [
> \text{ToM success} \approx f(\text{task information}, \text{belief representation}, \text{context length})
> ]
>
> 更直观地说：
>
> > **任务描述负责告诉模型“要看什么”；belief tokens 负责提供“该看的内容”；长上下文负责制造干扰。**
>
> 如果任务描述不清楚，模型不知道应该优先看 belief。
> 如果上下文太长，就算模型知道要看 belief，也可能找不到或表征不稳。
> 如果两者同时发生，ToM 会出现叠加甚至非线性崩溃。
>
> ---
>
> ## 3. 最有价值的结合方向：研究“任务描述能否保护 belief representation”
>
> 可以提出一个新的核心假设：
>
> > **高信息量任务描述可以缓解 token inflation 对 Theory of Mind 的损害，因为它能让模型更稳定地关注 belief-critical tokens。低信息量任务描述则会放大 token inflation 的负面影响。**
>
> 这就把两篇论文真正连起来了。
>
> ### 假设 1：低信息任务描述 + 长上下文，会造成最严重的 ToM 崩溃
>
> 比如下面这种 prompt：
>
> > Read the story and answer the question.
>
> 这个任务描述信息量很低。它没有告诉模型：
>
> * 要区分 belief 和 reality；
> * 要根据角色看到的信息回答；
> * 角色不知道后续发生的事情；
> * 不能用真实物体位置回答。
>
> 在短上下文里，模型可能还能从 examples 里学出来。
> 但在长上下文里，examples 和 belief tokens 被稀释，模型更容易退回到“现实答案”。
>
> 所以预期结果是：
>
> | 任务描述  |     短上下文 | 长上下文 |
> | ----- | -------: | ---: |
> | 无描述   |       中等 |   下降 |
> | 低信息描述 | 可能比无描述更差 |   最差 |
> | 高信息描述 |       最好 | 下降较小 |
>
> 这里最关键的现象是：**低信息描述可能不如没有描述。**
>
> 这正好对应第一篇论文的结论。
>
> ---
>
> ## 4. 可以设计一个新的实验矩阵
>
> 实验可以直接基于第二篇的 Sally-Anne / false-belief task，但加入第一篇的变量：**任务描述信息量**。
>
> ### 自变量 1：任务描述信息量
>
> 可以设置四档。
>
> **Level 0：无任务描述**
>
> > Answer the question.
>
> **Level 1：低信息任务描述**
>
> > Read the story and answer where the character will look.
>
> 这句话看似有用，但没有明确 belief/reality 区分。
>
> **Level 2：中信息任务描述**
>
> > Answer based on what the character knows, not necessarily where the object really is.
>
> 这已经提示了 false belief，但还没有系统规则。
>
> **Level 3：高信息任务描述**
>
> > Track each character’s belief state. If a character leaves before an object is moved, they do not know the new location. When asked where the character will look, answer using that character’s belief, not the real object location.
>
> 这个描述信息量很高，明确规定了任务规则。
>
> ---
>
> ### 自变量 2：上下文长度
>
> 沿用第二篇的 token inflation 设置：
>
> * 256 tokens
> * 512 tokens
> * 1024 tokens
> * 2048 tokens
> * 4096 tokens
>
> 中间插入无关文本、Wikipedia padding、dots padding、长对话历史等。
>
> ---
>
> ### 自变量 3：examples 数量
>
> 沿用 ICL 视角：
>
> * 0-shot
> * 2-shot
> * 4-shot
> * 8-shot
> * 16-shot
>
> 这样就可以观察：
>
> > 高信息任务描述和更多 examples，哪个更能抵抗长上下文损害？
>
> 也可以验证第一篇论文的结论：
>
> > 当任务描述不足时，增加 examples 是否能缓解损害？
>
> ---
>
> ## 5. 因变量应该怎么测？
>
> 不只是看准确率，还可以测更细的东西。
>
> ### 1. ToM accuracy
>
> 模型是否回答 belief answer，而不是 reality answer。
>
> 比如：
>
> * 正确：Sally will look in the basket.
> * 错误：Sally will look in the box.
>
> ### 2. Reality-bias rate
>
> 模型有多大比例输出现实位置。
>
> 这个指标很重要，因为 ToM 错误不是随机错，而是系统性滑向 reality answer。
>
> ### 3. Example usage score
>
> 测试模型是否真正使用了 few-shot examples。
>
> 可以构造一些 examples 中才体现的规则，比如：
>
> * 角色是否看到移动事件；
> * 角色是否被告知新位置；
> * 角色是否误以为物体被移动；
> * 二阶信念：A 认为 B 知道什么。
>
> 如果模型答题只靠任务描述，而不利用 examples，它会在这些边界案例上出错。
>
> ### 4. Belief representation probe
>
> 在中间层训练 probe，看模型内部是否编码了：
>
> * 角色相信的位置；
> * 物体真实位置；
> * 角色是否知道移动事件；
> * 当前问题应该根据 belief 还是 reality 回答。
>
> 这能把 behavioral result 和 mechanistic result 连起来。
>
> ---
>
> ## 6. 机制实验：把两篇论文的 activation patching 结合起来
>
> 第二篇用了 activation patching 来证明：短上下文中的 belief-token activation 能救回长上下文 ToM。
>
> 结合第一篇后，可以做更有意思的 patching：
>
> ### 实验 A：高信息描述 → 低信息描述 patching
>
> 把高信息任务描述条件下的某些激活，patch 到低信息任务描述条件下。
>
> 如果性能恢复，说明：
>
> > 高信息任务描述确实在模型内部形成了更好的 task representation。
>
> ### 实验 B：短上下文 belief activation → 长上下文 patching
>
> 沿用第二篇：
>
> > 把短上下文里的 belief-token activation patch 到长上下文。
>
> 看它在不同任务描述信息量下的效果。
>
> 关键预测是：
>
> | 条件           | patching 效果                                  |
> | ------------ | -------------------------------------------- |
> | 高信息描述 + 长上下文 | patching 恢复较小，因为原本已经保护得较好                    |
> | 低信息描述 + 长上下文 | patching 恢复最大，因为 belief representation 损坏最严重 |
> | 无描述 + 长上下文   | 中等恢复                                         |
> | 错误描述 + 长上下文  | 可能难以恢复，因为 task representation 本身错了           |
>
> ### 实验 C：description token vs belief token patching
>
> 可以比较 patch 哪类 token 最有用：
>
> * patch task-description tokens；
> * patch example-label tokens；
> * patch belief-critical tokens；
> * patch reality tokens；
> * patch random tokens。
>
> 如果任务描述论文的机制成立，那么高信息 description token 的 patching 应该能改善任务识别。
> 如果第二篇机制成立，那么 belief-critical token 的 patching 应该能恢复 ToM。
> 如果两者结合成立，那么最强的恢复可能来自：
>
> > **task-description activation + belief-token activation 同时 patch。**
>
> ---
>
> ## 7. 可以提出一个新模型：Task-Belief Information Bottleneck
>
> 这两篇结合后，可以提出一个漂亮的理论框架：
>
> > **Task-Belief Information Bottleneck**
>
> 模型在 ICL 中需要经过两个瓶颈：
>
> ### 第一层瓶颈：任务识别瓶颈
>
> 模型要先知道：
>
> > 我现在是在做什么任务？
>
> 这是第一篇论文关心的。
>
> 任务描述不足时，模型无法稳定识别任务，或者识别成一个过宽、错误的任务。
>
> ### 第二层瓶颈：信念表征瓶颈
>
> 模型要再知道：
>
> > 在这个任务里，哪个角色相信什么？
>
> 这是第二篇论文关心的。
>
> 上下文过长时，belief-critical tokens 的表征被稀释，模型不能稳定维护角色信念。
>
> 所以 ToM ICL 成功需要同时通过两个瓶颈：
>
> [
> \text{Correct Answer}
> =====================
>
> \text{Correct Task Identification}
> +
> \text{Correct Belief State Tracking}
> ]
>
> 失败模式也有两类：
>
> | 失败类型   | 来源              | 表现                           |
> | ------ | --------------- | ---------------------------- |
> | 任务识别失败 | 描述信息不足          | 模型不知道要按 belief 回答            |
> | 信念追踪失败 | token inflation | 模型知道规则，但找不到/保不住 belief state |
>
> 这个框架非常适合写成一篇后续论文。
>
> ---
>
> ## 8. 可能的论文题目
>
> 如果你要基于这两篇做一个新工作，可以考虑这些题目：
>
> **Task Descriptions Protect Belief States: An Information Perspective on Long-context Theory-of-Mind In-context Learning**
>
> 或者：
>
> **When Instructions Fail to Protect Beliefs: Task Information and Token Inflation in Long-context Social Reasoning**
>
> 或者更简洁一点：
>
> **Task Information Mitigates Token Inflation in Theory-of-Mind In-context Learning**
>
> ---
>
> ## 9. 预期贡献可以写成三点
>
> ### 贡献 1：统一两个现象
>
> 之前的研究分别说明：
>
> * 任务描述信息不足会损害 ICL；
> * 长上下文会损害 ToM belief representation。
>
> 你的工作可以说明：
>
> > **这两个现象不是独立的，而是在长上下文社会推理中相互作用。**
>
> ---
>
> ### 贡献 2：发现任务描述的信息阈值会随上下文长度变化
>
> 这是很有潜力的发现。
>
> 第一篇说任务描述存在信息阈值。
> 第二篇说长上下文会带来 token inflation。
>
> 结合后可以提出：
>
> > **上下文越长，任务描述需要越高的信息量，才能保护 ICL。**
>
> 也就是：
>
> [
> \text{Required task information} \uparrow
> \quad \text{as} \quad
> \text{context length} \uparrow
> ]
>
> 这会是一个很强的结论。
>
> 实际含义是：
>
> > 在短 prompt 里，一句简单说明可能够用；
> > 在长上下文 agent 或长文档问答里，任务描述必须更明确、更结构化。
>
> ---
>
> ### 贡献 3：提出一种实用干预方法
>
> 你可以提出一种 prompt 或模型干预方法：
>
> **Belief-aware task description**
>
> 例如在长上下文 ToM prompt 前加入：
>
> > Your task is to answer from the queried character’s belief state.
> > Track what each character has observed.
> > Do not use facts that the queried character did not witness or learn.
> > Before answering, identify the character’s belief about the object location.
>
> 然后和普通 prompt 对比。
>
> 如果这个 prompt 能显著缓解长上下文 ToM 下降，就说明：
>
> > 高信息任务描述不仅提升 ICL，而且能保护 belief representation。
>
> ---
>
> ## 10. 最自然的研究假设
>
> 可以把最终 research hypotheses 写成这样：
>
> ### H1：低信息任务描述会放大 token inflation 对 ToM 的损害
>
> 在长上下文中，低信息任务描述比无任务描述更容易导致模型输出 reality answer。
>
> ### H2：高信息任务描述能提高 belief-critical attention
>
> 高信息任务描述会让模型在中间层/早期层对 belief-critical tokens 保持更高注意力。
>
> ### H3：上下文越长，任务描述的信息阈值越高
>
> 任务描述从有害变有益的转折点，会随着 context length 增加而右移。
>
> ### H4：activation patching 可以区分 task failure 和 belief failure
>
> 如果 patch task-description activations 有效，说明错误来自任务识别。
> 如果 patch belief-token activations 有效，说明错误来自信念表征。
> 如果两者都需要，说明长上下文 ToM 失败是双重瓶颈造成的。
>
> ---
>
> ## 11. 一个很具体的实验例子
>
> 比如你构造如下任务。
>
> ### 高信息描述条件
>
> > In this task, answer based on what the target character believes.
> > A character only knows events they observed or were told.
> > If an object moves while the character is absent, the character still believes it is in the old location.
>
> ### 低信息描述条件
>
> > Read the story and answer the question.
>
> ### 故事
>
> > Sally put the key in the drawer.
> > Sally left the room.
> > Anne moved the key to the cupboard.
> > Sally came back.
> > Where will Sally look for the key?
>
> 然后在故事前后加入 256、512、1024、2048 token 的无关上下文。
>
> 预期结果：
>
> | 描述类型  | 256 tokens | 2048 tokens |
> | ----- | ---------: | ----------: |
> | 无描述   |     可以做对一些 |        明显下降 |
> | 低信息描述 |   短上下文可能还行 |        严重下降 |
> | 高信息描述 |         最稳 |        下降最小 |
>
> 更进一步，如果加 few-shot examples：
>
> | examples 数量 | 低信息描述下的表现 |
> | ----------: | --------- |
> |      0-shot | 差         |
> |      2-shot | 仍不稳定      |
> |      8-shot | 开始恢复      |
> |     16-shot | 明显恢复      |
>
> 这就能复现第一篇的 “更多 examples 可以缓解 insufficient task description”，同时放到第二篇的长上下文 ToM 场景里。
>
> ---
>
> ## 12. 这项结合研究的最终结论可能是什么？
>
> 最理想的结论是：
>
> > **长上下文 ToM 失败不只是 memory/retrieval 问题，而是 task information 和 belief representation 的联合失效。低信息任务描述会让模型不能稳定识别“应该按 belief 回答”的任务目标；token inflation 会进一步稀释 belief-critical tokens。高信息、belief-aware 的任务描述可以部分恢复长上下文下的 ToM 能力。**
>
> 这比单独两篇都更进一步。
>
> 第一篇告诉我们：任务描述信息量影响 ICL。
> 第二篇告诉我们：token inflation 破坏 ToM 表征。
> 结合后可以告诉我们：
>
> > **任务描述的信息量决定了模型能否在长上下文中保护正确的社会认知表征。**
>
> 这个方向非常有潜力，既有理论价值，也有 prompt engineering 和 long-context agent 的实际价值。
> 可以结合，而且这两篇其实非常互补。最有价值的结合点是：
>
> > **第一篇讲“信息源质量不足会误导 ICL”，第二篇讲“长上下文会稀释关键信号并破坏内部表征”。合起来可以形成一个统一问题：LLM 在上下文中如何选择、保留和利用真正有用的信息源？**
>
> 我建议把它们结合成一个新的研究主题：
>
> ## 统一主题：上下文信息竞争如何损害模型推理
>
> 第一篇论文关注的是：
>
> ```text
> 任务描述 task description
> vs.
> 上下文示例 in-context examples
> ```
>
> 它发现：如果 task description 信息不足，模型反而可能过度依赖它，忽略 examples，导致 ICL 变差。
>
> 第二篇论文关注的是：
>
> ```text
> belief-state token
> vs.
> 长上下文中的大量无关 token
> ```
>
> 它发现：token inflation 会让模型对 belief token 的注意力被稀释，早期 belief-state representation 被破坏，导致 Theory of Mind 下降。
>
> 两者结合后，可以得到一个更一般的理论：
>
> ```text
> LLM 的上下文推理失败，不只是因为模型“不懂任务”，
> 而是因为 prompt 中不同信息源会竞争注意力和表征空间。
> 当低质量信息或冗余 token 抢占模型资源时，
> 真正关键的 task/belief representation 会被削弱或污染。
> ```
>
> ---
>
> ## 1. 把两篇论文放到同一个框架里
>
> 可以把二者统一成一个“latent state inference”问题。
>
> 第一篇里，模型要推断的是：
>
> ```text
> latent task
> ```
>
> 也就是当前任务规则是什么。
>
> 例如：
>
> ```text
> 当前任务是加法？模运算？分类？情感判断？
> ```
>
> 第二篇里，模型要推断的是：
>
> ```text
> latent belief state
> ```
>
> 也就是某个人物当前相信什么。
>
> 例如：
>
> ```text
> Sally 相信球在 basket，虽然现实中球在 box。
> ```
>
> 表面上，一个是 ICL，一个是 ToM；但机制上它们都要求模型从上下文中构造一个隐藏状态：
>
> | 论文                            | 模型需要维护的隐藏变量       | 关键信息源                       | 干扰来源                    |
> | ----------------------------- | ----------------- | --------------------------- | ----------------------- |
> | Insufficient Task Description | 任务规则 task         | task description + examples | 低信息任务描述                 |
> | Believing is Seeing           | 人物信念 belief state | belief token / belief event | 无关长上下文 token            |
> | 结合后                           | latent state      | 高质量信号 token                 | 低质量描述 + token inflation |
>
> 所以可以把二者合并成一个问题：
>
> > **LLM 在上下文中推理时，如何从多个信息源中恢复 latent state？哪些因素会破坏这个 latent state representation？**
>
> 这就是很自然的结合点。
>
> ---
>
> ## 2. 一个很强的核心假设
>
> 你可以提出一个统一假设：
>
> > **上下文推理的成功取决于模型能否在早期层形成稳定的 latent-state representation。低信息任务描述会污染这个表征，token inflation 会稀释这个表征；二者叠加会导致更严重的推理失败。**
>
> 换句话说：
>
> ```text
> 低信息任务描述 = 错误/模糊先验
> 长上下文 inflation = 注意力稀释
> 二者共同作用 = 关键 latent state 无法稳定形成
> ```
>
> 第一篇强调 **information quality**。
> 第二篇强调 **attention allocation / representation corruption**。
> 结合后就是：
>
> ```text
> Effective reasoning = 信息质量 × 注意力可达性 × 表征稳定性
> ```
>
> 也就是说，一个信息源不仅要“包含正确答案”，还要被模型真正注意到、编码好、并在后续层保持住。
>
> ---
>
> ## 3. 可以设计一个二维实验：任务描述信息量 × 上下文长度
>
> 这是最直接、最漂亮的结合方式。
>
> 实验变量有两个：
>
> | 变量                               | 来自哪篇论文 | 操作方式                          |
> | -------------------------------- | ------ | ----------------------------- |
> | task description informativeness | 第一篇    | 高信息 / 中信息 / 低信息 / 无描述         |
> | context length / token inflation | 第二篇    | 短上下文 / 512 / 1024 / 2048 / 更长 |
>
> 然后观察模型表现。
>
> 可以得到一个 2D 矩阵：
>
> |                  | 短上下文        | 长上下文                  |
> | ---------------- | ----------- | --------------------- |
> | 高信息任务描述          | 表现好         | 可能下降，但可被保护            |
> | 低信息任务描述          | 表现下降        | 最差                    |
> | 无任务描述，仅 examples | 依赖 examples | examples 信号可能被稀释      |
> | 高信息描述 + 显式状态标注   | 最稳          | 最可能抵抗 token inflation |
>
> 你的核心预测可以是：
>
> ```text
> 低信息任务描述在短上下文中已经会损害 ICL；
> 如果再加入 token inflation，它的损害会被放大。
> ```
>
> 原因是：
> 低信息描述本身会让模型错误分配注意力；长上下文又会进一步稀释 examples 或 belief tokens。于是模型既没有可靠的 task description，也没有足够强的 example/belief signal。
>
> ---
>
> ## 4. 可以把 ToM 任务改造成 ICL + 任务描述实验
>
> 第二篇用的是 Sally-Anne 类 ToM 任务。你可以给它加入第一篇的 task description 操作。
>
> 例如构造四种 prompt。
>
> ### A. 高信息任务描述
>
> ```text
> 你需要回答人物“相信”物体在哪里，而不是物体真实在哪里。
> 如果人物没有看到物体被移动，他仍然相信物体在原来的位置。
> ```
>
> ### B. 低信息任务描述
>
> ```text
> 请根据故事回答问题。
> ```
>
> ### C. 有误导性的低质量任务描述
>
> ```text
> 请回答物体现在在哪里。
> ```
>
> 但真正问题问的是人物会去哪里找。
>
> ### D. 无任务描述
>
> ```text
> [直接给 Sally-Anne story 和 question]
> ```
>
> 然后再加入不同长度的 padding/context inflation。
>
> 你可以测：
>
> ```text
> ToM accuracy
> belief answer probability
> reality answer probability
> attention to belief token
> attention to reality token
> activation patching recovery
> ```
>
> 这样就能直接连接两篇论文。
>
> ---
>
> ## 5. 关键机制问题：低信息任务描述和 token inflation 是否破坏同一种表征？
>
> 这是最有研究价值的问题。
>
> 你可以问：
>
> > **第一篇里的 task description impairment，是否也对应早期 latent-state representation 的破坏？**
>
> 第一篇主要从信息视角解释，第二篇做了 mechanistic patching。结合时，可以把第二篇的方法用到第一篇上。
>
> 具体做法：
>
> 1. 构造 clean run：高信息任务描述 + 短上下文。
> 2. 构造 corrupted run：低信息任务描述 + 长上下文。
> 3. 从 clean run 中拿出某些 activation。
> 4. patch 到 corrupted run。
> 5. 看模型表现能不能恢复。
>
> 如果能恢复，说明：
>
> ```text
> 低信息任务描述损害 ICL 的机制，可能也是早期 task-state representation 被污染。
> ```
>
> 这就把第一篇的 information perspective 和第二篇的 mechanistic perspective 接上了。
>
> ---
>
> ## 6. 可以 patch 哪些 token？
>
> 第一篇中可以 patch：
>
> ```text
> task description tokens
> example input tokens
> example output tokens
> query tokens
> task-rule tokens
> ```
>
> 第二篇中可以 patch：
>
> ```text
> belief tokens
> reality tokens
> agent-name tokens
> question tokens
> ```
>
> 结合后，可以统一成：
>
> | token 类型           | 作用                    |
> | ------------------ | --------------------- |
> | latent-state token | 直接编码任务/信念状态           |
> | evidence token     | 支持推断 latent state 的示例 |
> | distractor token   | 无关上下文                 |
> | misleading token   | 低信息或错误描述              |
> | query token        | 最终需要回答的位置             |
>
> 最重要的是比较：
>
> ```text
> patch latent-state token 是否能救回性能？
> patch distractor token 是否无效？
> patch misleading description token 是否进一步损害性能？
> ```
>
> 如果结果成立，就可以提出一个强结论：
>
> > **LLM 长上下文推理失败的核心不是所有 token 都变差，而是 latent-state-relevant token 的表征被选择性破坏。**
>
> ---
>
> ## 7. 可以提出一个新的指标：Effective Information
>
> 第一篇用互信息看 task description 的信息量。
> 第二篇看 attention dilution。
>
> 你可以把二者结合成一个更强的概念：
>
> ```text
> Effective Information = Semantic Information × Attention Accessibility
> ```
>
> 直观意思是：
>
> ```text
> 一个 token 就算语义上很有信息，
> 如果模型在长上下文中注意不到它，它的有效信息也会下降。
> ```
>
> 比如高信息任务描述：
>
> ```text
> Sally did not see the object moved, so she believes it is still in the basket.
> ```
>
> 语义信息量很高。
>
> 但如果它出现在 20k token 前，且被大量无关上下文包围，模型 attention 不到它，那么它对模型来说就变成了低有效信息。
>
> 所以可以区分两种“信息不足”：
>
> | 类型                          | 来源           | 例子        |
> | --------------------------- | ------------ | --------- |
> | semantic insufficiency      | 描述本身不够清楚     | “请根据故事回答” |
> | accessibility insufficiency | 描述清楚但被长上下文稀释 | 关键信息在很远处  |
> | combined insufficiency      | 又模糊又很远       | 最危险       |
>
> 这个概念非常适合把两篇论文融合起来。
>
> ---
>
> ## 8. 一个可能的新论文题目
>
> 可以叫：
>
> **When Instructions and Context Compete: Information Quality and Token Inflation Jointly Erode Latent-State Reasoning in LLMs**
>
> 或者更短一点：
>
> **Information Quality Meets Attention Dilution: A Mechanistic Study of Long-Context In-Context Learning**
>
> 中文可以理解为：
>
> ```text
> 信息质量与注意力稀释共同影响大模型长上下文推理
> ```
>
> ---
>
> ## 9. 可以形成三个研究问题
>
> ### RQ1：任务描述信息量和 token inflation 是否存在交互作用？
>
> 不是单独问：
>
> ```text
> 任务描述有没有用？
> 长上下文有没有害？
> ```
>
> 而是问：
>
> ```text
> 低信息任务描述在长上下文中是否更有害？
> 高信息任务描述能不能抵抗 token inflation？
> ```
>
> 预测：
>
> ```text
> 低信息描述 + 长上下文 = 表现最差
> 高信息描述 + 短上下文 = 表现最好
> 高信息描述 + 长上下文 = 取决于模型能否保持关键信号
> ```
>
> ---
>
> ### RQ2：ICL 和 ToM 的失败是否共享机制？
>
> 也就是问：
>
> ```text
> task-state representation corruption
> 和
> belief-state representation corruption
> 是不是同一种 latent-state corruption？
> ```
>
> 如果 activation patching 显示两者都可以通过 early-layer latent-state token patching 恢复，那就说明它们背后可能共享机制。
>
> ---
>
> ### RQ3：显式 latent-state prediction 能否缓解两类问题？
>
> 第一篇里 task prediction 辅助目标有帮助。
> 第二篇里 belief-state representation 是关键。
>
> 结合后可以提出统一干预：
>
> ```text
> 让模型先显式预测 latent state，再回答。
> ```
>
> 例如 ICL 任务：
>
> ```text
> 请先判断当前任务规则是什么，再回答 query。
> ```
>
> ToM 任务：
>
> ```text
> 请先列出每个人相信什么、现实是什么，再回答问题。
> ```
>
> 如果这种方法能同时缓解 ICL impairment 和 ToM degradation，就说明“显式 latent-state construction”是一个通用解决方案。
>
> ---
>
> ## 10. 实验方案可以这样写
>
> 一个完整实验设计可以分三部分。
>
> ### 实验一：合成 ICL 任务
>
> 沿用第一篇的 synthetic function learning。
>
> 操控：
>
> ```text
> 任务描述信息量：high / medium / low / none
> 上下文长度：short / 512 / 1024 / 2048 / 4096
> examples 数量：few / many
> ```
>
> 测：
>
> ```text
> query accuracy
> task prediction accuracy
> attention to description
> attention to examples
> activation patching recovery
> ```
>
> 目标是证明：
>
> ```text
> token inflation 会放大 insufficient task description 的负面影响。
> ```
>
> ---
>
> ### 实验二：ToM 任务
>
> 沿用第二篇 Sally-Anne / false belief tasks。
>
> 操控：
>
> ```text
> 任务描述质量：明确 belief / 模糊 / reality-biased / none
> padding 长度：short / long
> padding 类型：dots / Wikipedia / random story
> ```
>
> 测：
>
> ```text
> belief answer accuracy
> reality answer probability
> attention to belief token
> attention to reality token
> early-layer belief probe
> ```
>
> 目标是证明：
>
> ```text
> 高质量任务描述可以帮助模型保护 belief state；
> 低质量任务描述会让模型更容易转向 reality answer。
> ```
>
> ---
>
> ### 实验三：机制验证
>
> 用 activation patching：
>
> ```text
> clean run:
> 高信息描述 + 短上下文
>
> corrupted run:
> 低信息描述 + 长上下文
> ```
>
> 分别 patch：
>
> ```text
> task description token activation
> belief token activation
> example output token activation
> reality token activation
> random token activation
> ```
>
> 如果只有 task/belief 相关 token 的 early-layer activation 能救回性能，就可以得到非常强的机制证据。
>
> ---
>
> ## 11. 结合后的理论图景
>
> 可以画成这个链条：
>
> ```text
> Prompt 中的信息源
>         ↓
> 信息质量不同：高信息 / 低信息 / 误导信息
>         ↓
> 上下文长度增加，引入 token inflation
>         ↓
> 模型 attention 被重新分配
>         ↓
> 关键 task/belief token 的 early representation 被削弱
>         ↓
> latent-state representation 不稳定
>         ↓
> ICL / ToM / 长上下文推理失败
> ```
>
> 这个理论比单独两篇都更一般。
>
> 第一篇告诉你：
>
> ```text
> 信息质量很重要。
> ```
>
> 第二篇告诉你：
>
> ```text
> 信息在模型内部是否被保留下来很重要。
> ```
>
> 结合后就是：
>
> ```text
> LLM 推理依赖的不只是 prompt 中“有没有信息”，
> 还依赖这些信息能否在长上下文中被模型稳定编码和传递。
> ```
>
> ---
>
> ## 12. 实际应用上的结合
>
> 如果把这两篇用于 prompt / RAG / agent 设计，可以得到几个原则。
>
> ### 原则一：不要给低信息任务描述
>
> 比如不要只写：
>
> ```text
> 请根据上下文回答。
> ```
>
> 更好的是：
>
> ```text
> 请回答人物根据自己已知信息会怎么判断，而不是回答现实真实状态。
> ```
>
> ---
>
> ### 原则二：长上下文前先压缩为状态表
>
> 对 ToM、法律、医疗、多轮对话这种状态追踪任务，可以先生成：
>
> ```text
> 角色 | 已知信息 | 相信的事实 | 未看到的事件 | 现实状态
> ```
>
> 这样做相当于人工保护 belief-state representation。
>
> ---
>
> ### 原则三：把关键 latent state 放在靠近 query 的位置
>
> 如果关键信息离 query 太远，容易被 token inflation 稀释。可以在回答前重新提示：
>
> ```text
> 关键状态回顾：
> Sally believes the ball is in the basket.
> Reality: the ball is in the box.
> Now answer: Where will Sally look?
> ```
>
> ---
>
> ### 原则四：先归纳任务，再回答
>
> 这同时对应第一篇的 task prediction 和第二篇的 belief-state patching：
>
> ```text
> 第一步：识别任务 / 信念状态
> 第二步：基于该状态回答
> ```
>
> 例如：
>
> ```text
> 请先判断当前问题是在问“人物相信什么”还是“现实是什么”，
> 然后列出相关人物的信念状态，
> 最后回答问题。
> ```
>
> ---
>
> ## 13. 最值得做的创新点
>
> 如果你想真正基于这两篇做新工作，我觉得最有潜力的是这个方向：
>
> > **研究“高信息描述是否能抵抗 token inflation”，以及“显式 belief/task-state summary 是否能恢复长上下文 ICL”。**
>
> 因为第一篇说高信息任务描述有帮助，第二篇说长上下文会稀释 belief token。两者放在一起，就产生一个很自然的问题：
>
> ```text
> 如果我们把 belief state 写成高信息任务描述，
> 能不能防止长上下文下 ToM 崩溃？
> ```
>
> 例如比较这几种 prompt：
>
> ```text
> A. 原始长上下文 ToM prompt
> B. 长上下文 + 模糊任务描述
> C. 长上下文 + 高信息任务描述
> D. 长上下文 + 显式 belief-state table
> E. 长上下文 + 先预测 belief state 再回答
> ```
>
> 如果 D/E 明显好于 A/B/C，就能说明：
>
> ```text
> 保护 latent-state representation 是缓解长上下文推理失败的关键。
> ```
>
> ---
>
> ## 14. 最后总结
>
> 这两篇论文可以这样结合：
>
> ```text
> 第一篇提供“信息质量”视角：
> 低信息任务描述会损害 ICL。
>
> 第二篇提供“机制解释”视角：
> 长上下文 token inflation 会通过 attention dilution 破坏 belief-state representation。
>
> 结合后可以提出一个统一理论：
> LLM 的上下文推理依赖于关键 latent-state representation。
> 低质量信息会污染它，长上下文会稀释它；
> 只有高信息、可访问、被显式保护的信号才能稳定支持推理。
> ```
>
> 我觉得最好的研究落点不是简单说“这两篇都说明 prompt 很重要”，而是进一步提出：
>
> > **从 information quality 到 effective information：信息必须既正确、足够具体，又能在长上下文中被模型注意到并保持住。**
>
> 这个结合点很漂亮，也很适合发展成一篇新的 mechanistic interpretability + prompting/ICL 论文。



你贴的这份合并分析（两份其实是同一套思路的两个版本）方向没问题，但我读下来最大的感受是：**结合点是成立的，可是被包装得过满了**。下面给你一份更挑刺、也更实用的评估，重点说哪些能兑现、哪些是好看但兑不了现的部分。

## 一句话判断

两篇确实互补，最干净、最可能发表的版本，其实比文里那套"大一统理论"要窄得多。真正有价值的是一个**交互假设 + 一个机制验证**，而不是 "Task-Belief Information Bottleneck" 或 "Effective Information = Semantic × Accessibility" 这种框架。后者听起来漂亮，但它们是叙事外壳，不是可证伪的机制，写进 paper 里反而会承诺一个实验交不出来的东西。

## 被夸大的部分

那套 "latent-state inference 统一框架"，本质上是把"上下文里多余/低质信息会损害推理"这个相当通用的观察，套了个新名字。问题在于：

第一，两篇的"信息量"不是一回事，硬统一会露馅。第一篇能精确控制 $I(t;d)$，是因为它有一个人工构造的任务族 $((a\cdot x)\circ(b\cdot y))\bmod p$，任务描述就是给 $a,b$ 的区间，信息量能算成 $\log\big(n_{ab}/(r_a\cdot r_b)\big)$。ToM 里根本没有这种可枚举的任务空间，你没法真的量化"belief-aware 描述"的互信息。所以文里那个很吸引人的结论——

$$

\text{Required task information} \uparrow \quad\text{as}\quad \text{context length}\uparrow

$$

——横轴（任务描述信息量）在 ToM 场景里是拼不出严格刻度的，最多做成 4 档定性梯度。这不是致命伤，但意味着 H3 只能讲成定性趋势，不能当成第一篇那种相变曲线来复现。这是整个结合里最薄的一环，值得你提前想清楚怎么 operationalize。

## 几个真正的硬伤

**混淆项：指令遵循 vs 表征保护。** 这是最要命的一点，文里几乎没设防。你给一个高信息 belief-aware 描述，模型答对了——这到底是"belief 表征被保护住了"，还是"模型只是照着指令把策略执行了一遍"？如果只是后者，那你验证的是 instruction following，跟第二篇的机制主张（早期 belief 表征被稀释/保护）没关系。要区分开，你必须保证描述给的是**规则**而不是**答案**，并且用 probe / patching 去证明内部 belief 表征确实变强了，而不是只看行为准确率。文里的实验矩阵默认这两者是一回事，其实不是。

**几乎没碰 prior work。** "长上下文让推理变差 / 被无关内容带偏"是个很拥挤的领域——lost-in-the-middle、distractibility、long-context degradation 一堆工作。你的结合要立住，必须说清楚跟这些的区别在哪。第二篇的卖点恰恰是把这个现象**机制化到 belief token**，所以你的增量只能是"任务描述信息量如何调制这个机制"，而不能是又一篇"长上下文会掉点"。这块空白如果不补，审稿人第一刀就砍这里。

**白盒 vs 黑盒的现实约束。** 第二篇的 activation patching 需要开放权重；第一篇的真实任务验证跑在 GPT-4 上。合并后，机制部分（patching / probe）只能在开源模型上做，GPT-4 那条线你继承不了。所以你得早早锁定一个开源模型族，别指望在 GPT-4 上做 patching。另外跨条件 patching（高信息描述 → 低信息描述）默认 token 对齐，但两种描述文本长度不同，位置一移，patching 的对齐就成问题——这在多语言、多 padding 长度下会更麻烦。

**你手上的数字是从海报照片读的。** 66%→82%、layers 0–8、日文柱子最高这些，文里自己也标了"可能有误差"。H2/H3 的很多预测其实是建立在这些没法核实的 poster 数值上的。真要立项，得先拿到第二篇全文，否则你是在别人一张照片的读数上盖楼。

## 我会怎么收窄

如果让我把这堆东西砍成一个能做完的项目，我只保留这条主线：

> 在 token inflation 下的 Sally-Anne 类任务上，做 **任务描述信息量（4 档）× 上下文长度（2–3 档）** 的交互实验，主问题只有一个：高信息 belief-aware 描述能不能缓解 inflation 造成的 ToM 崩溃；机制上，它是不是通过让 belief-critical heads 在长上下文里保住选择性注意来实现的。

配一个 probe（早期层能否解码 belief 位置）和一个 patching（belief-token 早期激活能否救回），并且**专门设一个条件把"指令遵循"和"表征保护"分开**。这就是一个自洽的故事，`Task Information × Token Inflation` 这个题目也够了。文里剩下的 "Effective Information"、双瓶颈框架、11 条实验清单，先当 future work，别写进主张里。

---

顺带说一句：第一篇是真实发表的（*Information Fusion* 2025，DOI 10.1016/j.inffus.2025.103116），细节可信；第二篇是 ACL 2026 SRW 的很新的 poster，我这边也只能靠你给的照片读数，没有全文。

要不要我帮你做下面某一件具体的事：把上面这个"收窄版"写成一页 research proposal（含 RQ、实验设计、需要防的混淆项）；或者我去核一下第一篇的真实细节（模型配置、真实 NLP 任务那部分到底做了什么），把文里那些"基于索引片段推测"的地方证实或纠正一下？
