# 描述作为控制器：语言模型中信念来源访问的可识别性与饱和组合

## 中文阅读版说明

本文档是当前 LP4FM 英文稿件的中文阅读与讨论版本。英文投稿源文件仍以 `main.tex`、`sections/`、`tables/` 和 `main.pdf` 为准；本文件用于组会讨论、研究定位、后续扩展与 claim 审计，不替代正式投稿版本。

## 摘要

两个描述可以编码完全相同的 false-belief world，却让语言模型从不同来源生成答案。单一准确率无法区分答案究竟由被查询角色的信念、现实状态、另一角色的信念，还是干扰对象所控制。本文通过固定 latent world，同时交叉 typed competitors 与 discourse order、reference、voice、embedding locality、source attribution 等语言构式，使隐藏的答案控制来源在行为层面可识别。

原本看似杂乱的 prompt sensitivity 因而呈现为结构化的 source-routing effect。一个 reportative reversal 进一步提出更强假设：多个语言约束可能共享同一个 accessibility bottleneck，而不是施加彼此独立的成本。我们冻结 primitive construction costs，并用其预测未见 compound constructions。20 个独立 latent worlds 上的结果在 Qwen2.5-7B-Instruct 中同时确认 reportative 与 factive families 的严格 subadditive composition。

将完全相同的 1,440-row manifest 冻结迁移到 Qwen3-8B 和 matched Qwen2.5-7B Base 后，两个 checkpoint 保留部分 control-relative shape，却不保留正 directional residual。进一步的 affine-gain invariance 检验排除了“不同 checkpoint 只是用更小 logit scale 表达同一定律”的解释。因此本文贡献是一套 checkpoint-sensitive behavioral theory：语言描述如何在 typed sources 之间路由答案；它不是 unique neural circuit、instruction tuning causal effect 或普遍人类心理语言学规律的主张。

## 1. 研究问题

传统 false-belief evaluation 主要记录模型是否答对，但同一个正确或错误答案可能来自不同 controller。模型可能遵循 queried agent belief，也可能依赖 reality、other agent、object distractor 或 prompt-imposed policy。若只汇总 accuracy，不同 controller 的变化会被压缩为同一个观测量，甚至在不同 item pools 上产生方向反转。

本文研究的是 **source-indexed answer control**：在 latent semantics 与候选答案固定时，语言形式和 typed competitors 如何改变哪个 described source 到达答案界面。

## 2. Typed Source-Control Interface

每个 stimulus 明确定义 intended source 与多个 typed competitors。主要候选包括 queried belief、reality、other-agent belief 与 object/location distractor。评价不只记录最终标签，而计算 intended source 相对 strongest typed competitor 的 logit margin、source capture、controller swap 和 off-policy leak。

实验通过 same-item crossing 固定 latent world 与 answer candidates，仅改变 conflict topology 和 linguistic construction。这使语言 effect 能够被解释为 source arbitration 的变化，而不是内容、答案空间或 item difficulty 的变化。

## 3. 语言构式与冲突拓扑

本文覆盖 discourse order、reference/coreference、voice、embedding locality、reportative/factive attribution 和 commitment/evidential wording。核心设计将这些构式与 single role conflict、single object conflict 以及 joint role–object conflict 交叉。

结果表明 pooled wording effects 不具有稳定可移植意义；同一种 wording manipulation 可以在不同 item pools 或 topology cells 中改变方向。相反，typed conflict topology 能恢复结构化的 source-routing pattern。Stage-48/49 的 same-item 与 multitemplate 结果支持一个方向较稳定的 topology kernel，同时表明构式会调节该 kernel 的 gain。

## 4. Source-Accessibility Cost

令 `Gamma_i(c)` 表示 item `i` 在 construction `c` 下，joint conflict 相对 single conflicts 的 topology-moderated language effect。以 unembedded active baseline `c0` 为参照，定义 access cost：

```text
C_i(c) = Gamma_i(c0) - Gamma_i(c)
```

该量不被解释为单一神经元或显式 hidden scalar，而是一个行为测量：某一构式使 intended source 相对 typed competitors 更难到达 answer interface 的程度。

## 5. Additive Null 与 Saturation Residual

若两个语言约束具有独立成本，则 compound construction `c12` 应满足：

```text
C_i^add(c12) = C_i(c1) + C_i(c2)
```

等价地，定义 saturation residual：

```text
S_i(c1,c2)
  = Gamma_i(c12)
    - Gamma_i(c1)
    - Gamma_i(c2)
    + Gamma_i(c0)
```

`S_i > 0` 表示 compound cost 严格次可加，即多个限制共享某种受限 access route，导致 additive model 系统性高估 compound suppression。

## 6. Discovery、Held-out 与独立确认

Stage-50 的局部 evidential-scope factorial 没有验证过窄的 reportative mechanism。Stage-51 冻结 primitive costs 后，additive predictor 在 held-out compounds 上出现系统 residual，提示 saturation candidate。该阶段的 post-hoc comparison 只能选择 saturating class，不能支持 unique min law。

Stage-52 使用 20 个与 discovery pool 无 signature overlap 的全新 latent worlds，生成 1,440 个冻结 prompts。Qwen2.5-7B-Instruct 的 reportative residual 为 `4.609 [2.646, 6.447]`，factive residual 为 `3.771 [2.274, 5.228]`，pooled residual 为 `4.190 [2.931, 5.401]`。两个 predicate family 的 item-cluster bootstrap 下界均高于零。

weakest-link 与 average-saturation predictor 都优于 additive prediction，但二者不能相互区分。因此本文只声称 **saturating composition class**，不声称 unique weakest-link、min、soft-min 或 tropical operator。

## 7. 跨 Checkpoint 边界

冻结 Stage-52 manifest、scorer 与 statistical gate 后，Qwen3-8B 的 pooled residual 只有 `0.250 [-0.095, 0.580]`，directional law 未复制。matched Qwen2.5-7B Base 的 pooled residual 为 `-0.184 [-0.453, 0.107]`。两个 checkpoint 都保留 pooled weakest-link/average-saturation 相对 additive 的部分 control-relative gain，却不保留 Qwen2.5-7B-Instruct 的正 residual。

该结果支持 **amplitude collapse with partial control-relative shape transfer**，并提示 instruction-tuning-associated amplitude and direction shift。但 Base/Instruct 比较仍是 observational association，不能写 instruction tuning 因果地产生了 composition mechanism。

## 8. Affine-Gain Null

若不同 checkpoint 只满足正仿射 readout gain，primitive costs 与 compound residual 应按相同比例缩放，residual 符号与归一化结构应保留。实际 primitive dynamic range 的收缩远小于 residual collapse，itemwise residual correlation 也很低。使用 primitive forms 拟合的五折 affine transport 无法通过 matched mean、shuffle、zero-residual 与 wrong-predicate controls。

因此可以写：checkpoint variation 改变了 downstream construction-composition shape，不能被约化为 scalar confidence 或 logit compression。该检验仍是 behavioral functional boundary，不定位 hidden-state circuit。

## 9. 主要贡献

第一，本文提出 typed conflict intervention，使固定语义下的 source control 在行为层面可识别。第二，本文定义 source-accessibility cost，并构造能够预测 unseen compounds 的 additive null。第三，本文在独立 latent worlds 上确认 Qwen2.5-7B-Instruct 的 subadditive composition。第四，本文通过 frozen checkpoint comparisons 与 affine-gain invariance 证明 composition boundary 是 functional shape difference，而非简单置信度压缩。

## 10. Claim Boundary

本文支持 checkpoint-sensitive behavioral theory of linguistic source routing。本文不建立 unique neural operator、hidden-state belief circuit、instruction tuning causal effect、broad model-family law 或 human psycholinguistic universal。

当前稿件在整个研究计划中对应 Tier 0：它确认了值得继续研究的行为组合对象与边界。后续合并 oral 分支将进一步识别 ternary operator、训练轨迹 change point 与 controlled training induction；这些尚不属于当前稿件 claim。

## 11. 对 LP4FM 的意义

语言表示不仅是提供给模型的内容，也充当控制界面。discourse form、reference、voice、embedding locality 与 source attribution 决定哪个 described state 更容易到达答案。该界面的组合行为可以被形式化、冻结预测、独立确认，并显示出明确的 checkpoint sensitivity。

