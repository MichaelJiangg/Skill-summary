---
name: article-visual-summary
description: >
  将会议纪要或文章报告转化为钉钉风格图文摘要 HTML 文件（蓝色系主题，支持一键下载 PNG）。
  触发词：「图文纪要」「一图总结」「摘要卡」「可视化摘要」「钉钉风格总结」。
  输出结构：左侧信息边栏 + 3个固定 Section（主体卡片 / 汇总网格 / 小结+建议）。
  采用总分总信息架构：标题下方设结论先行条，正文展开，Section 03 收尾。
  基础信息条置于 Header 内副标题之下，极简细线分隔风格，不抢总结视觉权重。
  内置 html2canvas 下载按钮，风格还原钉钉 AI 纪要设计语言，使用蓝色主色系。
---

# 图文摘要 Skill（钉钉风格·蓝色系）

## 一、使用触发条件

当用户提供文章/会议纪要/报告并说：
- "帮我做成图文纪要" / "一图总结" / "摘要卡片" / "钉钉风格"

→ 执行本 Skill，输出 `.html` 文件，用户可截图或点击按钮下载 PNG。

---

## 整体信息架构：总 → 分 → 总

```
标题 + 副标题
└── 【总·开篇】结论先行条        ← 1~2句话，概括全文最核心结论，置于正文之前
    ├── 【分】Section 01 主体展开  ← 3~5列卡片，详细内容
    ├── 【分】Section 02 汇总网格  ← 关键信息分类聚合
    └── 【总·收尾】Section 03 小结+建议  ← 综合评价 + actionable 建议
水印
```

**结论先行条是本模板的核心设计原则：**
读者扫一眼标题 + 结论条，即可在 5 秒内理解全文价值，再决定是否深读正文。

---

## 二、内容解析（动手前必读）

### 2.1 提取字段

| 字段 | 说明 | 长度 |
|---|---|---|
| `TITLE` | 主标题 | ≤ 18 字 |
| `SUBTITLE` | 副标题（会议类型/文章来源/场景说明） | ≤ 30 字 |
| `CONCLUSION_BAR` | **结论先行条**：1~2句话提炼全文最核心结论/判断/决策，置于标题下方正文之前 | ≤ 80 字 |
| `INFO_BAR_ITEMS` | 顶部横条键值对，3～6 条，等宽排列 | 每条键≤6字，值≤16字 |
| `S1_TITLE` | 第一段标题 | ≤ 10 字 |
| `S1_TAG` | 第一段右侧标签 | ≤ 4 字 |
| `S1_CARDS` | 主体卡片列表，3～5张 | 见 2.2 |
| `S2_TITLE` | 第二段标题 | ≤ 10 字 |
| `S2_CELLS_TOP` | 汇总网格上行，2～4 格（正向/事实类） | 见 2.3 |
| `S2_CELLS_BOT` | 汇总网格下行，1～3 格（风险/待澄清类） | 见 2.3 |
| `S3_CONCLUSION` | 小结段落，综合评价 | ≤ 120 字 |
| `S3_ADVICE` | 建议列表，2～4 条 actionable 建议 | 每条 ≤ 40 字 |

**结论先行条写作要求：**
- 必须是**判断句/结论句**，而非描述句。"本文分析了…"是描述，"两者定位不同，互补而非竞争"才是结论
- 优先提炼最重要的 **1个核心判断** + **1个关键行动方向**
- 会议纪要 → "本次会议核心决策是…，下一步重点是…"
- 文章/报告 → "核心结论：…；对我们的启示是…"

### 2.2 主体卡片（S1_CARDS）每张结构

| 字段 | 说明 | 规则 |
|---|---|---|
| `icon` | Emoji 图标 | 见图标参考表 |
| `title` | 卡片标题 | ≤ 10 字 |
| `badge_text` | 状态/类别标签 | ≤ 5 字 |
| `badge_color` | 标签颜色 | 见 3.1 |
| `bullets` | 要点列表 | 3～5条，每条≤30字，含行内高亮 |
| `insight` | 底部 Insight 语 | ≤ 20 字，斜体，判断句而非描述句 |
| `insight_color` | Insight 颜色 | 与卡片 badge 主色一致 |

**列数规则：**
- 内容 ≤ 3 个主题 → 3列
- 内容 4～5 个主题 → 5列（卡片字号略小）

### 2.3 汇总网格（S2）每格结构

| 字段 | 说明 |
|---|---|
| `cell_label` | 格子标题（如"核心技能"/"待澄清项"） |
| `cell_content` | 格子正文，支持行内高亮 |
| `cell_type` | `top`（正向/事实，白底）或 `bot`（风险/待处理，浅橙底） |

**内容类型 → 汇总格子参考：**

| 内容类型 | 上行格（正向） | 下行格（风险/待跟进） |
|---|---|---|
| 面试记录 | 核心技能、项目成果、绩效表现 | 待澄清项、风险标签 |
| 会议纪要 | 决策事项、行动计划、责任分工 | 待跟进项、风险点 |
| 产品文章 | 核心价值、适用场景、关键数据 | 局限性、待验证点 |
| 研究报告 | 核心发现、数据支撑、结论 | 研究局限、延伸问题 |

---

## 三、视觉规范

### 3.1 Badge 颜色系统

| badge_color 值 | 含义 | 效果 |
|---|---|---|
| `blue` | 参考/背景/信息类 | 蓝字蓝边蓝底 |
| `green` | 完成/正向/已确认 | 绿字绿边绿底 |
| `orange` | 进行中/待跟进/警示 | 橙字橙边橙底 |
| `pink` | 风险/待处理/问题 | 粉字粉边粉底 |
| `purple` | 类别/领域分类 | 紫字紫边紫底 |

### 3.2 行内高亮规则

| span class | 用于 | 颜色 |
|---|---|---|
| `hl-blue` | 工具名、产品名、专有名词 | 蓝色 |
| `hl-orange` | 数字/量化指标、交付物、截止时间 | 橙色 |
| `hl-red` | 风险词、问题描述、负面结论 | 红色 |
| `hl-green` | 正向结论、达标表现 | 绿色 |
| `hl-purple` | 方法论、框架、类别词 | 紫色 |

**原则：每句话最多 1～2 个高亮词，不整句高亮。**

### 3.3 Insight 颜色规则

| insight_color 值 | 效果 |
|---|---|
| `blue` | `#1677FF` |
| `green` | `#389E0D` |
| `orange` | `#D46B08` |
| `pink` | `#C41D7F` |
| `purple` | `#531DAB` |

### 3.4 图标参考

| 主题 | Emoji |
|---|---|
| 人物/候选人 | 👤 🧑‍💼 |
| 内容/文档/策划 | 📄 📝 ✍️ |
| 技术/系统/开发 | ⚙️ 🔧 💻 🛠️ |
| 数据/分析 | 📊 📈 🔢 |
| 市场/用户/增长 | 🚀 👥 📣 |
| 风险/问题 | ⚠️ 🔴 |
| 完成/决策 | ✅ 🎯 |
| 产品/功能 | 📦 💡 🏷️ |
| 会议/沟通 | 💬 🗣️ 📋 |
| 竞品/对比 | 🔍 ⚖️ |

---

## 四、HTML 完整模板

将下方模板中所有 `{{变量}}` 替换为实际内容。
注释 `<!-- CHOICE: ... -->` 说明结构选择，生成时删除注释。

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1.0">
<title>图文摘要</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
<style>
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}
body{
  font-family:-apple-system,"PingFang SC","Hiragino Sans GB",
    "Microsoft YaHei",sans-serif;
  background:#EBF1FF;
  padding:28px 24px 48px;
  min-height:100vh;
}
.dl-bar{
  max-width:1100px;margin:0 auto 12px;
  display:flex;justify-content:flex-end;
}
.btn-dl{
  display:inline-flex;align-items:center;gap:6px;
  background:#1677FF;color:#fff;border:none;border-radius:8px;
  padding:8px 18px;font-size:14px;font-weight:600;
  cursor:pointer;box-shadow:0 2px 8px rgba(22,119,255,.35);
  transition:background .2s;
}
.btn-dl:hover{background:#0958D9}
.card{
  background:#fff;border-radius:16px;
  max-width:1100px;margin:0 auto;
  box-shadow:0 4px 24px rgba(0,0,0,.08);overflow:hidden;
}

/* ── Header ── */
.card-header{padding:28px 36px 20px;border-bottom:1.5px solid #E8EEF8}
/* info-bar 置于 card-header 内，紧接副标题 */
.card-header h1{font-size:26px;font-weight:700;color:#0A1628;letter-spacing:.5px;margin-bottom:4px}
.card-header .subtitle{font-size:13px;color:#8896B3}

/* ── 结论先行条（总·开篇）── */
.conclusion-bar{
  margin:0 36px;
  padding:14px 20px;
  background:#EFF4FF;
  border-left:4px solid #1677FF;
  border-radius:0 10px 10px 0;
  display:flex;align-items:flex-start;gap:12px;
}
.conclusion-bar .cb-icon{font-size:18px;flex-shrink:0;margin-top:1px}
.conclusion-bar .cb-text{font-size:14px;color:#1A2744;line-height:1.7;font-weight:500}
/* 结论条与正文之间的间距 */
.body-gap{height:20px}

/* ── Body layout ── */
.card-body{display:flex;align-items:stretch}

/* ── Sidebar ── */
.info-bar{
  display:flex;gap:0;
  margin-top:16px;
  border-top:1px solid #E8EEF8;
  border-bottom:1px solid #E8EEF8;
  padding:10px 0;
}
.info-bar-item{
  flex:1;padding:8px 16px;
  border-right:1px solid #E8EEF8;
}
.info-bar-item:last-child{border-right:none}
.info-bar-item .s-label{
  font-size:10.5px;color:#B0BDD0;font-weight:500;
  margin-bottom:3px;letter-spacing:.3px;text-transform:uppercase;
}
.info-bar-item .s-value{font-size:12.5px;color:#4A5568;font-weight:600;line-height:1.4}

/* ── Sections ── */
.sections{flex:1;padding:20px 28px 24px;display:flex;flex-direction:column;gap:24px;min-width:0}
.section-hd{display:flex;align-items:center;gap:10px;margin-bottom:14px}
.section-num{
  display:inline-flex;align-items:center;justify-content:center;
  width:28px;height:28px;background:#1677FF;color:#fff;
  border-radius:6px;font-size:13px;font-weight:700;flex-shrink:0;
}
.section-name{font-size:17px;font-weight:700;color:#0A1628;flex:1}
.section-tag{font-size:12px;color:#8896B3;font-style:italic;white-space:nowrap}

/* ── Cards grid ── */
.cards-grid{display:grid;gap:12px}
.cards-grid.cols-3{grid-template-columns:repeat(3,1fr)}
.cards-grid.cols-5{grid-template-columns:repeat(5,1fr)}
.topic-card{
  background:#FAFBFF;border:1px solid #E3EAF8;
  border-radius:12px;padding:14px 14px 12px;
  display:flex;flex-direction:column;
}
.cols-5 .topic-card{padding:12px 10px 10px}
.cols-5 .topic-card *{font-size:11.5px!important}
.cols-5 .card-title{font-size:13px!important}
.card-hd{display:flex;align-items:center;gap:7px;margin-bottom:10px;flex-wrap:wrap}
.card-icon{font-size:16px;flex-shrink:0}
.card-title{font-size:14px;font-weight:700;color:#0A1628}
.card-body-inner{flex:1;margin-bottom:10px}
.card-body-inner ul{padding-left:15px;display:flex;flex-direction:column;gap:5px}
.card-body-inner ul li{font-size:12.5px;color:#3D4F73;line-height:1.6}
.card-insight{
  font-size:12px;font-style:italic;font-weight:600;
  padding-top:8px;border-top:1px solid #EEF2FA;line-height:1.5;
}
.card-insight.blue  {color:#1677FF}
.card-insight.green {color:#389E0D}
.card-insight.orange{color:#D46B08}
.card-insight.pink  {color:#C41D7F}
.card-insight.purple{color:#531DAB}

/* ── Summary grid ── */
.summary-grid{display:flex;flex-direction:column;gap:8px}
.summary-row{display:grid;gap:8px}
.summary-cell{border-radius:10px;padding:12px 14px}
.summary-cell.type-top{background:#F5F8FF;border:1px solid #D9E7FF}
.summary-cell.type-bot{background:#FFF8F0;border:1px solid #FFD8A8}
.cell-label{font-size:11px;color:#8896B3;font-weight:600;margin-bottom:6px;letter-spacing:.3px}
.cell-content{font-size:12.5px;color:#1A2744;line-height:1.7}

/* ── Conclude ── */
.conclude-row{display:grid;grid-template-columns:1fr 1fr;gap:12px}
.conclude-left{background:#F0F5FF;border-radius:12px;padding:16px 18px}
.conclude-left p{font-size:13px;color:#1A2744;line-height:1.75}
.conclude-right{background:#F6FFED;border-radius:12px;padding:16px 18px}
.conclude-right .advice-label{font-size:12px;font-weight:700;color:#389E0D;margin-bottom:10px;display:flex;align-items:center;gap:6px}
.conclude-right ol{padding-left:18px;display:flex;flex-direction:column;gap:7px}
.conclude-right ol li{font-size:12.5px;color:#1A2744;line-height:1.65}

/* ── Badges ── */
.badge{
  display:inline-flex;align-items:center;
  padding:2px 8px;border-radius:20px;
  font-size:11px;font-weight:600;white-space:nowrap;line-height:1.6;
}
.badge.blue  {background:#E6F4FF;color:#1677FF;border:1px solid #91CAFF}
.badge.green {background:#F6FFED;color:#52C41A;border:1px solid #B7EB8F}
.badge.orange{background:#FFF7E6;color:#FA8C16;border:1px solid #FFD591}
.badge.pink  {background:#FFF0F6;color:#EB2F96;border:1px solid #FFADD2}
.badge.purple{background:#F9F0FF;color:#722ED1;border:1px solid #D3ADF7}

/* ── Highlights ── */
.hl-blue  {color:#1677FF;font-weight:600}
.hl-orange{color:#D46B08;font-weight:600}
.hl-red   {color:#CF1322;font-weight:600}
.hl-green {color:#389E0D;font-weight:600}
.hl-purple{color:#531DAB;font-weight:600}

/* ── Watermark ── */
.watermark{
  text-align:right;font-size:11px;color:#C0C9DC;
  padding:10px 36px 14px;border-top:1px solid #EEF2FA;
}
</style>
</head>
<body>

<div class="dl-bar">
  <button class="btn-dl" onclick="downloadCard()">⬇ 下载为图片</button>
</div>

<div class="card" id="capture">

  <!-- 页眉（info-bar 置于 header 内，紧接副标题） -->
  <div class="card-header">
    <h1>{{TITLE}}</h1>
    <p class="subtitle">{{SUBTITLE}}</p>
    <!-- 基础信息条：3～6项，等宽排列，标签大写浅灰，value 中灰加粗 -->
    <div class="info-bar">
      <div class="info-bar-item">
        <div class="s-label">{{KEY_1}}</div>
        <div class="s-value">{{VALUE_1}}</div>
      </div>
      <div class="info-bar-item">
        <div class="s-label">{{KEY_2}}</div>
        <div class="s-value">{{VALUE_2}}</div>
      </div>
      <!-- 继续添加，最多 6 项 -->
    </div>
  </div>

  <!-- 基础信息横条（REPEAT: 每项复制一个 info-bar-item，共 3～6 项）-->
  <div class="info-bar">
    <div class="info-bar-item">
      <div class="s-label">{{KEY_1}}</div>
      <div class="s-value">{{VALUE_1}}</div>
    </div>
    <div class="info-bar-item">
      <div class="s-label">{{KEY_2}}</div>
      <div class="s-value">{{VALUE_2}}</div>
    </div>
    <!-- 继续添加，每项等宽 flex:1 -->
  </div>

  <div class="card-body">
    <div class="sections">

      <!-- 【总·开篇】结论先行条，放在 sections 内第一位，与 section 内容左对齐 -->
      <div class="conclusion-bar">
        <div class="cb-header">
          <span class="cb-icon">✏️</span>
          <span class="cb-title">总结</span>
        </div>
        <div class="cb-text">{{CONCLUSION_BAR}}</div>
      </div>

      <!-- 【分】Section 01：主体卡片 -->
      <!-- CHOICE: cards-grid 的 class 用 cols-3 或 cols-5 -->
      <div class="section">
        <div class="section-hd">
          <span class="section-num">01</span>
          <span class="section-name">{{S1_TITLE}}</span>
          <span class="section-tag">/{{S1_TAG}}/</span>
        </div>
        <div class="cards-grid cols-3">
          <!-- REPEAT: 每张卡片复制一个 topic-card -->
          <div class="topic-card">
            <div class="card-hd">
              <span class="card-icon">{{ICON_1}}</span>
              <span class="card-title">{{CARD_TITLE_1}}</span>
              <span class="badge {{BADGE_COLOR_1}}">{{BADGE_TEXT_1}}</span>
              <!-- 无 badge 时删除 span.badge -->
            </div>
            <div class="card-body-inner">
              <ul>
                <li>{{BULLET_1}}</li>
                <li>{{BULLET_2}}</li>
                <li>{{BULLET_3}}</li>
              </ul>
            </div>
            <div class="card-insight {{INSIGHT_COLOR_1}}">{{INSIGHT_TEXT_1}}</div>
          </div>
          <!-- 继续复制卡片… -->
        </div>
      </div>

      <!-- 【分】Section 02：汇总网格 -->
      <div class="section">
        <div class="section-hd">
          <span class="section-num">02</span>
          <span class="section-name">{{S2_TITLE}}</span>
          <span class="section-tag">/汇总/</span>
        </div>
        <div class="summary-grid">
          <!-- 上行：正向/事实类，列数 = TOP格数 -->
          <div class="summary-row top-row"
               style="grid-template-columns:repeat({{TOP_COL_COUNT}},1fr)">
            <div class="summary-cell type-top">
              <div class="cell-label">{{TOP_LABEL_1}}</div>
              <div class="cell-content">{{TOP_CONTENT_1}}</div>
            </div>
            <!-- 继续添加… -->
          </div>
          <!-- 下行：风险/待处理类，列数 = BOT格数 -->
          <div class="summary-row bot-row"
               style="grid-template-columns:repeat({{BOT_COL_COUNT}},1fr)">
            <div class="summary-cell type-bot">
              <div class="cell-label">{{BOT_LABEL_1}}</div>
              <div class="cell-content">{{BOT_CONTENT_1}}</div>
            </div>
            <!-- 继续添加… -->
          </div>
        </div>
      </div>

      <!-- 【总·收尾】Section 03：小结 + 建议 -->
      <div class="section">
        <div class="section-hd">
          <span class="section-num">03</span>
          <span class="section-name">{{S3_TITLE}}</span>
          <span class="section-tag">/小结·建议/</span>
        </div>
        <div class="conclude-col">
          <!-- 上行：综合结论全宽 -->
          <div class="conclude-top">
            <p>{{S3_CONCLUSION}}</p>
          </div>
          <!-- 下行：3格建议卡，每卡对应一类人群/场景 -->
          <div class="conclude-bot">
            <div class="advice-card">
              <div class="ac-header">
                <span class="ac-icon">{{ADVICE_ICON_1}}</span>
                <span class="ac-title">{{ADVICE_TITLE_1}}</span>
              </div>
              <div class="ac-body">{{ADVICE_BODY_1}}</div>
            </div>
            <div class="advice-card">
              <div class="ac-header">
                <span class="ac-icon">{{ADVICE_ICON_2}}</span>
                <span class="ac-title">{{ADVICE_TITLE_2}}</span>
              </div>
              <div class="ac-body">{{ADVICE_BODY_2}}</div>
            </div>
            <div class="advice-card">
              <div class="ac-header">
                <span class="ac-icon">{{ADVICE_ICON_3}}</span>
                <span class="ac-title">{{ADVICE_TITLE_3}}</span>
              </div>
              <div class="ac-body">{{ADVICE_BODY_3}}</div>
            </div>
          </div>
        </div>
      </div>

    </div><!-- /sections -->
  </div><!-- /card-body -->

  <div class="watermark"><span class="wm-brand"><span class="wm-text">Powered by OpenClaw</span> <span class="wm-emoji">🦞</span></span><span class="wm-author">jiangdelinp@foxmail.com</span></div>
</div>

<script>
async function downloadCard(){
  const btn=document.querySelector('.btn-dl');
  const el=document.getElementById('capture');
  btn.textContent='生成中…';btn.disabled=true;
  try{
    const canvas=await html2canvas(el,{
      scale:2,useCORS:true,backgroundColor:'#ffffff',logging:false
    });
    const link=document.createElement('a');
    link.download='图文摘要.png';
    link.href=canvas.toDataURL('image/png');
    link.click();
  }finally{
    btn.textContent='⬇ 下载为图片';btn.disabled=false;
  }
}
</script>
</body>
</html>
```

---

## 五、输出规范

1. 生成完整 HTML 文件，命名：`summary_{{主题关键词}}.html`
2. 保存路径：`/mnt/user-data/outputs/summary_XXX.html`
3. 用 `present_files` 工具向用户展示文件
4. 告知用户：浏览器打开 → 右上角点「下载为图片」→ 保存 PNG

---

## 六、生成质量检查清单

- [ ] 结论先行条是**判断句**，不是"本文介绍了…"式的描述句
- [ ] 结论先行条能让人在 5 秒内理解全文核心价值
- [ ] 所有 `{{变量}}` 已替换，无残留占位符
- [ ] 每张卡片都有 insight（不为空，是判断而非描述）
- [ ] 行内高亮不超过每句 2 个词
- [ ] 汇总网格 `grid-template-columns` 与实际格数一致
- [ ] 无 badge/责任人时对应标签已删除

---

## 七、内容类型快速映射

| 内容类型 | 结论先行条示例 | S1 标题/标签 | S2 上行格 | S2 下行格 |
|---|---|---|---|---|
| 面试记录 | "候选人技术能力突出，但测试用例设计存在验证盲区，建议业务Leader二面重点考察" | 面试问答/问答 | 核心技能、项目成果、绩效 | 待澄清、风险项 |
| 会议纪要 | "核心决策已达成，GPU资源到位时间是唯一关键路径风险" | 议题讨论/议题 | 决策事项、行动项、分工 | 待跟进、风险点 |
| 产品文章 | "两者定位互补而非竞争，Claude补齐本地化三项能力后差距将显著缩小" | 核心观点/分析 | 核心价值、数据支撑、场景 | 局限性、待验证 |
| 研究报告 | "行业增速放缓已成定局，头部集中化是唯一确定趋势，中腰部玩家需在Q3前完成差异化卡位" | 研究发现/洞察 | 核心结论、数据依据、方法 | 研究局限、延伸 |
