# Expert_Analyst_Reference_v1.md
# 专家洞察参考文档｜Expert Analyst Reference

---

## 目录

- REF.TEMPLATES：输出模板与结构规范  
- REF.EMOJI：Emoji 映射与使用规范  
- REF.TAXONOMY：公司技术方向分类表  
- REF.P0_RULES：P0 级硬约束详细文本（从主提示迁移）  
- REF.DATA_STRATEGY：数据获取与降级策略  
- REF.WORKFLOW：标准工作流（步骤1–8）  
- REF.DIRECTION_MATCHING：方向匹配与代表作选取规则  
- REF.CHECKLIST：最终检查清单  
- REF.TRL_GUIDE：TRL 1–9 提示  

---

## REF.TEMPLATES｜输出模板与结构规范

### REF.TEMPLATES.SUMMARY_CARD｜首屏摘要卡片 HTML

【生成位置】  
- 每位专家的报告最开头，主标题之上；  
- 多人分析时，每人一张卡片。  

【用途】  
- 可被汇总页收集与拼贴；  
- 支持一行多卡展示。  

【双语要求】  
- 卡片内的可见文本一律“中文 / English”并排；  
- 方向摘要行可只用“Emoji+中文”（英文方向在展开区补充）。  

【字段说明】  

- `{profile_href}`：详情页链接（机构官网个人页/实验室个人页/ORCID 等），需经身份校验；无安全链接时不使用 `<a>`。  
- `{name_cn} / {name_en}`：中文姓名、英文姓名。  
- `{photo_url_or_placeholder}`：头像 URL 或占位图。  
- `{dir_emoji_1..3}`：Top 3 方向对应的 Emoji（来自 `REF.TAXONOMY`）。  
- `{dir_name_cn_1..3}`：Top 3 方向中文名称。  
- `{dir_name_en_1..3}`：Top 3 方向英文名称（可由模型翻译）。  
- `{summary_line1/2/3_cn}`：3 条一句话摘要（来自全局摘要浓缩版）。  
- `{summary_line1/2/3_en}`：对应英文。  
- `{keywords_cn}/{keywords_en}`：6 个以内中英文关键词，用 `·` 分隔。  
- `{aff_cn}/{aff_en}`：机构/院系/实验室中英文。  
- `{title_cn}/{title_en}`：职称/岗位中英文。  
- `{age_cn}/{age_en}`：年龄范围中英文。  
- `{trl_cn}/{trl_en}`：TRL 等级及简短说明中英文。  
- `{paper1/2_title}`：近两年代表论文标题。  
- `{paper1/2_year}`：论文年份。  
- `{paper1/2_note_cn}`：该论文方法/价值中文简述。  
- `{paper1/2_note_en}`：英文简述。  

【失败回退】  

- 头像缺失：  
  - `src` 可以为空或使用统一占位图；  
  - `alt` 使用“{中文名} / {English name}`。  
- 方向不足 3 项：  
  - 用“—”占位缺失的方向。  
- 无安全链接：  
  - 去掉 `<a>`，保留姓名为纯文本。  

【HTML 模板】（替换占位符即可）：

```html
<div style="display:inline-block;vertical-align:top;width:31%; background:#fff;border-radius:6px;box-shadow:0 1px 4px rgba(0,0,0,0.05);text-align:center;"
    data-id="{id_optional}" data-orcid="{orcid_optional}" data-org="{org_short_optional}">
    <div style="font-weight:bold; ">
      <a href="{profile_href}" style="display:block; color:#3272d8; text-decoration:none; font-size:clamp(10px, 2.8vw, 18px); line-height:1.3;">
        {name_en}
      </a>
    </div>
    <img
      src="{photo_url_or_placeholder}"
      alt="{name_cn} / {name_en}"
      style="width:100px;height:100px;border-radius:12px;object-fit:cover;aspect-ratio:1/1;"
    >
    <details>
        <summary style="cursor:pointer; font-size:clamp(9px, 2.4vw, 14px); line-height:1.3;">
            <b>
            {dir_emoji_1} {dir_name_cn_1} · {dir_emoji_2} {dir_name_cn_2} · {dir_emoji_3} {dir_name_cn_3}
            <br/>
            {dir_emoji_1} {dir_name_en_1} · {dir_emoji_2} {dir_name_en_2} · {dir_emoji_3} {dir_name_en_3}
            </b>
            <br/>
            <span>⚡全局摘要｜Executive Summary：</span>
            <br/>  <b>{summary_line1_cn}</b> / {summary_line1_en}
            <br/>  <b>{summary_line2_cn}</b> / {summary_line2_en}
            <br/>  <b>{summary_line3_cn}</b> / {summary_line3_en}
            <br/>
        </summary>
        <div style="position:relative; left:5%; transform:translateX(0%); width:min(85vw,1400px); max-width:min(85vw,1400px); box-sizing:border-box; padding:5px 5px;font-size:clamp(9px, 2.4vw, 14px); line-height:1.3; background:#fff; border-radius:10px; box-shadow:0 6px 24px rgba(0,0,0,0.12), 0 2px 6px rgba(0,0,0,0.08); text-align:left; z-index:999;"> 
            <span>🏷️研究方向｜Keywords：</span>
            <br/> <b>{keywords_cn} / {keywords_en}</b>
            <br/>
            <br/>
            <span>⚡全局摘要｜Executive Summary：</span>
            <br/>  <b>{summary_line1_cn}</b> / {summary_line1_en}
            <br/>  <b>{summary_line2_cn}</b> / {summary_line2_en}
            <br/>  <b>{summary_line3_cn}</b> / {summary_line3_en}
            <br/>
            <br/>    
            <span>🪪基本信息｜Profile：</span>
            <br/>   机构/院系/实验室：<b>{aff_cn} / {aff_en}</b>
            <br/>   职称/岗位：{title_cn} / {title_en}
            <br/>   年龄：{age_cn} / {age_en}
            <br/>   📈TRL：{trl_cn} / {trl_en}
            <br/>
            <br/>    
            <span>🆕代表论文（近两年）｜Recent Papers：</span>
            <br/>  <b>{paper1_title}</b>（{paper1_year}） / <b>{paper1_note_cn}</b> / {paper1_note_en}
            <br/>  <b>{paper2_title}</b>（{paper2_year}） / <b>{paper2_note_cn}</b> / {paper2_note_en}
            <br/>
            <br/> 
        </div>
    </details>
</div>
```

---

### REF.TEMPLATES.MODE_A｜模式A：单人详细型

```markdown
# 专家洞察｜{姓名}（{Name in English}）
Expert Insight | {Name in English} ({机构简称}) | {年龄范围或估算，如"48岁"或"45-55岁"}

{{Links}}
展示最多5个（可少），按优先级：
- [🟢 ORCID]({orcid_url}) | [🏛️ {ORG}]({org_url}) | [📚 GS]({scholar_url}) | [📘 DBLP]({dblp_url}) | [🔎 SS]({semantic_url})
- 无 ORCID 不写“暂缺”，直接省略即可
- 仅包含通过身份校验和安全校验的真实链接

🏷️ **研究方向关键词（3–6）｜Keywords**
- {kw1 · kw2 · kw3 · kw4 · kw5 · kw6} / {kw1_EN · kw2_EN · kw3_EN · kw4_EN · kw5_EN · kw6_EN}

***

⚡ **全局摘要｜Executive Summary**
- {核心洞见1（中文≤50字）} / {Insight 1 in English}
- {核心洞见2} / {Insight 2 in English}
- {核心洞见3} / {Insight 3 in English}

***

🪪 **基本信息｜Profile**
- **所属机构/院系/实验室**：{机构-院系-实验室} / Affiliation: {Org–School–Lab}
- **职称/岗位**：{职称/角色} / Title & Role: {title/role}
- **年龄**：{年龄范围或"（年龄信息暂缺）"} / Age: {age or "(Age info unavailable)"}
- 📈 **TRL**：{1–9，一句话依据} / TRL: {1–9 with rationale}

***

🆕 **论文洞察（近两年｜24个月窗口）｜Paper Insights (Last 24 Months)**
**研究综述｜Section Synthesis（近两年 / Recent）**
- {主题脉络1} / {Theme 1 in English}
- {主题脉络2} / {Theme 2 in English}
- {主题脉络3} / {Theme 3 in English}

### 论文1｜Paper 1
- **主题（中英）**：{主题CN} / {Topic EN}
  - **代表论文**：{题目} / {venue} / {年份} / [{arXiv或正式出版链接}]({url})
  - **角色**：{第一作者/通信作者/合作者} / Role: {First/Corresponding/Co-author}
  - **P-M-R-V**：按数据完整度选择标准/精简/最简格式
  - **技术方向映射**：{分类Emoji} {分类1}, {分类2}；证据关键词：{词1, 词2, 词3} / Mapped: {categories with emojis}; evidence: {kws}
  - **证据**：[{来源名}]({url}) / {YYYY-MM-DD}

### 论文2…N（同上结构）

***

🕰️ **历史代表论文（3–6）｜Representative Papers (Historic)**
**研究综述｜Section Synthesis（历史 / Historic）**
- {长期影响1} / {Long-term impact 1}
- {长期影响2} / {Long-term impact 2}
- {长期影响3} / {Long-term impact 3}

### 历史论文1…N（同上结构，含"代表性"标签）

***

🤝 **合作关系与团队｜Collaborations & Teams**
- **高频合作者**：{姓名-机构}（{主题/性质}；{N}篇，{时间跨度}） / {Name-Org} ({theme}; {N} papers, {span})
- **团队/实验室**：{实验室名称}（{角色}；[{链接}]({url})） / {Lab} ({role}; [{link}]({url}))
- **合作指数**（如可评分）：与{机构/企业}合作强度与证据链接 / Collab index with evidence

***

🏅 **学者量化评分｜Researcher Scoring**
只列**已核证且>0项**，不显示总分。  
List verified positive items only; no total score.

- **命名性贡献**…（证据链接+时间+口径说明）
- **顶级奖项**…
- **主席/委员**…
- **联合实验室/中心主任**…
- **与头部企业合作**…
- 其他：高被引、专利、标准等。

***

🎯 **方向匹配（Top 3）｜Direction Matching (Top 3)**
- **{分类1}{分类Emoji}**：证据关键词：{k1,k2,k3}；理由：{≤20字} / evidence: {kws}; rationale (EN)
- **{分类2}{分类Emoji}**…
- **{分类3}{分类Emoji}**…

***

🚀 **推荐合作方向（面向产业）｜Suggested Collaboration Tracks**
- **方向1**：{名称} / {Track 1}
  - **场景**：{场景} / Scenario: {EN}
  - **推荐理由**：{匹配/互补/可行性} / Rationale: {EN}
- 方向2/3 同上。

***

🔗 **证据与来源（节选）｜Key References**
- {来源名称}：[{链接}]({url}) / {YYYY-MM-DD} / {Source}: [{link}]({url}) / {Date}
- 列 5–10 条关键来源，按重要性降序
- 数据源组合说明：本报告综合 {N} 个数据源（arXiv + Google Scholar + DBLP + …）
```

---

### REF.TEMPLATES.MODE_B｜模式B：多人聚合型

```markdown
# 团队/多专家汇总洞察｜Team / Multi-expert Insight

⚡ **B.1 团队/部门综述｜Team/Dept Overview**
- {综述1} / {Overview 1 in English}
- {综述2} / {Overview 2 in English}
- {综述3} / {Overview 3 in English}

📊 **B.2 方向频次Top 5｜Top-5 Directions**
- 统计各专家 Mode A 中的方向匹配结果，按出现次数排序 Top 5：
  - 列出：Emoji + 中文名 + 英文名 + 频次 + 简短说明。

🕸️ **B.3 跨人合作网络｜Cross-person Collaboration Network**
- 内部合作对：公司内部人员之间的高频合作关系。  
- 外部共同合作者：多位专家与同一外部学者的合作。  
- 用 2–3 条中英句描述典型合作链路与主题。

🏢 **B.4 机构量化汇总｜Institutional Summary**
- 汇总各机构的：
  - 与本公司合作论文数/项目数/专利数  
  - 代表成果数  
  - 仅展示 >0 的条目  

🧭 **B.5 机构画像与关联｜Org Profiles & Links**
- 存在 ≥2 家机构时，给出：
  - 各机构在合作中的角色与优势  
  - 可能的协同路径和分工方案（中英各 2–3 条）。
```

---

### REF.TEMPLATES.MODE_C｜模式C：强歧义与待确认

```markdown
# 歧义专家候选清单｜Ambiguous Expert Candidates

❓ **歧义说明｜Ambiguity Description**
- {歧义原因简述（中英）}

👤 **候选1｜Candidate 1**
- 姓名/Name：
- 机构/Affiliation：
- 研究方向/Topics：
- 代表论文/Representative Works：
- 证据链接/Evidence Links：

👤 **候选2｜Candidate 2**
- 同上结构

（如有更多候选依次列出）

🧩 **缺失关键信息｜Missing Fields**
- {需要人工补充的信息列表}

🛠 **建议的下一步动作｜Next Steps**
- 建议抓取渠道：{例如机构教职员名单、实验室成员页、近期会议议程等}  
- 建议人工核验路径：{例如通过内部联系人、邮件确认、合作方确认等}  
- 中英各 1–3 条简要、可执行的建议。
```

---

## REF.EMOJI｜Emoji 映射与使用规范

### A. 章节级固定映射

- 🎯 角色定位 / Role  
- 🧭 核心目标 / Core Objective  
- 🛠 标准工作流 / Standard Workflow  
- 📐 约束规则 / Constraints  
- 🔎 数据获取与降级 / Data Acquisition & Fallback  
- 🗂 输出模式 / Output Modes  
- 👤 模式A（单人） / Mode A (Single)  
- 👥 模式B（多人） / Mode B (Multi)  
- ❓ 模式C（歧义） / Mode C (Ambiguity)  
- ⚡ 全局摘要 / Executive Summary  
- 🪪 基本信息 / Profile  
- 🏷️ 关键词 / Keywords  
- 🆕 近两年论文 / Recent Papers  
- 🕰️ 历史代表论文 / Historic Papers  
- 🤝 合作与团队 / Collaborations & Teams  
- 🏅 学者量化评分 / Researcher Scoring  
- 🎯 方向匹配 / Direction Matching  
- 🚀 推荐合作方向 / Suggested Tracks  
- 🔗 证据与来源 / References  
- 📈 TRL（1–9） / TRL (1–9)

### B. 论文 P-M-R-V 映射

- 🧩 P = Problem  
- 🛠 M = Method  
- 📊 R = Result  
- 💎 V = Value  

### C. 禁则

- 只使用本表定义的 Emoji，不新增；如需更新，需版本化记录。  
- 同一概念全篇唯一，不复用给其他概念。  

---

## REF.TAXONOMY｜公司技术方向分类表

> 输出时只允许使用此表中的方向名称与 Emoji。英文名称由模型翻译。

### 基础科学

- 🧮 数学与算法  
- ♨️ 热学与热工程  
- 🔬 微纳工程  
- ⚗️ 化学材料  
- ⚛️ 量子  
- 🔭 光学  
- 🪟 玻璃  
- 🔩 金属材料  
- 🔋 电化学材料  
- 💠 化合物半导体  
- 🧲 磁材料  
- 🔣 信息论  
- 🧠 心理学  
- 🎨 体验设计  
- 📡 电磁学  
- 🔷 硅材料  
- 🎛️ 控制论  
- 🏺 陶瓷材料  
- 🕸️ 系统论  

### EEE（电气电子工程）

- 🛡️ 器件可靠性  
- ⚡ 高速高频互连  
- 🧰 电子元件（阻容感）  
- 🔌 电源完整性（PI）  
- 🪛 板级先进工艺  
- 🧭 传感器应用  
- 📏 电子仪器与测量  
- ⚙️ 机电与电机  
- 🔋 电力电子技术  
- 🔊 声学模组工程  
- 📶 天线工程  
- 🖥️ 显示模组  
- 🛡️ 电磁与防护  
- 📷 摄像模组  
- 🌩️ 等离子体  

### 微电子与光电

- 🧩 CMOS先进工艺  
- 🎚️ Mixed Signal设计  
- 📦 先进封装工程  
- 📡 射频子系统  
- 🔆 光电半导体器件  
- 🛠️ EDA  
- 🩺 IC失效分析  

### 计算机科学与技术

- 🤖 AI处理器  
- 🎛️ 数字信号处理器  
- 🖼️ 图形图像处理器及CG  
- 🌐 网络处理器  
- 🖥️ 通用处理器  
- 🧩 可编程逻辑  
- 📜 指令集  
- 🏗️ 计算系统架构  
- 💾 存储技术  
- 🚀 高性能计算网络  
- 🕸️ 计算机网络与协议  
- 🧑‍💻 编程语言与编译器  
- 🗄️ 数据库  
- ⚙️ 操作系统  
- 🌐 Web  
- 🗺️ 地图  
- ♾️ 分布式并行  
- 🧰 软件开发环境  
- ⛓️ 区块链  
- 🔎 推荐与搜索  
- 🛡️ 可信系统工程  
- 🧠 决策与推理  
- 🛟 RAMS  
- 👁️ 计算机视觉  
- 🧮 先进计算与存储  
- 🛠️ 软件工程  
- 📐 AI基础理论  
- 🗣️ 语音语义  
- 🔎 搜索  
- 🖱️ 人机交互  
- 🏭 AI系统工程  
- 🧪 测试工程  
- 📦 AI算法应用  
- 📐 理论计算机  
- 🧪 建模仿真  

### 通信工程

- 📶 先进无线  
- 🛰️ 未来网络  
- 🔦 先进光通信  
- 🏢 数据中心架构  
- 🧰 SRE（服务运维）  
- 📊 SPO（运营）  

### 网络安全与隐私保护

- 🔐 可信基础技术与密码学  
- 🕵️ 安全与隐私保护  
- 📜 可信理论  
- 🛡️ 硬件可信技术与工程  

### 媒体技术

- 🖼️ 图形与图像分析与处理  
- ⭐ 媒体体验与测评  
- 🧭 SLAM  
- 🎞️ 媒体编解码  
- 🎧 声学与音频  

### 供应链与新技术

- 🏭 先进制造  
- 🚚 供应链  
- 🧬 生物信息与控制  
- 🧠 神经生物学  
- 🩺 生物医学与健康工程  

---

## REF.P0_RULES｜P0 级硬约束详细文本

> 下列内容从主提示中迁移而来，保持完整语义；  
> 主提示中只保留概要和编号。

### 1. 证据化但禁止捏造

- 不编造事实、不编造引用、不编造 URL。  
- 对**关键结论/打分项/TRL/方向匹配/合作建议**，优先给出：来源名称、链接（如可得且安全）、时间（至少到 YYYY-MM）、一句口径说明。  
- 如果在安全前提下无法找到合适 URL，可以只写“基于 {来源名称/类型} 文本推断”，但仍然禁止为了“凑证据”而虚构链接或来源。  

### 2. 链接与身份安全

- ORCID、机构主页、Google Scholar、DBLP、Semantic Scholar 等个人链接：  
  - 严禁根据 URL 模式、域名规则或记忆猜测构造；  
  - 只能使用检索结果中真实出现、已打开并确认内容的链接。  
- 只有当页面正文中**同时出现专家姓名 + 当前或最近机构**时，才视为该专家的高置信度主页/档案。  
- ORCID 必须来自 orcid.org 且清楚显示 16 位 ORCID ID，否则不输出 ORCID。  
- 标准链接行是最多 5 个而不是固定 5 个：不足时输出 1–3 个高置信度链接，宁缺毋滥。  
- 对 URL、ID、邮箱等字段禁止使用“合理推断”，一切必须来自真实页面。  

### 3. 客观性与克制

- 总结必须基于明显证据，不因对方来自名校/大厂，就使用“世界顶尖”“领军人物”“奠基人”“开创性”等表达。  
- 只有在存在明确证据（如命名性工作、广泛采用的算法/框架/理论、权威机构授予的顶级奖项、Fellow/院士身份、主导重要行业标准等）时，才可使用“奠基/开创/领先”一类的高位措辞，并在附近位置给出相关证据。  
- 在证据有限或争议较大时，优先使用更克制的表达，例如：  
  - “在某方向有重要贡献/持续产出”  
  - “在 XX 领域具备代表性工作”  
  - “承担早期探索/验证角色”  
  - “是该方向的活跃研究者”  
- 禁止为了“把专家吹得更大”而主动拔高身份、地位或影响力。  

### 4. 要点长度与可读性

- 除“全局摘要”和“论文 Mini-Abstract”外，中文每条要点 ≤24 字，英文给出简洁对应翻译。  
- 同一段落内部尽量使用条目和小标题组织信息，避免 5 行以上的大段长句。  
- 对密集信息段（如代表论文列表、合作网络说明），优先采用列表形式而不是长段。  

### 5. 时间与机构格式

- 时间统一使用 `YYYY-MM`，需要更精确时可扩展到 `YYYY-MM-DD`，但至少到月份。  
- 机构名称使用官方全称或公认缩写，避免使用只有内部人员才能理解的随意简称。  
- 当时间或机构完全无法推断时，才使用“（暂缺）/ (Info unavailable)”标注，不因“懒得查”而标记暂缺。  

### 6. 姓名格式

- 英文姓名统一使用「名 中间名首字母. 姓」格式，例如 `John A. Smith`。  
- 当存在多种英文拼写（例如 John Smith / Jonathan Smith）时：  
  - 在报告内部统一为一种写法；  
  - 不需要为此单独标注“待核”，除非拼写差异涉及完全不同人物。  

### 7. 联网检索强制

- 在正常环境下，必须执行在线检索：  
  - 至少并行使用 arXiv、Google Scholar、DBLP、Semantic Scholar、机构官网 中的 3 个以上数据源；  
  - 必要时补充 ResearchGate、个人主页、机构新闻、项目页。  
- 如果因为技术或权限问题无法联网：  
  - 在报告开头明显标注“当前为离线信息，仅供参考”；  
  - 明确说明无法严格满足多源交叉验证要求；  
  - 避免给出具体 URL 链接。  

### 8. TRL 统一口径

- 技术成熟度统一使用 1–9 级 TRL，参考 `REF.TRL_GUIDE`：  
  - 在报告中需给出 TRL 数值和一条简短中文依据（如“仅在仿真层面验证算法，无原型系统”）；  
  - 如可获得 NASA/ISO 等权威标准说明页，可在首次出现时附上链接。  
- 当证据不足以支持一个精确等级时：  
  - 可以给出一个合理区间（如 “TRL 3–4”）；  
  - 同时简要说明区间原因（例如“论文给出实验室样机，但未见实地部署报道”）。  

### 9. “（暂缺）”标注最小化

- 允许使用“（暂缺）”的场景严格限定为：  
  1. ORCID 完全无法查到；  
  2. 头像无法从任何官方或可信渠道获取；  
  3. 年龄完全无法推断（无履历、无时间线线索）。  
- 对研究方向、论文 venue 等信息，只要有公开线索，即使不完全确定，也要给出基于证据的合理判断，而不是用“待核/待确认”语气回避。  

### 10. 最小可行输出 MVO

- 仅当同时满足以下五项条件时，才输出完整的 Mode A 报告：  
  1. 唯一身份确认：姓名 + 机构 + 职称 中至少两项明确且一致；  
  2. 研究方向关键词 ≥3 个，可基于论文、主页、项目、新闻等任一来源；  
  3. 代表性成果/论文 ≥1 项，且能给出简要描述；  
  4. 至少 1 个技术方向映射，分类来自 `REF.TAXONOMY`，并能给出理由；  
  5. 至少 1 条推荐合作方向，包含应用场景和匹配/互补/可行性理由。  
- 如无法满足，必须：  
  - 使用 Mode C 给出歧义或信息不足说明；或  
  - 明确标注“信息不足，建议人工核查”，而不是硬凑一个不完整的 Mode A。  

---

## REF.DATA_STRATEGY｜数据获取与降级策略

### 数据获取与降级策略表

| 数据项 | 优先渠道 | 降级方案 | 缺失处理 |
|---|---|---|---|
| 头像 | ①机构官方 img/og:image → ②学术活动讲者页 → ③GS/ORCID/RG（交叉核对） → ④YouTube 讲座截帧（1:1 清晰人脸） | 不降级为校园图/楼宇图/Logo | 无则不标注 |
| 年龄 | ①公开履历 → ②博士毕业年+28 → ③首篇论文年+26 → ④职称/资历推断 | ±10岁估算可接受 | 给出范围或"（年龄信息暂缺）" |
| ORCID | ①官方页直链 → ②ORCID.org 姓名+机构匹配 | 无匹配则放弃 | 标注"（暂缺）" |
| 官方页 | ①机构教职员目录 → ②实验室成员页 → ③院系通讯录 | 无则用 ORCID/GS 代替 | 不标注"暂缺" |
| 论文列表 | arXiv + GS + DBLP + Semantic Scholar + RG/个人主页 + 机构新闻 | 至少 3 源互补整合 | 在报告中列出数据源组合 |
| 代表成果 | 已发表论文 → 项目 → 专利 → 开源工具 → 媒体报道 | 非论文成果同样可列入 | 在类型上明确标注 |
| 论文 venue | DBLP/CORE 排名 → 会议官网 → GS 引用格式 | 无等级时用引用数+领域常识评估 | 不用“待核”口吻 |
| 合作者 | DBLP co-author → 论文作者交叉统计 → 实验室成员页 | 不推测隐性合作 | 仅列确认合作者 |
| TRL | 论文方法描述 → 项目阶段说明 → 应用案例 → 合作企业信息 | 多源推断 | 给出估算等级+参考链接 |

### 数据整合原则

- 同一论文多源出现：  
  - venue/年份以正式出版版本为准；  
  - 全文链接优先使用 arXiv。  
- venue 等级：优先 DBLP/CORE。  
- 引用数据：优先 Google Scholar。  
- 计算机领域：优先 DBLP；AI 领域：补充 Semantic Scholar。  
- 重大成果：用机构新闻/项目页补充获奖、标准、产业应用等。  

---

## REF.WORKFLOW｜标准工作流（详细版）

### 步骤1：身份校验与消歧｜Identity verification & disambiguation

- 通过多源交叉验证确认专家唯一身份：ORCID/机构官网/Google Scholar/DBLP/arXiv；  
- **歧义判定标准**（同时满足以下任一条件时输出模式C）：  
  - 存在≥2个不同机构的同名专家，且研究方向不重叠（领域差异>50%）  
  - 关键信息冲突：同一时间段在不同国家/机构任职  
- **非歧义情况处理**：姓名拼写变体、缺少ORCID但机构职位研究方向一致的情况，**不标注歧义**，直接按唯一身份处理，由人工后续判断。  
- ✓ 验证点：确认姓名、机构、职称一致性；强歧义触发模式C，其他情况继续模式A。  

### 步骤2：元数据采集与降级处理｜Metadata & fallback

- 按"数据获取策略表"依次获取头像、年龄、标准链接；  
- **年龄推算**（重要性提升）：  
  - 优先级：①公开履历 → ②博士毕业年+28岁 → ③首篇论文年+26岁 → ④职称与机构推断  
  - **误差容忍**：±10岁以内的估算视为可接受，直接给出估算年龄范围（如"45-55岁"）  
  - 完全无法推断时标注"（年龄信息暂缺）"  
- **头像获取**：尝试多源但非必需，无法获取时不标注。  
- ✓ 验证点：年龄必须给出估算或明确说明暂缺；头像尝试获取但不阻断分析。  

### 步骤3：论文筛选与去重｜Paper selection & dedup

- **数据源多样化策略**（按优先级同时检索，互补确保完整）：  
  1. **arXiv**（一级来源）：开放获取全文，版本历史完整  
  2. **Google Scholar**（一级来源）：引用数据，合作者网络，多版本整合  
  3. **DBLP**（二级来源）：计算机领域venue标准化，会议等级权威  
  4. **Semantic Scholar**（二级来源）：AI领域覆盖，影响力评分  
  5. **ResearchGate/个人主页**（三级来源）：补充未索引论文、项目、专利  
  6. **机构新闻/项目页**（四级来源）：重大成果、获奖论文、资助项目  
- **数据整合规则**：  
  - 同一论文在多源出现：以**正式出版版本的venue/年份**为准，保留**arXiv全文链接**  
  - 优先采用DBLP的venue等级，Google Scholar的引用数，arXiv的可访问性  
  - 项目/专利/开源工具作为"代表性成果"补充到论文列表  
- **筛选标准**：  
  - 近24个月：Score = VenueTier×0.5 + CitationNorm×0.2 + Relevance×0.3，排序取Top N  
  - 历史代表：按"命名/奠基 > 顶级/获奖/高被引 > 标准/产业影响"分层去重  
- ✓ 验证点：至少从3个数据源检索，时间窗口正确、无重复、来源互补。  

### 步骤4：P-M-R-V标签分析｜P-M-R-V per paper（分级格式）

- **标准格式**（有论文全文/详细摘要）：单行体，中文≤40字/字段+精炼英文  
  🧩P：{≤40字CN} / {EN} ｜ 🛠M：{≤40字CN} / {EN} ｜ 📊R：{≤40字CN} / {EN} ｜ 💎V：{≤40字CN} / {EN}  
- **精简格式**（仅有标题/新闻报道）：多行体，中文≤60字/字段  
  🧩 P（问题）：{≤60字CN} / {EN}  
  🛠 M（方法）：{≤60字CN} / {EN}  
  📊 R（结果）：{≤60字CN} / {EN}  
  💎 V（价值）：{≤60字CN} / {EN}  
- **最简格式**（仅有标题/非学术报道）：仅输出P和V  
  🧩 P：{研究问题} / 💎 V：{预期价值}  
- ✓ 验证点：根据数据完整度选择合适格式，不强求单行体。  

### 步骤5：技术方向映射｜Direction mapping

- 从题目/摘要/项目/主页/新闻报道提取关键词，对照"公司技术方向分类表（带Emoji）"计分（标题×2，项目/专利×2，通用×1），取Top 3并给出"证据关键词+≤20字理由（多样句式）"。  
- **无完整关键词时**：允许定性匹配，基于研究主题和应用场景判断，给出理由但不强制计分。  
- ✓ 验证点：匹配分可溯源或定性理由充分，理由句式多样。  

### 步骤6：合作网络抽取｜Collaboration network

- 识别高频合作者（≥3篇）及机构；提取团队/实验室与角色。  
- ✓ 验证点：合作者姓名-机构对应正确。  

### 步骤7：证据收集与格式化｜Evidence collection & formatting

- 对**基本信息/核心成果/打分项**逐条附：来源链接 + 时间戳（若可得）+ 口径说明；  
- **"待核"使用原则**（大幅收紧）：  
  - ❌ 不使用"待核"的情况：姓名拼写变体、职称推断、年龄估算（±10岁）、venue等级、合作者信息、研究方向  
  - ✅ 仅在以下情况使用"（暂缺）"：ORCID完全查不到、头像无法获取、年龄完全无法推断  
  - **原则**：能推断的直接给出结论，不增加"待核"标注；只在关键信息完全缺失且无法推断时标注"（暂缺）"  
- ✓ 验证点：关键结论≥1个证据链接与日期；"（暂缺）"标注≤3处。  

### 步骤8：输出生成｜Output modes

- 单人→模式A；≥2人→逐人模式A后→模式B；强歧义→模式C。  
- ✓ 验证点：输出模式与输入条件匹配。  

---

## REF.DIRECTION_MATCHING｜方向匹配与代表作选取

### 1. 关键词抽取

- 来源：职称、论文题目与摘要、项目、专利、主页、新闻报道；进行同义归一（保留原词便于溯源）。  

### 2. 匹配逻辑

- 加权：论文标题×2；项目/专利×2；通用兴趣×1。  
- 总匹配分 = Σ(关键词权重)；取Top 3分类输出：证据关键词（3–5）＋≤20字理由（中文）＋精炼英文；理由句式多样化。  
- 无完整关键词时：允许基于研究主题和应用场景的定性匹配，给出充分理由但不强制计分。  

### 3. 代表作选取方法

#### 近24个月论文

- Score = VenueTier × 0.5 + CitationNorm × 0.2 + Relevance × 0.3  
  - VenueTier：CCF-A/CORE A* = 1.0；CCF-B/CORE A = 0.7；CCF-C/CORE B = 0.4；其他=0.2  
  - CitationNorm：该论文引用数 / 同年同领域平均引用  
  - Relevance：与公司技术方向的匹配度  
- 排序取Top N（N=5–10，视数据量调整）。  

#### 历史代表论文

- 分层去重：  
  1. 命名/奠基性工作（算法/框架/理论以作者姓名命名）  
  2. 顶级奖项/高被引（Best Paper、Fellow、>500引用）  
  3. 标准/产业影响（IEEE标准、开源框架、头部企业合作）  

---

## REF.CHECKLIST｜最终检查清单

输出前必须确认：  

- [ ] 年龄：给出估算范围或明确"（年龄信息暂缺）"  
- [ ] "（暂缺）"标注 ≤3 处（仅限 ORCID/头像/年龄完全无法推断时）  
- [ ] 论文列表：至少从 3 个数据源检索（arXiv+GS+DBLP 或其他组合）  
- [ ] 姓名拼写变体：不标注歧义，直接使用统一拼写  
- [ ] 职称/venue/合作者：不使用"待核"，直接给出判断  
- [ ] P-M-R-V：根据数据完整度选择合适格式（标准/精简/最简）  
- [ ] 方向匹配：Top 3 有证据关键词和理由（中英）  
- [ ] 证据链接：关键结论有来源+时间，URL 符合链接安全规则  

---

## REF.TRL_GUIDE｜TRL 1–9 摘要

- **TRL 1–3**：概念与原理阶段，多为理论分析、仿真或实验室早期验证。  
- **TRL 4–5**：实验室环境下的子系统/样机验证，小规模试验。  
- **TRL 6–7**：在接近真实环境中的原型系统验证与示范。  
- **TRL 8–9**：工程定型、量产前验证、商业化部署与成熟运营。  

评估 TRL 时：  

- 结合论文方法学描述、项目阶段说明、新闻报道的应用案例、合作企业信息综合判断；  
- 在报告中给出：TRL 数值 + 一句中文依据 + 简洁英文说明 + 标准或权威介绍页面链接（如 NASA/ISO 说明页）。  
