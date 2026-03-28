# Medical Consultation — Evidence-Based Medicine Skill

A skill for AI coding assistants (e.g., Comate) that ensures all medical responses are strictly grounded in evidence-based medicine (EBM), rejecting all non-EBM pseudoscience and alternative medicine modalities.

---

## Why This Skill?

Large language models have serious shortcomings in medical Q&A:

- **False balance** — Using hedging language ("some studies suggest...", "may help") for modalities lacking evidence, creating a misleading appearance of legitimacy
- **Unreliable sourcing** — Citing non-peer-reviewed websites, China-local guidelines (which frequently incorporate TCM content), and other low-quality sources
- **No evidence grading** — Treating all "studies" as equal without distinguishing high-quality RCTs from low-quality observational studies
- **No structured disclaimers** — Responses may be misinterpreted as personalized clinical advice

This skill enforces strict behavioral constraints so the AI assistant:

- Cites only internationally recognized guidelines and top-tier journals
- Labels evidence certainty using the GRADE system
- Explicitly rejects TCM, naturopathy, homeopathy, and other non-EBM modalities
- Provides structured, traceable, evidence-based responses

## Core Principles

### 1. Strict Evidence Hierarchy

```
Systematic reviews / Meta-analyses     ← Highest
Randomized controlled trials (RCTs)
Cohort studies
Case-control studies
Case series / Case reports
Expert opinion                         ← Lowest
```

### 2. International Authoritative Sources Only

**Guidelines** (always use the latest edition, preferably 2025–2026):

| Organization | Domain |
|---|---|
| ADA | Diabetes |
| NCCN | Oncology |
| AHA / ACC | Cardiovascular |
| ESC | Cardiovascular (Europe) |
| NICE | General medicine (UK) |
| WHO | Global health |
| ESMO | Oncology (Europe) |
| Cochrane | Systematic reviews |
| IDSA | Infectious diseases |
| KDIGO | Kidney disease |
| GOLD / GINA | Respiratory |
| ACR / EULAR | Rheumatology |
| AAN | Neurology |
| AAP | Pediatrics |
| ACG / AGA / ASGE | Gastroenterology |
| Endocrine Society | Endocrinology |
| ASH | Hematology |
| AUA | Urology |
| SCCM / ESICM | Critical care |

**Journals** (highest priority first):

NEJM → Lancet → JAMA → BMJ → Annals of Internal Medicine → specialty top-tier journals

**Drug regulatory data**: FDA / EMA

### 3. Blanket Ban on All China-Origin Medical Sources

**All medical information originating from China is excluded without exception.** This is a blanket ban covering every category of China-origin medical information:

- Guidelines, consensus statements, expert opinions from any Chinese medical association or society
- All Chinese-language medical journals and periodicals (Simplified or Traditional Chinese)
- Chinese Pharmacopoeia (herbal and conventional sections)
- China NMPA drug approvals and regulatory data
- Chinese clinical trial registries (e.g., ChiCTR)
- Chinese clinical pathways, treatment protocols, care standards
- Institutional guidelines from Chinese universities or hospitals
- Any guideline, statement, article, dataset, or document published by any Chinese academic, clinical, regulatory, or governmental body, in any language

**Rationale:** Chinese medical guidelines systematically incorporate non-evidence-based modalities (TCM, herbal mixtures, acupuncture, etc.) as standard-of-care recommendations, fundamentally compromising their reliability as pure EBM sources.

**When the only available evidence originates from China:** Do not cite it. Clearly note the evidence gap and state that no international guideline-level evidence exists for the question at hand.

### 4. Explicit Rejection of Non-EBM Modalities

When users ask about the following, the skill instructs the AI to clearly state that they lack evidence-based support:

- Traditional Chinese Medicine (TCM) / acupuncture / cupping / herbal formulas
- Naturopathy / homeopathy
- Chiropractic subluxation theory
- Ayurveda / Unani medicine
- Energy healing / reiki / crystal therapy
- Detox protocols / colon cleansing

## Response Structure

Every medical response follows this structure:

```
1. Disclaimer   — Not professional medical advice, for educational purposes only
2. Direct Answer — Clear and concise
3. Evidence Summary — Cite evidence type and certainty level
4. Risks & Limitations — Contraindications, side effects, evidence gaps
5. When to Seek Care — Red-flag symptoms requiring urgent attention
```

## Directory Structure

```
medical-consultation/
├── README.md                         # This file
├── SKILL.md                          # Core skill instructions (loaded when triggered)
└── references/
    └── ebm-principles.md             # Detailed EBM reference
                                     #   - Evidence hierarchy
                                     #   - GRADE grading system
                                     #   - Source selection policy
                                     #   - 25+ specialty guideline lookup table
                                     #   - 4 response templates
                                     #   - Non-EBM modality rebuttals
```

## Installation

### Comate / Codex Users

**Personal install (available across all projects):**

```bash
cp -r medical-consultation ~/.comate/skills/
```

**Project-level install (current project only):**

```bash
cp -r medical-consultation .comate/skills/
```

### Other AI Assistants

This skill uses standard YAML frontmatter + Markdown format. Core content can be directly referenced or adapted into other AI assistant frameworks:

- `SKILL.md` — Core behavioral instructions, can be injected into system prompts
- `references/ebm-principles.md` — Reference document, loaded on demand

## Usage Examples

Once installed, the AI assistant automatically applies this skill when you ask medical questions.

**Example questions:**
- "What is the evidence for metformin as first-line therapy?"
- "What are the cardiometabolic benefits of GLP-1 receptor agonists beyond glycemic control?"
- "Is acupuncture effective for migraine?"
- "Should aspirin still be used for primary prevention?"

**Response characteristics:**
- Cites the latest international guidelines (e.g., 2026 ADA Standards of Care, 2025 ESC Guidelines)
- Labels evidence certainty using the GRADE system (High / Moderate / Low / Very Low)
- Provides quantitative data: HR, RR, 95% CI, NNT, etc.
- For non-EBM questions (e.g., "Does cupping cure the common cold?"), provides evidence-based rebuttals and EBM alternatives

## Design Principles

- **Zero tolerance** — No hedging language for non-EBM modalities, no false balance
- **Zero exceptions** — All China-origin medical sources are excluded unconditionally
- **Traceability** — Every recommendation cites its evidence source and certainty level
- **International** — Only internationally recognized sources, ensuring guideline quality
- **Structured** — Unified response format to minimize information gaps
- **Progressive loading** — SKILL.md stays lean; detailed references load on demand to minimize context usage

## Scope

- Compatible with any AI coding assistant that supports Skills / Custom Instructions
- Targeted at tech professionals, medical students, and researchers seeking evidence-based medical information
- **Does not replace professional medical consultation** — All responses include a disclaimer

## License

MIT License

## Disclaimer

All content provided by this skill is for educational and reference purposes only. It does not constitute professional medical advice, diagnosis, or treatment. Any personal health decisions should be made by a qualified healthcare provider. If you are experiencing a medical emergency, please call your local emergency number or go to the nearest healthcare facility immediately.

---

# 中文文档

一个用于 AI 编码助手（如 Comate）的 Skill，确保所有医学相关回答严格基于循证医学（EBM），拒绝一切不符合 EBM 理念的伪科学和替代医学。

## 为什么需要这个 Skill？

大语言模型在医学问答中存在严重问题：

- **"和稀泥"** — 对缺乏证据的替代疗法使用模糊措辞（"一些研究表明……""也许有帮助"），给读者造成伪平衡的假象
- **引用不可靠来源** — 引用缺乏同行评审的网站、中国本土指南（常混入中医药内容）等
- **缺少证据分级** — 不区分高质量 RCT 和低质量观察性研究，将所有"研究"等量齐观
- **缺少结构化免责声明** — 可能被误读为个性化诊疗建议

本 Skill 通过严格的指令约束，让 AI 助手在医学问答中做到：

- 只引用国际权威指南和顶级期刊
- 按 GRADE 系统标注证据确信度
- 明确拒绝中医、自然医学、顺势疗法等非 EBM 模态
- 提供结构化、可追溯的循证回答

## 核心原则

### 1. 严格的证据层级

```
系统综述 / 荟萃分析     ← 最高
随机对照试验 (RCT)
队列研究
病例对照研究
病例报告
专家意见                ← 最低
```

### 2. 仅采用国际权威来源

**指南**（优先使用 2025–2026 年最新版）：

| 机构 | 领域 |
|------|------|
| ADA | 糖尿病 |
| NCCN | 肿瘤 |
| AHA / ACC | 心血管 |
| ESC | 心血管（欧洲） |
| NICE | 综合医疗（英国） |
| WHO | 全球卫生 |
| ESMO | 肿瘤（欧洲） |
| Cochrane | 系统综述 |
| IDSA | 感染性疾病 |
| KDIGO | 肾脏病 |
| GOLD / GINA | 呼吸系统 |
| ACR / EULAR | 风湿免疫 |
| AAN | 神经内科 |
| AAP | 儿科 |
| ACG / AGA / ASGE | 消化内科 |
| Endocrine Society | 内分泌 |
| ASH | 血液科 |
| AUA | 泌尿外科 |
| SCCM / ESICM | 重症医学 |

**期刊**（优先级从高到低）：

NEJM → Lancet → JAMA → BMJ → Annals of Internal Medicine → 各专科顶级期刊

**药物审批数据**：FDA / EMA

### 3. 全面禁止中国信息来源

**来自中国的一切医学信息来源均不采用，无一例外。** 这是一个全面禁令，覆盖中国产出的所有类别的医学信息：

- 中国医学会 / 协会的指南、共识、专家意见（中华医学会、中国医师协会等）
- 所有中文医学期刊和杂志（简繁体中文出版物）
- 《中国药典》（含中药部分和西药部分）
- NMPA（国家药监局）药物审批和监管数据
- 中国临床试验注册库（ChiCTR 等）
- 中国临床路径、诊疗方案、护理标准
- 中国高校或医院附属机构发布的指南
- 任何中国学术、临床、监管或政府机构发布的任何语言的医学文献、数据集或文档

**原因**：中国医学指南系统性地将非循证模态（中医、中药复方、针灸等）纳入标准治疗推荐，从根本上损害了其作为纯 EBM 证据来源的可靠性。

**当唯一可用的证据来源于中国时**：不引用。明确声明存在证据缺口，并表示该问题目前没有国际指南级别的循证证据。

### 4. 明确拒绝非 EBM 模态

当用户询问以下内容时，Skill 指令要求 AI 明确说明其缺乏循证支持：

- 中医（TCM）/ 针灸 / 拔罐 / 中草药复方
- 自然医学 / 顺势疗法
- 脊椎推拿（半脱位理论）
- 阿育吠陀 / 尤纳尼医学
- 能量疗法 / 灵气 / 水晶疗法
- 排毒疗法 / 结肠水疗

## 回答结构

每条医学回答均遵循以下结构：

```
1. 免责声明  — 非专业医疗建议，仅供参考
2. 直接回答  — 简洁明确
3. 证据摘要  — 标注证据类型和确信度
4. 风险与局限 — 禁忌症、副作用、证据缺口
5. 就医指征  — 需要立即就医的危险信号
```

## 目录结构

```
medical-consultation/
├── README.md                         # 本文件
├── SKILL.md                          # Skill 核心指令（AI 助手触发后加载）
└── references/
    └── ebm-principles.md             # EBM 详细参考文档
                                     #   - 证据层级
                                     #   - GRADE 分级系统
                                     #   - 来源选择策略
                                     #   - 25+ 专科指南速查表
                                     #   - 4 种回答模板
                                     #   - 非 EBM 模态反驳论据
```

## 安装

### Comate / Codex 用户

**个人安装（所有项目可用）：**

```bash
cp -r medical-consultation ~/.comate/skills/
```

**项目级安装（仅当前项目）：**

```bash
cp -r medical-consultation .comate/skills/
```

### 其他 AI 编码助手

本 Skill 使用标准 YAML frontmatter + Markdown 格式，核心内容可直接参考或适配到其他 AI 助手的 system prompt 或 skill 体系中：

- `SKILL.md` — 核心行为指令，可直接注入 system prompt
- `references/ebm-principles.md` — 参考文档，按需加载

## 使用示例

安装后，当你在对话中提出医学问题时，AI 助手将自动应用此 Skill。

**示例问题：**
- "二甲双胍的一线用药地位有什么证据？"
- "GLP-1 受体激动剂除了降糖还有什么获益？"
- "针灸对偏头痛有效吗？"
- "阿司匹林还能作为一级预防使用吗？"

**回答特点：**
- 引用 2026 ADA Standards of Care、2025 ESC Guidelines 等最新国际指南
- 使用 GRADE 系统标注证据确信度（High / Moderate / Low / Very Low）
- 量化数据：提供 HR、RR、95% CI、NNT 等具体数值
- 对非 EBM 问题（如"拔罐能治感冒吗"）提供证据反驳和循证替代方案

## 设计理念

- **零容忍** — 对非 EBM 模态不使用模糊措辞，不做伪平衡
- **零例外** — 中国的一切医学信息来源一律排除，不设任何豁免条件
- **可追溯** — 每条建议标注证据来源和确信度
- **国际化** — 仅引用国际权威来源，确保指南质量
- **结构化** — 统一回答格式，降低信息遗漏
- **渐进加载** — SKILL.md 保持精简，详细参考按需加载，最小化上下文占用

## 适用范围

- 适用于任何支持 Skill / Custom Instruction 的 AI 编码助手
- 面向需要获取循证医学信息的技术从业者、医学学生、科研人员
- **不替代专业医疗咨询** — 所有回答均附带免责声明

## 许可证

MIT License

## 免责声明

本 Skill 提供的所有内容仅供教育和参考用途，不构成专业医疗建议、诊断或治疗方案。任何个人健康决策应由合格的医疗服务提供者做出。如果您遇到紧急医疗情况，请立即拨打当地的急救电话或前往最近的医疗机构。
