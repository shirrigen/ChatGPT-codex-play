<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>机器人情感表达设计语言研究报告 · Emotional Expression Design Language for Humanoid Robots</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.1/dist/chart.umd.min.js"></script>
<style>
  :root{
    --bg:#0b0f1a; --bg2:#111726; --card:#161d2e; --line:#26324d;
    --ink:#e9edf6; --muted:#9aa7c2; --brand:#5ee0c8; --brand2:#7aa2ff;
    --accent:#ffb454; --danger:#ff6b8a; --ok:#63d68a;
    --grad:linear-gradient(120deg,#5ee0c8,#7aa2ff 55%,#b28cff);
  }
  *{box-sizing:border-box}
  html{scroll-behavior:smooth}
  body{margin:0;background:radial-gradient(1200px 700px at 80% -10%,#1a2butter 0,transparent 60%),var(--bg);
    color:var(--ink);font-family:"PingFang SC","Microsoft YaHei","Segoe UI",Roboto,Helvetica,Arial,sans-serif;
    line-height:1.75;font-size:16px;-webkit-font-smoothing:antialiased}
  .wrap{max-width:1080px;margin:0 auto;padding:0 22px}
  header.hero{padding:72px 0 44px;position:relative;overflow:hidden}
  .kicker{letter-spacing:.28em;font-size:12px;color:var(--brand);text-transform:uppercase;font-weight:700}
  h1{font-size:clamp(30px,5vw,52px);line-height:1.12;margin:14px 0 10px;font-weight:800;
    background:var(--grad);-webkit-background-clip:text;background-clip:text;color:transparent}
  .sub{color:var(--muted);font-size:18px;max-width:760px}
  .meta{margin-top:22px;display:flex;gap:10px;flex-wrap:wrap}
  .chip{background:var(--card);border:1px solid var(--line);border-radius:999px;padding:6px 14px;font-size:13px;color:var(--muted)}
  nav.toc{background:var(--bg2);border:1px solid var(--line);border-radius:16px;padding:18px 22px;margin:26px 0 8px}
  nav.toc a{color:var(--brand2);text-decoration:none;font-size:14px;margin-right:18px;display:inline-block;padding:3px 0}
  nav.toc a:hover{color:var(--brand)}
  section{padding:40px 0;border-top:1px solid var(--line)}
  h2{font-size:clamp(22px,3.4vw,32px);margin:0 0 6px;font-weight:800;display:flex;align-items:center;gap:12px}
  h2 .n{font-size:14px;color:var(--brand);border:1px solid var(--line);border-radius:8px;padding:3px 9px;background:var(--card)}
  h3{font-size:20px;margin:26px 0 6px;color:#fff;font-weight:700}
  h4{font-size:16px;margin:18px 0 4px;color:var(--brand)}
  p{color:#cdd6ea}
  .lead{font-size:18px;color:#e9edf6}
  .grid{display:grid;gap:16px}
  .g2{grid-template-columns:1fr 1fr}
  .g3{grid-template-columns:1fr 1fr 1fr}
  @media(max-width:820px){.g2,.g3{grid-template-columns:1fr}}
  .card{background:var(--card);border:1px solid var(--line);border-radius:16px;padding:20px 22px}
  .card.brand{border-color:#2f6a5f;box-shadow:0 0 0 1px #1c3a34 inset}
  .stat{background:var(--bg2);border:1px solid var(--line);border-radius:16px;padding:18px 20px}
  .stat .num{font-size:32px;font-weight:800;background:var(--grad);-webkit-background-clip:text;background-clip:text;color:transparent}
  .stat .lab{color:var(--muted);font-size:13px;margin-top:2px}
  ul{padding-left:20px}li{margin:5px 0;color:#cdd6ea}
  .tldr{background:linear-gradient(180deg,#132033,#0e1626);border:1px solid #274063;border-radius:18px;padding:8px 24px 20px}
  .tldr li{font-size:16px;margin:12px 0}
  table{width:100%;border-collapse:collapse;font-size:14px;margin:14px 0;overflow:hidden;border-radius:12px}
  th,td{border:1px solid var(--line);padding:10px 12px;text-align:left;vertical-align:top}
  th{background:var(--bg2);color:#fff;font-weight:700}
  tr:nth-child(even) td{background:#131a2a}
  .tag{display:inline-block;font-size:11px;padding:2px 8px;border-radius:6px;font-weight:700}
  .t-low{background:#123227;color:var(--ok)} .t-mid{background:#332a12;color:var(--accent)} .t-high{background:#331520;color:var(--danger)}
  .chartbox{background:var(--bg2);border:1px solid var(--line);border-radius:16px;padding:18px;margin:16px 0}
  .chartbox h4{margin:0 0 4px;color:#fff}
  .cap{color:var(--muted);font-size:12.5px;margin-top:8px}
  .src{color:#6f7d9c;font-size:12px}
  .quote{border-left:3px solid var(--brand);padding:6px 0 6px 16px;color:#dfe6f5;font-style:italic;margin:12px 0}
  .pill-row{display:flex;flex-wrap:wrap;gap:8px;margin:10px 0}
  .pill{background:var(--bg2);border:1px solid var(--line);border-radius:999px;padding:5px 12px;font-size:12.5px;color:var(--muted)}
  .timeline{position:relative;margin:24px 0;padding-left:4px}
  .step{position:relative;padding:0 0 6px 40px;margin-bottom:8px}
  .step::before{content:"";position:absolute;left:11px;top:26px;bottom:-8px;width:2px;background:var(--line)}
  .step:last-child::before{display:none}
  .dot{position:absolute;left:0;top:4px;width:24px;height:24px;border-radius:50%;background:var(--grad);
    display:flex;align-items:center;justify-content:center;font-size:12px;font-weight:800;color:#04121e}
  .step h4{margin:0;color:#fff;font-size:17px}
  .step .wk{font-size:12px;color:var(--brand);font-weight:700}
  .fw{display:flex;gap:14px;flex-wrap:wrap;align-items:stretch}
  .school{flex:1;min-width:220px;background:var(--card);border:1px solid var(--line);border-radius:16px;padding:18px}
  .school .ico{font-size:26px}
  .callout{background:linear-gradient(180deg,#101d18,#0d1512);border:1px solid #2f6a5f;border-radius:16px;padding:20px 22px;margin:18px 0}
  .warn{background:#1a1220;border:1px solid #4a2438;border-radius:16px;padding:18px 22px}
  footer{padding:40px 0 60px;color:var(--muted);font-size:13px;border-top:1px solid var(--line)}
  .facegrid{display:grid;grid-template-columns:repeat(4,1fr);gap:12px;margin:14px 0}
  @media(max-width:700px){.facegrid{grid-template-columns:repeat(2,1fr)}}
  .facecard{background:var(--bg2);border:1px solid var(--line);border-radius:14px;padding:12px;text-align:center}
  .facecard svg{width:100%;height:84px}
  .facecard .nm{font-size:12.5px;color:var(--muted);margin-top:6px}
  a.ext{color:var(--brand2)}
</style>
</head>
<body>

<header class="hero">
  <div class="wrap">
    <div class="kicker">Physical AI · Affective Computing · Design System</div>
    <h1>机器人情感表达设计语言<br>研究报告</h1>
    <p class="sub lead">从视觉设计师视角，用科学方法定义并落地一套标准化、品牌化的人形机器人（Humanoid / Embodied AI）表情与动效设计语言——覆盖 HRI 学术方法、经典角色拆解、四大表情流派、硬件媒介权衡、设计系统化与从 0 到 1 的落地路线图。</p>
    <div class="meta">
      <span class="chip">编制日期：2026-07-28</span>
      <span class="chip">方法：20+ 次网络检索 + 定向子研究</span>
      <span class="chip">受众：视觉/交互/工业设计负责人</span>
    </div>
  </div>
</header>

<div class="wrap">
  <nav class="toc">
    <strong style="color:#fff">目录</strong><br>
    <a href="#tldr">TL;DR</a>
    <a href="#key">关键发现</a>
    <a href="#m1">① 调研方法论</a>
    <a href="#m2">② 经典角色</a>
    <a href="#m3">③ 现实案例与四流派</a>
    <a href="#m4">④ 硬件与媒介</a>
    <a href="#m5">⑤ 设计系统化</a>
    <a href="#m6">⑥ 主张与路线图</a>
    <a href="#caveat">注意事项</a>
  </nav>
</div>

<!-- TL;DR -->
<div class="wrap"><section id="tldr" style="border-top:none">
  <h2><span class="n">TL;DR</span>三句话结论</h2>
  <div class="tldr"><ol>
    <li><b>采取"经典提炼为骨、媒介创新为翼"的双轨策略。</b> 先从 WALL-E、R2-D2、Baymax 等被票房与民调验证的角色中提炼可迁移的设计原语（大眼睛、婴儿图式、极简少嘴、缓动动效、非语言拟声），再用屏幕 / LED / 机械 / 肢体四大媒介大胆重组，创造品牌专属表达——而非二选一。</li>
    <li><b>科学方法已经成熟且可直接套用。</b> 用 Ekman 六大基本情绪 + FACS 面部动作单元把"情感"翻译成可执行参数；用恐怖谷曲线定位拟人度、规避风险；用 Godspeed 五维问卷与情绪识别准确率测试量化验证。这是一条"语义定义 → 风险规避 → 原型 → 量化测试 → 迭代"的闭环。</li>
    <li><b>落地要像做品牌设计系统（Design System）。</b> 建立情绪状态机、表情组件库与动效 token、缓动曲线与时间参数规范，绑定品牌人格，并引入 LLM 驱动的实时情感生成层——设计师定约束，AI 做运行时组合。</li>
  </ol></div>
</section></div>

<!-- KEY FINDINGS -->
<div class="wrap"><section id="key">
  <h2><span class="n">摘要</span>关键发现（速览）</h2>
  <div class="grid g3">
    <div class="stat"><div class="num">$380亿</div><div class="lab">Goldman Sachs 预测人形机器人 2035 年市场规模（较原预测 60 亿美元上调六倍多，出货约 140 万台）</div></div>
    <div class="stat"><div class="num">~96%</div><div class="lab">happiness / surprise 情绪识别准确率上限（系统综述）；anger / disgust 最低约 91.7% / 93.7%</div></div>
    <div class="stat"><div class="num">30.5%</div><div class="lab">低自由度真实机器人 Reachy Mini 上的<b>精确</b>情绪标签识别率——远低于算法离线水平</div></div>
    <div class="stat"><div class="num">840 毫秒</div><div class="lab">哥伦比亚 Emo 机器人可提前预测人的微笑并同步"共表情"</div></div>
    <div class="stat"><div class="num">27 / 61</div><div class="lab">Ameca 面部自由度 / 总自由度；价格约 $10 万–$25 万，定位研究与展示</div></div>
    <div class="stat"><div class="num">71%</div><div class="lab">R2-D2 在星战角色好感度民调中的得分（仅次于莱娅 73%）</div></div>
  </div>
  <p style="margin-top:20px">下文将逐一展开六大方向。核心张力贯穿全文：<b>拟人度越高，情感表现力越强，但成本与"恐怖谷"风险也越高</b>——设计的艺术在于为特定用途选对"停靠点"。</p>
</section></div>

<!-- M1 METHODOLOGY -->
<div class="wrap"><section id="m1">
  <h2><span class="n">01</span>调研分析方法论：如何科学地定义与验证</h2>
  <p class="lead">情感表达设计不能凭直觉。HRI（Human-Robot Interaction）与情感计算领域已积累了一套可操作的科学工具链，把主观情绪转化为可复现、可测量的设计参数。</p>

  <h3>心理学地基：Ekman 六情绪 + FACS 动作单元</h3>
  <p>心理学家 Paul Ekman 提出六大<b>基本情绪</b>——快乐、悲伤、恐惧、愤怒、厌恶、惊讶，具有跨文化普适性。他与 Friesen 于 1978 年建立<b>面部动作编码系统 FACS</b>，把面部肌肉活动编码为约 46 个<b>动作单元（Action Unit, AU）</b>，不同 AU 组合形成情绪。例如 AU6（脸颊上提，Cheek Raiser）+ AU12（嘴角上扬，Lip Corner Puller）= 快乐/情感。FACS 已被广泛用于 HRI 机器人表情建模（Furhat、EMYS 等）。</p>
  <div class="callout">
    <b>对视觉设计师的意义：</b> FACS 相当于表情的"字体规格表 / 设计 token 表"。它让你把"开心"这种主观词，转化为客观、可复现的几何位移（哪条眉弧上扬几度、嘴角位移几毫米、眼睑开合比例多少），从而在屏幕像素、舵机角度或 LED 亮度之间无损翻译同一套情绪语义。
  </div>

  <h3>恐怖谷：拟人度与好感度的非线性关系</h3>
  <p>日本机器人学家<b>森政弘（Masahiro Mori）1970 年</b>提出恐怖谷（Uncanny Valley）假说：当拟人度接近但未完全达到真人时，好感度骤降至负值，形成一道"谷"。此后大量实证研究复现了 U 型曲线；2021 年《ACM Transactions on Human-Robot Interaction》发表了该领域<b>首个元分析</b>（Ho & MacDorman），为方法论奠基。Mori 原始图形的拟合优度约 R²=0.640。Bartneck 等 2007 年用真人与机器人图片实验，提出<b>"恐怖悬崖"（uncanny cliff）</b>替代模型（二次曲线拟合 R²≈0.474），并发现耐人寻味的一点：即便真人照片，其好感度也不如玩具机器人高。</p>

  <div class="chartbox">
    <h4>图 1 · 恐怖谷曲线（示意）与经典机器人角色的"安全停靠点"</h4>
    <canvas id="uncannyChart" height="150"></canvas>
    <p class="cap">横轴为拟人度（0=纯机械，100=真人），纵轴为情感好感度。WALL-E / R2-D2 / Baymax 等停留在"卡通/玩具"第一好感峰；Ameca 等超拟真面部逼近谷底。曲线为基于 Mori(1970) 及 Bartneck(2007) 描述的示意重建，非精确数据点。</p>
  </div>
  <div class="callout"><b>设计启示：</b> 抽象、卡通化、明确"非人"的设计（WALL-E 的望远镜眼睛、R2-D2 的圆桶）可安全停在<b>第一个好感峰值</b>，避免掉入谷底。这是"少即是多"在机器人脸上的科学依据。</div>

  <h3>量化评估工具</h3>
  <div class="grid g2">
    <div class="card"><h4>Godspeed 问卷（Bartneck 2009）</h4>
      <p>社会机器人领域被引用最多的标准量表之一，用 5 点语义差异量表测五维：</p>
      <ul>
        <li><b>拟人性</b> Anthropomorphism</li>
        <li><b>生命感</b> Animacy</li>
        <li><b>喜爱度</b> Likeability</li>
        <li><b>感知智能</b> Perceived Intelligence</li>
        <li><b>感知安全</b> Perceived Safety</li>
      </ul>
      <p class="src">补充工具：ROSAS 机器人社会属性量表、感知能力量表 PCS。</p>
    </div>
    <div class="card"><h4>情绪识别准确率测试</h4>
      <p>让被试观看机器人表情并标注情绪，计算命中率。这是验证"表情设计有没有说清楚"的黄金标准。</p>
      <p>系统综述（Applied Soft Computing，检索 1990–2023 共 35 项研究）显示：<b>happiness 与 surprise 平均识别准确率最高（96.42% 和 96.32%）</b>，而 <b>anger 与 disgust 最低（91.68% 和 93.71%）</b>，fear 与 sadness 均值约 93.87%。</p>
      <div class="quote">但请注意：这些是"算法识别人脸"的成绩。当换成"低自由度真实机器人表达情绪、由人来识别"时，成绩会大幅下降。</div>
    </div>
  </div>

  <div class="chartbox">
    <h4>图 2 · 两类识别率对比：算法识别人脸 vs. 人识别机器人表情</h4>
    <canvas id="recogChart" height="160"></canvas>
    <p class="cap">蓝：算法识别人脸的平均准确率（系统综述）。橙：人识别低自由度机器人 Reachy Mini 表情的<b>精确标签</b>准确率（arXiv:2605.12786，N=100）——总体仅 30.5%，anger 最高 81.8%，disgust 为 0%。若放宽到<b>效价（valence）</b>维度识别可达 65.9%。</p>
  </div>
  <div class="callout"><b>关键洞察：</b> 对真实机器人，"让人识别出<b>大致情绪方向</b>（正向/负向、兴奋/平静）"比"精确分类到某一种情绪"更现实、更值得优先设计。这直接影响你的目标验收阈值（见路线图阶段三）。</div>

  <h3>方法论框架图</h3>
  <div class="chartbox"><div style="display:flex;gap:10px;flex-wrap:wrap;align-items:center;justify-content:center;font-size:13.5px">
    <span class="pill" style="background:#132033">① 语义定义<br>Ekman + FACS</span>→
    <span class="pill" style="background:#132033">② 风险定位<br>恐怖谷曲线</span>→
    <span class="pill" style="background:#132033">③ 原型<br>动画管线</span>→
    <span class="pill" style="background:#132033">④ 量化测试<br>Godspeed+识别率</span>→
    <span class="pill" style="background:#132033">⑤ 迭代/系统化</span>
  </div></div>
</section></div>

<!-- M2 CLASSIC CHARACTERS -->
<div class="wrap"><section id="m2">
  <h2><span class="n">02</span>经典角色的设计语言：为什么我们爱它们</h2>
  <p class="lead">大众最喜爱的机器人角色几乎共享一套"配方"。拆解它们，就是拆解被数十年文化沉淀验证过的用户预期。</p>

  <div class="facegrid">
    <div class="facecard">
      <svg viewBox="0 0 120 84"><rect x="20" y="24" width="80" height="42" rx="8" fill="#2a3350"/><circle cx="42" cy="45" r="15" fill="#0b0f1a" stroke="#5ee0c8" stroke-width="2"/><circle cx="46" cy="42" r="5" fill="#7aa2ff"/><circle cx="78" cy="45" r="15" fill="#0b0f1a" stroke="#5ee0c8" stroke-width="2"/><circle cx="74" cy="42" r="5" fill="#7aa2ff"/></svg>
      <div class="nm">WALL-E · 望远镜眼睛 · 无嘴</div>
    </div>
    <div class="facecard">
      <svg viewBox="0 0 120 84"><rect x="42" y="14" width="36" height="20" rx="4" fill="#8fa0c0"/><rect x="38" y="34" width="44" height="40" rx="4" fill="#c7d0e3"/><circle cx="60" cy="46" r="9" fill="#0b0f1a" stroke="#5ee0c8" stroke-width="2"/><circle cx="60" cy="46" r="3.5" fill="#5ee0c8"/><rect x="46" y="60" width="6" height="6" fill="#7aa2ff"/><rect x="68" y="60" width="6" height="6" fill="#ffb454"/></svg>
      <div class="nm">R2-D2 · 圆桶 · 非语言拟声</div>
    </div>
    <div class="facecard">
      <svg viewBox="0 0 120 84"><ellipse cx="60" cy="44" rx="40" ry="34" fill="#f2f3f7"/><circle cx="46" cy="44" r="3.5" fill="#1a1a1a"/><circle cx="74" cy="44" r="3.5" fill="#1a1a1a"/><line x1="49" y1="44" x2="71" y2="44" stroke="#1a1a1a" stroke-width="2"/></svg>
      <div class="nm">Baymax · 婴儿图式 · 铃铛脸</div>
    </div>
    <div class="facecard">
      <svg viewBox="0 0 120 84"><rect x="30" y="20" width="60" height="46" rx="10" fill="#1a2233"/><path d="M40 46 q8 -14 16 0" stroke="#5ee0c8" stroke-width="4" fill="none" stroke-linecap="round"/><path d="M64 46 q8 -14 16 0" stroke="#5ee0c8" stroke-width="4" fill="none" stroke-linecap="round"/></svg>
      <div class="nm">Cozmo/Vector · 皮克斯式屏幕眼</div>
    </div>
  </div>

  <div class="grid g2">
    <div class="card">
      <h4>WALL-E & EVE（皮克斯 2008，奥斯卡最佳动画长片）</h4>
      <p>导演 Andrew Stanton 的眼睛灵感来自棒球赛上借来的一副<b>双筒望远镜</b>，他"错过了整整一局"只顾把望远镜摆弄出喜怒哀乐。他定下"WALL-E 是盒子、EVE 是蛋"，<b>不要鼻子和嘴</b>，只靠一对望远镜眼睛——联想到默片喜剧演员 Buster Keaton——并加<b>变焦镜头</b>增加同情感与童真。设计师 Jay Shuster 说，直到镜头簇、光圈与眨眼快门机构确定，WALL-E 才"活了过来"。</p>
      <div class="pill-row"><span class="pill">极简</span><span class="pill">大眼睛</span><span class="pill">无嘴</span><span class="pill">Ben Burtt 电子拟声</span></div>
    </div>
    <div class="card">
      <h4>R2-D2 & BB-8（星球大战）</h4>
      <p>被 NPR 称为"银河系最受爱戴的机器人"，只靠哔哔声与身体动作表达情感，却让片场演员都"融化"。<b>民调数据：</b>某星战角色好感度调查中 R2-D2 达 <b>71%</b>，仅次于莱娅公主 73%、与卢克/丘巴卡/尤达 72% 几乎并肩；新角色 BB-8 为 39%。</p>
      <div class="pill-row"><span class="pill">非语言拟声</span><span class="pill">圆润几何</span><span class="pill">忠诚可爱</span><span class="pill">沉默胜千言</span></div>
    </div>
    <div class="card">
      <h4>Baymax 大白（超能陆战队 2014）</h4>
      <p>灵感来自<b>卡内基梅隆大学软体机器人实验室</b>的充气机械臂（Chris Atkeson 团队）；导演 Don Hall 一句"你说'充气'就够了"敲定方案。脸部灵感来自日本神社的<b>铃铛（suzu bell）</b>——两点一线的极简脸；动作参考<b>婴儿企鹅</b>的摇摆步态；充气乙烯基外壳象征"可拥抱（huggable）"。视觉开发艺术家称其核心概念就是"newborn（新生儿）般的天真"。</p>
      <div class="pill-row"><span class="pill">婴儿图式</span><span class="pill">柔软无威胁</span><span class="pill">极简脸</span><span class="pill">憨态动效</span></div>
    </div>
    <div class="card brand">
      <h4>方法论桥梁：动画 12 原则 → 机器人</h4>
      <p>迪士尼动画师 Ollie Johnston 与 Frank Thomas 在 1981 年《The Illusion of Life》中总结<b>12 条动画原则</b>（挤压拉伸、预备动作、跟随与重叠、缓入缓出、弧线运动、夸张、吸引力等）。2012 年 ACM/IEEE HRI 会议论文《The Illusion of Robotic Life》把这些原则应用到 EMYS 机器人表情，<b>证明能显著提升人对机器人情绪的理解</b>——这是连接"动画艺术"与"机器人工程"的关键方法论。</p>
    </div>
  </div>

  <div class="callout">
    <b>共同"配方"：</b> 让人喜爱与信任的经典机器人 = <b>大眼睛 + 婴儿图式 + 极简（少嘴或无嘴）+ 缓动动效 + 非语言拟声 + 明确"可爱的非人"定位</b>。这套配方恰好与科学证据高度吻合：婴儿图式激活大脑奖赏系统（见 §4）、"非人"定位规避恐怖谷、12 原则赋予"生命的错觉"。
  </div>
</section></div>

<!-- M3 REAL CASES + 4 SCHOOLS -->
<div class="wrap"><section id="m3">
  <h2><span class="n">03</span>现实案例与四大表情流派</h2>
  <p class="lead">现实产品把上述原语落到了硬件上，分化出四条技术路线。每条都有明确的成本、可靠性与用户接受度权衡。</p>

  <div class="fw">
    <div class="school"><div class="ico">🖥️</div><h4>① 屏幕式眼睛<br><span class="src">Screen-based face</span></h4>
      <p>OLED/LCD 显示卡通化眼睛。<b>Cozmo/Vector、Jibo、Aibo、Astro</b>。</p>
      <div class="pill-row"><span class="tag t-low">成本低</span><span class="tag t-low">易迭代</span></div></div>
    <div class="school"><div class="ico">🤖</div><h4>② 机械拟真面部<br><span class="src">Animatronic face</span></h4>
      <p>舵机 + 硅胶皮肤。<b>Ameca、Emo</b>。表现力最强。</p>
      <div class="pill-row"><span class="tag t-high">成本高</span><span class="tag t-high">恐怖谷风险</span></div></div>
    <div class="school"><div class="ico">💡</div><h4>③ 抽象光效<br><span class="src">Light / LED matrix</span></h4>
      <p>灯带、点阵、彩色眼灯。<b>Pepper 眼灯</b>。最省成本。</p>
      <div class="pill-row"><span class="tag t-low">成本最低</span><span class="tag t-mid">语义模糊</span></div></div>
    <div class="school"><div class="ico">🦿</div><h4>④ 纯肢体动效<br><span class="src">Body language</span></h4>
      <p>无脸，靠全身运动学。<b>Atlas、1X Neo、BDX</b>。</p>
      <div class="pill-row"><span class="tag t-mid">适合工作型</span><span class="tag t-low">低承诺</span></div></div>
  </div>

  <h3>皮克斯式屏幕眼睛：把动画电影管线搬进机器人</h3>
  <p>Anki 的 <b>Cozmo</b>（2016，售价 $179.99）与 <b>Vector</b> 由<b>前皮克斯动画师 Carlos Baena</b>（曾在皮克斯参与 WALL-E、海底总动员等 10 年）主导表情设计。团队用 <b>定制版 Autodesk Maya 3D 软件</b>直接命令机器人如何移动，"按下播放键就能看到桌上的机器人做出同样动作"；通过专有的 <b>"emotion engine"（情绪引擎）</b>驱动反应。Baena 说服团队<b>简化眼睛（无眉毛、无瞳孔）</b>并让其尽可能有表现力。团队做了 <b>40 多个物理原型</b>（包括一个独眼版本）才定稿。</p>
  <div class="warn" style="margin:14px 0">
    <b>商业警示：</b> Anki 融资 <b>2 亿美元</b>后仍于 2019 年 4 月破产；Jibo（2017 年《时代》年度最佳发明，售价约 $899）因价格高、功能有限于次年停运。IEEE Spectrum 总结 Jibo/Kuri/Anki 的教训：<b>社会机器人缺乏长期使用价值、期望管理失败</b>——表情设计再好，也救不了薄弱的商业模式与产品定位。
  </div>

  <h3>机械拟真面部：表现力天花板，也是恐怖谷雷区</h3>
  <p>Engineered Arts 的 <b>Ameca</b> 有 <b>27 个面部自由度、61 个总自由度</b>，基于 Mesmer 技术，价格约 <b>$10 万–$25 万</b>，定位为 HRI 研究与展示平台（不干活）。耐人寻味的是：即便设计团队刻意把它做成"明显是机器人"的灰色脸，记者仍觉得"令人不安"。哥伦比亚大学创意机器实验室（Hod Lipson 团队，Yuhang Hu 一作）的 <b>Emo</b> 有 <b>26 个执行器</b>、硅胶皮肤、瞳孔内置摄像头，能<b>提前约 840 毫秒预测人的微笑</b>并同步"共表情"，2024 年 3 月发表于《Science Robotics》。</p>

  <h3>肢体 + 微光效混合：迪士尼 BDX 的启示</h3>
  <p>Disney Research 的 <b>BDX droid</b>（灵感来自《星战绝地：陨落的武士团》BD-1）是"肢体 + 微光效"混合流派：<b>14 个执行器</b>（头颈 4 + 每腿 5），用<b>强化学习</b>在仿真中训练"会走、会平衡、会 emote"的角色，2023 年在迪士尼乐园"银河边缘"首次亮相。表情来自头部、两根会动的天线、腿部姿态、灯光与声音。Imagineering 高管 Kyle Laughlin 的理念是<b>"故事优先，技术服务于故事"</b>——"真正让它们与众不同的是那一点点鲜活的个性，能让人微笑"。据报道 Disney 每台仅执行器成本约 $7,500。</p>
  <p>相反，1X 的 <b>Neo</b>（2025，$20,000 或 $499/月订阅）用 <b>KnitSuit 针织外套</b>软化外形，脸部只是一块黑色塑料面板配深陷的眼睛，刻意"低表情、融入家居"——它的设计哲学是"不主导你的空间，而是消隐其中直到你需要"。</p>

  <h3>四大流派量化对比</h3>
  <table>
    <tr><th>维度</th><th>① 屏幕式眼睛</th><th>② 机械拟真面部</th><th>③ 抽象光效</th><th>④ 纯肢体动效</th></tr>
    <tr><td>代表产品</td><td>Cozmo/Vector、Aibo、Astro、Jibo</td><td>Ameca、Emo</td><td>Pepper 眼灯、LED 点阵</td><td>Atlas、1X Neo、BDX</td></tr>
    <tr><td>面部执行器</td><td>0（眼睛是像素）</td><td>26–27 个</td><td>0（像素/LED）</td><td>0（无脸）</td></tr>
    <tr><td>成本量级</td><td><span class="tag t-low">$1,000–$3,000 消费级</span></td><td><span class="tag t-high">$10 万–$25 万+</span></td><td><span class="tag t-low">数十美元元件</span></td><td>由运动而非表情决定</td></tr>
    <tr><td>情感表现力</td><td>中高（卡通夸张）</td><td>最高</td><td>低（需配合动效/声音）</td><td>中（整体姿态）</td></tr>
    <tr><td>恐怖谷风险</td><td><span class="tag t-low">低</span></td><td><span class="tag t-high">高</span></td><td><span class="tag t-low">极低</span></td><td><span class="tag t-low">低</span></td></tr>
    <tr><td>量产/可靠性</td><td>好</td><td>差</td><td>好</td><td>取决于本体</td></tr>
    <tr><td>最适用途</td><td>家用陪伴 / 消费</td><td>展示 / 研究 / 影视</td><td>提示 / 辅助信号</td><td>工作型人形</td></tr>
  </table>

  <div class="chartbox">
    <h4>图 3 · 四流派雷达图：表现力 · 成本控制 · 可靠性 · 恐怖谷安全 · 迭代速度</h4>
    <canvas id="radarChart" height="230"></canvas>
    <p class="cap">评分为综合定性研判（5 分制，越高越好；"成本控制"与"恐怖谷安全"均为正向指标）。屏幕式眼睛在成本、安全、迭代上全面领先，是当下商业主流选择。</p>
  </div>

  <div class="callout">
    <b>一项关键实证（Mishra & Skantze 2024，N=305）：</b> 完整动画脸——<b>无论更像人还是更机械</b>——其情绪表达能力都可媲美真人；但当只显示<b>眼部区域</b>时识别率下降，此时更拟人的特征反而有帮助。这对"只有一对屏幕眼睛"的流派（Vector/Astro）是个提醒：<b>眼睛能表达的情绪有天花板，需靠动效、姿态与声音补足。</b>
  </div>

  <h3>商业与依恋数据</h3>
  <table>
    <tr><th>产品</th><th>价格</th><th>表情媒介</th><th>规模/结局</th></tr>
    <tr><td>Sony Aibo (ERS-1000, 2018)</td><td>约 ¥198,000（约 $1,750）</td><td>22 自由度 + <b>两块 OLED 屏做眼睛</b></td><td>上市半年售出约 2 万台；2026 年宣布日本市场售完即停</td></tr>
    <tr><td>SoftBank Pepper</td><td>约 ¥198,000（另有月费）</td><td>胸前平板 + 肢体 + 彩色眼灯</td><td>共生产约 <b>27,000 台</b>；2020 年停产</td></tr>
    <tr><td>Amazon Astro</td><td>$999–$1,599</td><td>10.1" 屏显<b>会变形的"数字眼睛"</b></td><td>商用版 2024 年 9 月停用；消费版仍限量</td></tr>
    <tr><td>GROOVE X LOVOT</td><td>陪伴型订阅</td><td>OLED 眼 + 触感毛绒 + 拟声</td><td>研究：接触 15 分钟降皮质醇；主人催产素基线更高</td></tr>
  </table>
  <p class="src">注：Sony 未公布 Aibo 终身总销量；Pepper "生产约 27,000 台"来自路透社，"售出"数字较软（约 1.7 万–2.1 万台）；Ameca 价格为报价制，$25 万为最常被引用的"典型整机"数字，区间 $10 万–$50 万。</p>
</section></div>

<!-- M4 HARDWARE -->
<div class="wrap"><section id="m4">
  <h2><span class="n">04</span>硬件与媒介载体：成本 · 可靠性 · 量产权衡</h2>

  <div class="grid g2">
    <div class="card"><h4>屏幕（OLED / 圆形屏 / 柔性屏）</h4>
      <p>成本低、可编程性极高、可秒级迭代表情，是 Cozmo/Vector/Aibo/Astro 的选择。Sony Aibo 用两块独立 OLED 做眼睛，实现眨眼、闭眼等"细腻表情"，刻意<b>放弃老式"面罩（visor）"设计</b>以改善 HRI。缺点：仍有"屏幕感"，削弱实体临场感。</p></div>
    <div class="card"><h4>LED 矩阵与灯光</h4>
      <p>最省成本、功耗低、抗污损、无机械磨损。适合做<b>状态提示与情绪基调</b>（如呼吸灯、彩色眼灯），但离散情绪语义模糊，必须与动效、声音叠加才能"说清楚"。</p></div>
    <div class="card"><h4>机械结构与仿生皮肤</h4>
      <p>舵机驱动面部（Ameca 27 个、Emo 26 个执行器）配硅胶电子皮肤。表现力天花板最高，但成本、可靠性、量产性全面最差，且恐怖谷风险最高。适合展示、研究、影视，而非量产消费品。</p></div>
    <div class="card"><h4>材料语言</h4>
      <p><b>硅胶皮肤</b>（拟真，但踩谷）；<b>半透明外壳</b>（露出内部光效，科技感）；<b>织物 / 针织</b>（1X Neo 的 KnitSuit，柔化威胁感、暗示"安全可触")。材料本身就是情感信号。</p></div>
  </div>

  <h3>婴儿图式（Baby Schema）：大眼睛大头的科学依据</h3>
  <p>生态学家 <b>Konrad Lorenz</b> 提出"婴儿图式（Kindchenschema）"：<b>大头、圆脸、大眼睛、高额头、圆脸颊、小鼻小嘴、短粗四肢</b>会被感知为可爱并激发照护动机，具进化上的适应意义。</p>
  <div class="grid g2" style="margin-top:12px">
    <div class="card brand"><h4>神经科学证据</h4><p>2009 年发表于《PNAS》的研究用功能性磁共振（fMRI）证明：婴儿图式激活<b>伏隔核（nucleus accumbens）</b>——大脑奖赏系统的关键结构，介导奖赏处理与趋近动机。也就是说，"可爱"是可以被神经影像测量的奖赏反应。</p></div>
    <div class="card brand"><h4>机器人应用证据</h4><p>2023 年 Chen & Jia 的两项实验（含 100 张真实机器人脸、98 份有效问卷）直接验证：<b>婴儿图式效应能同时提升社会机器人脸的"可爱度"与"可信度"</b>。这为"大眼睛 + 大头"设计原语提供了直接的机器人语境证据。</p></div>
  </div>

  <h3>多模态配合</h3>
  <p>WALL-E 与 R2-D2 都靠"眼睛/光效 + 肢体 + 拟声"三模态叠加来讲情绪。研究表明<b>多模态一致性</b>（如 LOVOT 的表情 + 发声同步）能增强信任；反之，<b>模糊的拟声或过强的 LLM 语音反而降低接受度</b>。设计时应把声音设计（sound design）与表情、动效当作一个整体来编排——而非事后配音。</p>
</section></div>

<!-- M5 DESIGN SYSTEM -->
<div class="wrap"><section id="m5">
  <h2><span class="n">05</span>设计语言系统化与品牌化落地</h2>
  <p class="lead">要像建立品牌 VI / 设计系统那样建立机器人情感语言——让它可复用、可治理、可跨产品线一致。</p>

  <div class="grid g2">
    <div class="card"><h4>① 情绪状态机 Emotion State Machine</h4>
      <p>定义有限的核心情绪状态（建议 = Ekman 六情绪 + 中性 + 若干<b>品牌特色状态</b>如"好奇""调皮""专注"），以及状态间的<b>转移规则与触发条件</b>。Anki 的 emotion engine、Pepper 的情绪地图都是范例。状态机让机器人的情绪"连贯而非跳变"。</p></div>
    <div class="card"><h4>② 表情组件库 + 动效 Token</h4>
      <p>把每个情绪拆成可复用的<b>表情组件</b>（眼形、眉弧、光色、身体姿态）与<b>动效 token</b>（时长、缓动曲线、幅度）。参考 <b>Google Material Design</b> 的动效规范（标准缓动曲线 + 持续时间分级）与迪士尼 12 原则（缓入缓出、预备、跟随重叠）。所有参数写成规范文档。</p></div>
    <div class="card"><h4>③ 品牌人格绑定 Brand Personality</h4>
      <p>像苹果、迪士尼幻想工程那样：<b>先定义品牌人格</b>（"温暖可靠的管家" vs "调皮好奇的伙伴"），再让表情动效的<b>节奏、幅度、色彩</b>服务于该人格。同一个"开心"，管家型是含蓄点头，伙伴型是雀跃弹跳。</p></div>
    <div class="card brand"><h4>④ AI 生成式表情（最新前沿）</h4>
      <p>Furhat Robotics 与马普所（Mishra、Skantze 等）2023 年在《Frontiers in Robotics and AI》发表研究：用 <b>GPT-3.5 实时预测</b>对话中该表达的情绪并驱动表情。47 人被试研究显示，一致的模型驱动表情让机器人被感知为<b>更像人、更恰当、印象更正面</b>，参与者在配套游戏中表现也更好。2025 年开源库 <b>"Expressive Furhat"</b> 进一步把 LLM 实时表情产品化。</p>
      <p><b>范式：</b> 设计师定义"表情组件库 + 品牌约束"，LLM 在运行时实时组合调用——把"手工编排每个表情"升级为"定规则、AI 填充"。</p></div>
  </div>

  <div class="chartbox">
    <h4>图 4 · 情感表达设计系统分层架构</h4>
    <div style="display:grid;gap:8px;font-size:13.5px">
      <div class="pill" style="background:#0f1d17;border-color:#2f6a5f;text-align:center;padding:10px">🎭 品牌人格层 Brand Personality（"我是谁"：语气、节奏、色彩基调）</div>
      <div class="pill" style="background:#131f33;border-color:#274063;text-align:center;padding:10px">🧠 情绪状态机 State Machine（状态集 + 转移规则 + 触发条件）</div>
      <div class="pill" style="background:#1b1830;border-color:#3a2f5e;text-align:center;padding:10px">🧩 表情组件库 + 动效 Token（眼形/眉弧/光色/姿态 · 时长/缓动/幅度）</div>
      <div class="pill" style="background:#241826;border-color:#4a2438;text-align:center;padding:10px">⚙️ 媒介渲染层 Media（屏幕像素 / 舵机角度 / LED 亮度 / 关节运动学）</div>
      <div class="pill" style="background:#0d1b26;border-color:#1f4a5e;text-align:center;padding:10px">🤖 LLM 实时生成层（运行时按上下文组合调用，受上层约束）</div>
    </div>
    <p class="cap">上层定义"意图与约束"，下层负责"翻译与渲染"。FACS 语义在各媒介间保证同一情绪无损迁移。</p>
  </div>
</section></div>

<!-- M6 THESIS + ROADMAP -->
<div class="wrap"><section id="m6">
  <h2><span class="n">06</span>作者的设计主张与落地路线图</h2>

  <h3>核心主张：双轨策略 —— "经典提炼为骨，媒介创新为翼"</h3>
  <div class="grid g2">
    <div class="card"><h4>为什么要"提炼经典"</h4>
      <p>经典角色的设计语言已被<b>票房、民调、数十年文化沉淀</b>反复验证（R2-D2 好感度 71%、WALL-E 奥斯卡、Baymax 全球叫座）。其共性——大眼睛、婴儿图式、极简少嘴、缓动动效、非语言拟声、可爱的非人定位——恰好与<b>科学证据</b>高度吻合（婴儿图式激活奖赏系统、非人定位规避恐怖谷、12 原则赋予生命错觉）。抛弃这些，等于抛弃免费且经过验证的用户预期。</p></div>
    <div class="card"><h4>为什么不能"照搬"</h4>
      <p>直接复制会有三重风险：<b>同质化</b>（人人都做 WALL-E 眼睛）、<b>媒介错配</b>（人形机器人不是桌面小车，尺度与交互距离完全不同）、<b>错失新媒介</b>（柔性屏、LED、LLM 实时生成带来的差异化机会）。因此要在"被验证的原语"之上，用当下媒介<b>大胆重组</b>，创造品牌专属表达。</p></div>
  </div>

  <div class="callout"><b>决策规则（按用途选停靠点）：</b>
    <ul style="margin-top:8px">
      <li><b>家用陪伴</b>（LOVOT / Aibo 定位）→ 优先<b>婴儿图式 + 屏幕眼睛 + 柔软材质</b>，停在第一好感峰。</li>
      <li><b>工作型人形</b>（Atlas / Neo 定位）→ 优先<b>肢体动效 + 极简光效</b>，刻意低表情以避免过度承诺（over-promise）引发失望。</li>
      <li><b>展示 / 研究 / 影视</b>（Ameca 定位）→ 才考虑<b>机械拟真面部</b>，并接受高成本与恐怖谷风险。</li>
    </ul>
  </div>

  <h3>从 0 到 1 的分阶段路线图</h3>
  <div class="timeline">
    <div class="step"><div class="dot">1</div>
      <span class="wk">阶段一 · 约 4–8 周</span>
      <h4>调研 Research：建立科学地基</h4>
      <p>做竞品与经典角色的设计语言拆解（本报告即样本）；用 FACS + Ekman 定义目标情绪集；用恐怖谷曲线定位产品的<b>拟人度目标区间</b>；确定媒介载体与品牌人格。<br><b>产出：</b> 情绪语义字典、竞品图谱、拟人度定位文档。</p></div>
    <div class="step"><div class="dot">2</div>
      <span class="wk">阶段二 · 约 6–12 周</span>
      <h4>原型 Prototype：动画管线快速试错</h4>
      <p>学 Anki，用 <b>Maya / After Effects</b> 等动画工具做表情原型（电影级管线）；建立最小情绪状态机；做 3–5 个媒介方案对比原型（屏幕眼睛 vs 光效 vs 混合）。<br><b>产出：</b> 可交互表情原型、动效 token 初稿。</p></div>
    <div class="step"><div class="dot">3</div>
      <span class="wk">阶段三 · 约 4–8 周</span>
      <h4>测试 Test：量化验证</h4>
      <p>用 <b>Godspeed 问卷</b>测五维；用<b>情绪识别率测试</b>验证每个表情能否被正确识别；做恐怖谷风险评估。<br><b>验收阈值：</b> 效价（valence）识别率 &gt; 65%（对标 Reachy Mini 研究的现实上限）、喜爱度显著优于对照；核心情绪精确识别率尽量高。<br><b>改变决策的基准：</b> 若喜爱度或识别率不达标 → 回到阶段二。</p></div>
    <div class="step"><div class="dot">4</div>
      <span class="wk">阶段四 · 约 8–12 周</span>
      <h4>系统化 Systematize：固化为 Design System</h4>
      <p>把验证过的表情固化为设计系统：表情组件库、动效 token 规范、缓动曲线与时间参数文档、情绪状态机代码、跨产品线一致性规则。<br><b>产出：</b> 机器人情感表达设计系统文档 + 代码库。</p></div>
    <div class="step"><div class="dot">5</div>
      <span class="wk">阶段五 · 持续</span>
      <h4>品牌化 Brand & Scale：绑定人格 + AI 生成</h4>
      <p>绑定品牌人格，定义"表情语气"；引入 <b>LLM 实时情感生成层</b>（设计师定约束、AI 做组合）；建立跨产品线一致性治理与版本迭代机制。<br><b>产出：</b> 品牌化情感表达语言 + LLM 集成 + 治理流程。</p></div>
  </div>

  <div class="chartbox">
    <h4>图 5 · 落地路线图甘特概览（相对周数）</h4>
    <canvas id="ganttChart" height="150"></canvas>
    <p class="cap">阶段可部分重叠；系统化与品牌化后进入持续迭代。总周期约 22–40 周达到可量产的 v1.0 设计语言。</p>
  </div>
</section></div>

<!-- CAVEATS -->
<div class="wrap"><section id="caveat">
  <h2><span class="n">⚠︎</span>注意事项与证据可靠性</h2>
  <div class="warn">
    <ul>
      <li><b>识别率数据的口径差异：</b> 约 96% 的高准确率多来自"<b>算法识别人脸</b>"的研究，而机器人"<b>表达</b>"情绪供人识别的准确率通常低得多（Reachy Mini 精确标签仅 30.5%）。制定验收标准时务必区分这两类。</li>
      <li><b>恐怖谷理论仍有争议：</b> U 型曲线虽被广泛复现，但其理论机制未有共识；Bartneck 等提出的"恐怖悬崖"等替代模型说明单一曲线可能过度简化。把它当"设计罗盘"而非"精确公式"。</li>
      <li><b>市场规模是激进情景：</b> Goldman Sachs 的 $380 亿（2035）为上调后的可寻址市场预测，含"AI 与操控瓶颈被突破""通用人形可行性成立"等重大前提，Goldman 自身也标注了这些警示——非既定事实。2025 年公开估算多在个位数十亿美元区间。</li>
      <li><b>Ameca 价格为报价制：</b> $10 万（头部）到 $25 万（典型整机）到约 $50 万（定制）区间跨度大，均为第三方数据；官方从未公布标价。网络上流传的"会走路的 Gen 3 Ameca"部分为 AI 生成/存疑内容，官方规格仍为固定式。</li>
      <li><b>BDX 执行器数：</b> "14 个"由两个独立来源（头颈 4 + 每腿 5）交叉印证；"每台约 $7,500 执行器成本"为二手报道（Hackster），按量级参考而非官方。</li>
      <li><b>LLM 实时表情仍处早期：</b> 存在幻觉、延迟与"过强语音降低接受度"的风险；上线前需人机测试与安全护栏（guardrails）。</li>
    </ul>
  </div>
</section></div>

<footer>
  <div class="wrap">
    <p><b>方法说明：</b> 本报告基于 20+ 次结构化网络检索与一次定向子研究编制，主要证据来源包括：ACM Transactions on HRI、Science Robotics（Columbia Emo）、PNAS（婴儿图式 fMRI）、Frontiers in Robotics and AI（LLM 实时情绪）、arXiv（Reachy Mini、Mishra & Skantze 2024）、Applied Soft Computing 系统综述、Goldman Sachs Research、IEEE Spectrum、Fast Company / Engadget（Anki/Cozmo）、Engineered Arts、Disney Research / The Walt Disney Company、NPR / CMU（Baymax）、皮克斯 / AWN（WALL-E）等。数据存在口径差异或不确定处已在正文与"注意事项"中标注。</p>
    <p class="src">© 2026 · 机器人情感表达设计语言研究报告 · 供视觉/交互/工业设计决策参考。图表为基于所引数据的可视化重建，示意性曲线（如恐怖谷）非精确测量值。</p>
  </div>
</footer>

<script>
const gridC='#26324d', tickC='#9aa7c2';
Chart.defaults.color=tickC; Chart.defaults.font.family="'PingFang SC','Microsoft YaHei',sans-serif";

/* 1. Uncanny valley */
new Chart(document.getElementById('uncannyChart'),{
  type:'line',
  data:{labels:[0,10,20,30,40,50,60,70,80,90,100],
    datasets:[{label:'情感好感度（示意）',
      data:[10,28,45,52,50,40,15,-35,-55,20,60],
      borderColor:'#5ee0c8',backgroundColor:'rgba(94,224,200,.12)',
      tension:.45,fill:true,pointRadius:0,borderWidth:3}]},
  options:{plugins:{legend:{display:false},
    tooltip:{callbacks:{title:(t)=>'拟人度 '+t[0].label}}},
    scales:{x:{title:{display:true,text:'拟人度（0 纯机械 → 100 真人）'},grid:{color:gridC}},
      y:{title:{display:true,text:'好感度（负=不适）'},grid:{color:gridC}}},
    annotationsFake:true}
});

/* 2. recognition comparison */
new Chart(document.getElementById('recogChart'),{
  type:'bar',
  data:{labels:['Happiness','Surprise','Sad','Fear','Anger','Disgust'],
    datasets:[
      {label:'算法识别人脸（%）',data:[96.4,96.3,93.9,93.9,91.7,93.7],backgroundColor:'#7aa2ff'},
      {label:'人识别机器人表情·精确标签（%）',data:[28,null,55,39.8,81.8,0],backgroundColor:'#ffb454'}
    ]},
  options:{plugins:{legend:{position:'top'}},
    scales:{y:{beginAtZero:true,max:100,grid:{color:gridC},title:{display:true,text:'准确率 %'}},
      x:{grid:{color:gridC}}}}
});

/* 3. radar */
new Chart(document.getElementById('radarChart'),{
  type:'radar',
  data:{labels:['情感表现力','成本控制','量产可靠性','恐怖谷安全','迭代速度'],
    datasets:[
      {label:'屏幕式眼睛',data:[3.5,5,4.5,4.5,5],borderColor:'#5ee0c8',backgroundColor:'rgba(94,224,200,.15)'},
      {label:'机械拟真面部',data:[5,1,1.5,1.5,2],borderColor:'#ff6b8a',backgroundColor:'rgba(255,107,138,.15)'},
      {label:'抽象光效',data:[2,5,5,5,4.5],borderColor:'#ffb454',backgroundColor:'rgba(255,180,84,.12)'},
      {label:'纯肢体动效',data:[3,3,3.5,4.5,2.5],borderColor:'#b28cff',backgroundColor:'rgba(178,140,255,.12)'}
    ]},
  options:{plugins:{legend:{position:'top'}},
    scales:{r:{min:0,max:5,ticks:{stepSize:1,backdropColor:'transparent'},
      grid:{color:gridC},angleLines:{color:gridC},pointLabels:{color:'#e9edf6',font:{size:12}}}}}
});

/* 5. gantt-like */
const phases=['① 调研','② 原型','③ 测试','④ 系统化','⑤ 品牌化'];
new Chart(document.getElementById('ganttChart'),{
  type:'bar',
  data:{labels:phases,
    datasets:[
      {label:'起始周',data:[0,6,16,22,32],backgroundColor:'rgba(0,0,0,0)',stack:'s'},
      {label:'持续周数',data:[8,10,8,12,10],backgroundColor:['#5ee0c8','#7aa2ff','#ffb454','#b28cff','#63d68a'],stack:'s'}
    ]},
  options:{indexAxis:'y',plugins:{legend:{display:false},
    tooltip:{callbacks:{label:(c)=>c.datasetIndex===1?('约 '+c.raw+' 周'):''}}},
    scales:{x:{stacked:true,grid:{color:gridC},title:{display:true,text:'相对周数（可重叠）'}},
      y:{stacked:true,grid:{color:gridC}}}}
});
</script>
</body>
</html>