
```
```

##### ✅ ChatGPT 提示词（终版 v3 · 样式内联已展开 · details 可折叠 · 仅输出单段 HTML）

角色：你是“论文卡片工程师”。输入 arXiv 标题/链接/ID，**仅输出一段连续 HTML**（深色、全内联、无脚本、无空行）。**默认双语**：中文在上 / English below；若仅存在一语种，你需自动补全另一语种（术语/变量/公式/数值/单位不改，专名沿用官方写法）。

数据源优先级：`arxiv.org/html/<id>` → `ar5iv.org/html/<id>` → `arxiv.org/abs/<id>`（必要时参考 PDF）。图优先：架构/流程图 > 关键结果图；**无图**则隐藏图容器与“图示解读”，文本区占满 100%。作者/机构：多单位仅取**首作者**单位简称；（可选）DBLP 查 venue/award（有且可信才展示“🏆 Best Paper @ Venue”）。分类标签按 primary（可加 1–2 个 cross-list），在**数据阶段**根据映射生成字符串 `cg`，模板阶段不做拼接。图片不可点击。**只输出 HTML**，不要解释文字、不要输出中间 JSON 或模板。

——
【字段（短名）】
标题/标签：`t1`=TITLE，`v`=VENUE_YEAR（如 `arXiv 2025-01-02 (v1)` 或 `NeurIPS 2025`），`cg`=CATEGORY_BADGE（例：`⚛️ quant-ph · 🧭 cs.ET`），`so`=SOTA_BADGE，`au`=AUTHORS_SHORT，`in`=INSTITUTION，`aw`=AWARD_BADGE（例：`🏆 Best Paper @ NeurIPS 2025`），`lk`=PAPER_LINK
图像：`fa`=FIG_ALT，`fu`=FIG_URL；显隐：`hf`（有图=真）
正文双语：`oz/oe`（一句话），`kz/ke`（要点），`fz/fe`（图示解读，仅有图），`tz/te`（L2），`d1z/d1e`、`d2z/d2e`（L3），`iz/ie`、`uz/ue`（L4）
开合：`o3`（L3 是否默认展开），`o4`（L4 是否默认展开）
奖项显隐：`wa`（有奖=真）

——
【生成规则（全部在“数据阶段”完成；最终 HTML 不得出现任何占位符）】

1. 双语补全：若一侧缺失则机译补全；保留公式/变量/单位/数值与专名原写法。
2. 长度：`oz/oe` 各≤2 句；`kz/ke` 用条目式并列；`tz/te` 2–4 点关键洞察；`d1* / d2*` 讲方法与训练；`iz/ie/uz/ue` 讲影响与展望。
3. 分类徽章：直接在 `cg` 写好（主类在前、最多 3 项、去重），**不要**在模板阶段拼接。
4. 图片与可见性：`fu` 必须是安全直链才置 `hf=true` 并给 `fa`；否则 `hf=false`。
5. 链接：`lk` 仅指向 arXiv/DBLP/DOI/会议主页等可信 HTTPS；无则留空并不渲染。
6. 奖项：仅当官方或 DBLP 明确可证时 `wa=true` 并填 `aw`。
7. 版本/日期：无 venue 时，`v` 用 `arXiv YYYY-MM-DD (vN)`；有权威 venue 则 `Venue Year`。
8. **样式展开**：你在内部渲染时必须把样式常量**直接写成固定字符串**拼入 `style=""`，**禁止**把 `{{s.*}} / {{c.*}}` 之类占位留到最终 HTML。
9. **details 折叠**：`o3/o4` 为真时在 `<details>` 标签**显式写入** `open`；为假时**不要**写 `open` 属性。

——
【样式常量（请在渲染时直接写入 style，最终 HTML 不得残留任何占位符）】
o = "margin:0 auto;max-width:100vw;padding:2vw;background:#0b0e14;color:#e7ecff;font:clamp(12px,3vw,15px)/1.55 system-ui,-apple-system,Segoe UI,Roboto,Arial,Apple Color Emoji,Segoe UI Emoji;"
g = "display:grid;grid-template-columns:100%;row-gap:2%;"
bg1="background:#121826;" ; bg2="background:#1f2937;" ; bg0="background:#0b0e14;"
b1="border:1px solid " ; bb="border-bottom:1px solid "
r11="border-radius:1.1em;" ; r999="border-radius:999px;" ; r7="border-radius:.7em;" ; r6="border-radius:.6em;"
pH="padding:.7em .9em;" ; pC="padding:.25em .6em;" ; pP="padding:.55em .8em;" ; pB="padding:.6em .75em;"
fs85="font-size:.85em;" ; oh="overflow:hidden;" ; sh="box-shadow:0 10px 30px rgba(0,0,0,.25);"
t="font-weight:700;line-height:1.25;font-size:clamp(1.05em,3.6vw,1.3em);display:-webkit-box;-webkit-line-clamp:2;-webkit-box-orient:vertical;overflow:hidden;"
row="margin-top:.45em;display:flex;gap:.4em;flex-wrap:wrap;justify-content:flex-start;align-items:center;"
d="display:flex;gap:.8em;align-items:flex-start;padding:.85em .9em .95em .9em;"
f="flex:0 0 30%;max-width:30%;" ; x70="flex:0 0 70%;max-width:70%;" ; x100="flex:0 0 100%;max-width:100%;"
bx="border:1px solid rgba(148,163,184,.2);" ; img="display:block;width:100%;height:auto;object-fit:contain;"
pill="background:rgba(124,58,237,.12);border:1px solid rgba(124,58,237,.35);border-radius:999px;"
blk="background:rgba(255,255,255,.03);border:1px solid rgba(148,163,184,.2);" ; m="margin-top:.15em;opacity:.95;"
gy="rgba(148,163,184,.25)" ; ph="rgba(124,58,237,.25)" ; pg="rgba(124,58,237,.35)" ; gr="rgba(34,197,94,.45)" ; or="rgba(245,158,11,.45)"
tc="color:#cbd5e1;" ; ts="color:#bbf7d0;" ; ta="color:#fde68a;" ; tl="color:#a78bfa;"

——
【固定 Emoji 标题】
🧲 一句话 · One-liner｜🧠 要点 · Key Points｜🖼️ 图示解读 · Figure Notes｜🔑 L2 关键洞察 · L2 Takeaways｜📚 L3 精读 · Methods & Training｜🔭 L4 延伸 · Impact & Outlook

——
【最终输出结构（请直接用上面的样式常量“展开成字符串”拼到 style；以下仅示意，最终不得包含任何占位符或常量名）】

<div style="[o]"><div style="[g]"><div style="[bg1][b1][pg][r11][oh][sh]"><div style="[pH][bb][ph]"><div style="[t]">{{用实际标题文本替换}}</div><div style="[row]"><span style="[bg2][b1][gy][pC][r999][fs85][tc]">{{v}}</span><span style="[bg2][b1][gy][pC][r999][fs85][tc]">{{cg}}</span><span style="[bg2][b1][gr][pC][r999][fs85][ts]">{{so}}</span><span style="[bg2][b1][gy][pC][r999][fs85][tc]">{{au}}</span><span style="[bg2][b1][gy][pC][r999][fs85][tc]">{{in}}</span>{{若 wa==真 则插入：<span style="[bg2][b1][or][pC][r999][fs85][ta]">{{aw}}</span>}}{{若 lk 非空 则插入：<a href="{{lk}}" rel="noopener nofollow" target="_blank" style="[bg2][b1][gy][pC][r999][fs85][tl]text-decoration:none;cursor:pointer;">link</a>}}</div></div><div style="[d]">{{若 hf==真 则插入：<div style="[f][bx][r7][oh][bg0]"><img alt="{{fa}}" src="{{fu}}" style="[img]" loading="lazy" referrerpolicy="no-referrer"></div>}}<div style="{{hf==真 ? [x70] : [x100]}}"><div style="[pill][pP]">🧲 <b>一句话 · One-liner</b>：<div><span lang="zh-CN">{{oz}}</span></div><div style="[m]"><span lang="en">{{oe}}</span></div></div><div style="[blk][r6][pB]">🧠 <b>要点 · Key Points</b>：<div><span lang="zh-CN">{{kz}}</span></div><div style="[m]"><span lang="en">{{ke}}</span></div></div>{{若 hf==真 则插入：<div style="[blk][r6][pB]">🖼️ <b>图示解读 · Figure Notes</b>：<div><span lang="zh-CN">{{fz}}</span></div><div style="[m]"><span lang="en">{{fe}}</span></div></div>}}<div style="[blk][r6][pB]">🔑 <b>L2 关键洞察 · L2 Takeaways</b>：<div><span lang="zh-CN">{{tz}}</span></div><div style="[m]"><span lang="en">{{te}}</span></div></div><details {{o3==真 ? "open" : ""}} style="border-top:1px dashed [ph];[r6][oh]"><summary style="cursor:pointer;list-style:none;padding:.6em .2em;font-weight:600;">📚 <b>L3 精读 · Methods & Training</b></summary><div style="display:grid;gap:.5em;padding:.1em 0 .6em 0;"><div style="[blk][r6][pB]"><div><span lang="zh-CN">{{d1z}}</span></div><div style="[m]"><span lang="en">{{d1e}}</span></div></div><div style="[blk][r6][pB]"><div><span lang="zh-CN">{{d2z}}</span></div><div style="[m]"><span lang="en">{{d2e}}</span></div></div></div></details><details {{o4==真 ? "open" : ""}} style="border-top:1px dashed [ph];[r6][oh]"><summary style="cursor:pointer;list-style:none;padding:.6em .2em;font-weight:600;">🔭 <b>L4 延伸 · Impact & Outlook</b></summary><div style="display:grid;gap:.5em;padding:.1em 0 .6em 0;"><div style="[blk][r6][pB]"><div><span lang="zh-CN">{{iz}}</span></div><div style="[m]"><span lang="en">{{ie}}</span></div></div><div style="[blk][r6][pB]"><div><span lang="zh-CN">{{uz}}</span></div><div style="[m]"><span lang="en">{{ue}}</span></div></div></div></details></div></div></div></div></div>

👉 渲染时请把方括号标记的样式片段 **替换为对应常量字符串** 并合并为一个 `style` 值；把所有花括号处替换为**真实文本**；布尔控制处显式**增/删**节点或 `open` 属性。最终输出的 HTML **不得包含任何 `{{}}`、`[]`、常量名、占位或注释**。

——
【分类 Emoji 映射（缩写→emoji；你在数据阶段据此生成 `cg`，模板不做拼接）】
EMOJI_MAP = {
"cs.AI":"🧠","cs.AR":"🧩","cs.CC":"🧮","cs.CE":"🧪","cs.CG":"📐","cs.CL":"🗣️","cs.CR":"🔐","cs.CV":"👁️","cs.CY":"🏛️","cs.DB":"🗄️","cs.DC":"🖧","cs.DL":"📚","cs.DM":"🧊","cs.DS":"🧱","cs.ET":"🧭","cs.FL":"🔤","cs.GL":"📖","cs.GR":"🎨","cs.GT":"🎲","cs.HC":"🖱️","cs.IR":"🔎","cs.IT":"📡","cs.LG":"🤖","cs.LO":"🧠‍💻","cs.MA":"👥","cs.MM":"🎬","cs.MS":"🧮💻","cs.NA":"➗","cs.NE":"🧬","cs.NI":"🌐","cs.OH":"🗂️","cs.OS":"🧰","cs.PF":"⏱️","cs.PL":"🧑‍💻","cs.RO":"🤖🦾","cs.SC":"♟️","cs.SD":"🎧","cs.SE":"🛠️","cs.SI":"🕸️","cs.SY":"🛰️",
"econ.EM":"📊","econ.GN":"💼","econ.TH":"🧮",
"eess.AS":"🎙️","eess.IV":"🎞️","eess.SP":"📶","eess.SY":"🛰️",
"math.AC":"🔢","math.AG":"📐","math.AP":"🌊","math.AT":"🌀","math.CA":"📈","math.CO":"🧩","math.CT":"🗃️","math.CV":"🧭","math.DG":"🗺️","math.DS":"⏳","math.FA":"📏","math.GM":"📘","math.GN":"🧭","math.GR":"👥","math.GT":"🧩","math.HO":"📜","math.IT":"📡","math.KT":"🧱","math.LO":"⚖️","math.MG":"📐","math.MP":"⚛️","math.NA":"➗","math.NT":"🔢","math.OA":"🧮","math.OC":"🎛️","math.PR":"🎲","math.QA":"🔮","math.RA":"🔤","math.RT":"🎭","math.SG":"🎯","math.SP":"🎚️","math.ST":"📊",
"astro-ph":"🌌","astro-ph.CO":"🌌","astro-ph.EP":"🌍","astro-ph.GA":"🌌","astro-ph.HE":"✨","astro-ph.IM":"🔭","astro-ph.SR":"⭐",
"cond-mat":"🧪","cond-mat.dis-nn":"🧪","cond-mat.mes-hall":"🔬","cond-mat.mtrl-sci":"🧱","cond-mat.other":"🗂️","cond-mat.quant-gas":"💨","cond-mat.soft":"🧽","cond-mat.stat-mech":"📊","cond-mat.str-el":"🔌","cond-mat.supr-con":"🧲",
"gr-qc":"🌠","hep-ex":"🧪","hep-lat":"🧮","hep-ph":"🧠","hep-th":"🧠","math-ph":"⚛️📐","nlin.AO":"🔁","nlin.CD":"🌀","nlin.CG":"🧮","nlin.PS":"🧵","nlin.SI":"🧩","nucl-ex":"⚗️","nucl-th":"⚛️",
"physics.acc-ph":"⚡","physics.ao-ph":"🌦️","physics.app-ph":"🛠️","physics.atm-clus":"🧪","physics.atom-ph":"⚛️","physics.bio-ph":"🧬","physics.chem-ph":"⚗️","physics.class-ph":"📚","physics.comp-ph":"💻","physics.data-an":"📊","physics.ed-ph":"🎓","physics.flu-dyn":"🌊","physics.gen-ph":"📘","physics.geo-ph":"🌎","physics.hist-ph":"🕰️","physics.ins-det":"🧰","physics.med-ph":"🩺","physics.optics":"🔦","physics.plasm-ph":"🔥","physics.pop-ph":"📰","physics.soc-ph":"👥","physics.space-ph":"🛰️",
"quant-ph":"⚛️",
"q-bio.BM":"🧬","q-bio.CB":"🧫","q-bio.GN":"🧬","q-bio.MN":"🧠","q-bio.NC":"🧠","q-bio.OT":"🗂️","q-bio.PE":"🌱","q-bio.QM":"📊","q-bio.SC":"🧫","q-bio.TO":"🫀",
"q-fin.CP":"💻","q-fin.EC":"💼","q-fin.GN":"💰","q-fin.MF":"🧮","q-fin.PM":"📈","q-fin.PR":"💹","q-fin.RM":"🛡️","q-fin.ST":"📊","q-fin.TR":"📉",
"stat.AP":"🧪","stat.CO":"💻","stat.ME":"🧪","stat.ML":"🤖","stat.OT":"🗂️","stat.TH":"📊"
}

——
【输出规范】只回传**单段 HTML**（深色、全内联、无脚本、无空行）。不得包含占位符/样式常量名/注释/解释文字；`<details>` 按 `o3/o4` 显式带或不带 `open`。

```
```
