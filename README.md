# luxshare-timeline
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=3.0">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="mobile-web-app-capable" content="yes">
<title>立讯精密发展里程碑全景时间轴</title>
<style>
  * { margin: 0; padding: 0; box-sizing: border-box; -webkit-tap-highlight-color: transparent; }
  html { font-size: 16px; }
  body {
    font-family: -apple-system, BlinkMacSystemFont, "PingFang SC", "Helvetica Neue", "Microsoft YaHei", sans-serif;
    background: #F0F2F5;
    color: #1a1a2e;
    min-height: 100vh;
    -webkit-text-size-adjust: 100%;
  }

  /* === HEADER === */
  .header {
    position: sticky; top: 0; z-index: 50;
    background: linear-gradient(135deg, #1a1a2e 0%, #2d2d5e 100%);
    color: #fff; padding: 16px 16px 12px;
    box-shadow: 0 2px 10px rgba(0,0,0,0.12);
  }
  .header h1 {
    font-size: 18px; font-weight: 800; letter-spacing: -0.01em;
    margin-bottom: 4px;
  }
  .header p { font-size: 12px; opacity: 0.75; }

  /* === FILTER BAR === */
  .filter-bar {
    display: flex; gap: 6px; padding: 10px 16px;
    background: #fff; border-bottom: 1px solid #E5E7EB;
    overflow-x: auto; -webkit-overflow-scrolling: touch;
    position: sticky; top: 0; z-index: 40;
    scrollbar-width: none;
  }
  .filter-bar::-webkit-scrollbar { display: none; }
  .filter-btn {
    display: flex; align-items: center; gap: 5px;
    padding: 7px 12px; border-radius: 18px; border: none;
    font-size: 12px; font-weight: 600; cursor: pointer;
    white-space: nowrap; flex-shrink: 0;
    transition: all 0.2s ease; outline: none;
  }
  .filter-btn .fdot {
    width: 8px; height: 8px; border-radius: 50%; flex-shrink: 0;
  }
  .filter-btn .cnt {
    font-size: 10px; opacity: 0.8; background: rgba(0,0,0,0.08);
    padding: 1px 5px; border-radius: 8px;
  }

  /* === TIMELINE (mobile: vertical) === */
  .timeline {
    position: relative; padding: 20px 0 32px;
  }
  .timeline::before {
    content: ''; position: absolute;
    left: 28px; top: 0; bottom: 0;
    width: 3px; background: linear-gradient(to bottom, #D1D5DB 0%, #9CA3AF 50%, #D1D5DB 100%);
    border-radius: 2px;
  }

  /* Year separator */
  .year-sep {
    position: relative; padding: 14px 16px 6px 56px;
    font-size: 20px; font-weight: 800; color: #1a1a2e;
    letter-spacing: -0.02em;
  }
  .year-sep .year-circle {
    position: absolute; left: 17px; top: 14px;
    width: 25px; height: 25px; border-radius: 50%;
    background: #1a1a2e; color: #fff;
    display: flex; align-items: center; justify-content: center;
    font-size: 9px; font-weight: 800;
    box-shadow: 0 2px 6px rgba(0,0,0,0.2);
  }

  /* Milestone card */
  .ms-card {
    position: relative; margin: 8px 16px 8px 56px;
    background: #fff; border-radius: 12px;
    box-shadow: 0 1px 4px rgba(0,0,0,0.06), 0 1px 2px rgba(0,0,0,0.04);
    overflow: hidden; cursor: pointer;
    transition: all 0.2s ease;
    border-left: 4px solid var(--cat-color);
  }
  .ms-card:active { transform: scale(0.985); }
  .ms-card.dimmed { display: none; }
  .ms-card .card-dot {
    position: absolute; left: -33px; top: 14px;
    width: 13px; height: 13px; border-radius: 50%;
    background: var(--cat-color);
    border: 3px solid #F0F2F5;
    box-shadow: 0 0 0 2px var(--cat-color);
    transition: all 0.2s ease;
  }
  .ms-card.expanded .card-dot {
    width: 17px; height: 17px; left: -35px; top: 12px;
  }
  .ms-card .card-head {
    display: flex; align-items: center; gap: 8px;
    padding: 10px 14px 8px;
  }
  .ms-card .cat-badge {
    font-size: 10px; font-weight: 700; color: #fff;
    padding: 2px 8px; border-radius: 8px;
    background: var(--cat-color); flex-shrink: 0;
  }
  .ms-card .event-text {
    font-size: 13.5px; font-weight: 600; color: #1a1a2e;
    line-height: 1.5; flex: 1;
  }
  .ms-card .expand-icon {
    font-size: 14px; color: #9CA3AF; flex-shrink: 0;
    transition: transform 0.2s ease;
  }
  .ms-card.expanded .expand-icon { transform: rotate(180deg); }

  /* Expandable detail */
  .ms-card .card-detail {
    max-height: 0; overflow: hidden;
    transition: max-height 0.3s ease, padding 0.3s ease;
  }
  .ms-card.expanded .card-detail {
    max-height: 300px;
  }
  .ms-card .card-detail .inner {
    padding: 0 14px 12px;
    font-size: 12.5px; color: #4B5563; line-height: 1.6;
    border-top: 1px dashed #E5E7EB;
    margin-top: 4px; padding-top: 10px;
  }
  .ms-card .card-detail .inner strong {
    color: var(--cat-color); font-weight: 700;
  }

  /* === SUMMARY === */
  .summary {
    display: flex; justify-content: space-around;
    background: #fff; margin: 12px 16px; padding: 16px 8px;
    border-radius: 12px; box-shadow: 0 1px 4px rgba(0,0,0,0.06);
  }
  .summary-item { text-align: center; }
  .summary-num { font-size: 22px; font-weight: 800; }
  .summary-label { font-size: 10px; color: #6B7280; margin-top: 2px; }

  .footer {
    text-align: center; font-size: 11px; color: #9CA3AF;
    padding: 16px; line-height: 1.6;
  }

  /* === DESKTOP: wider screens get horizontal layout === */
  @media (min-width: 768px) {
    .header { padding: 22px 28px 16px; }
    .header h1 { font-size: 24px; }
    .header p { font-size: 13px; }
    .filter-bar { padding: 12px 28px; gap: 8px; top: auto; position: relative; }
    .filter-btn { padding: 8px 16px; font-size: 13px; }

    .timeline { padding: 28px 28px 40px; }
    .timeline::before { left: 40px; width: 4px; }
    .year-sep { padding: 16px 16px 8px 72px; font-size: 24px; }
    .year-sep .year-circle {
      left: 26px; width: 32px; height: 32px; font-size: 11px;
    }
    .ms-card {
      margin: 10px 16px 10px 72px;
      border-radius: 14px;
      border-left-width: 5px;
    }
    .ms-card .card-dot { left: -42px; top: 16px; width: 15px; height: 15px; }
    .ms-card.expanded .card-dot { width: 19px; height: 19px; left: -44px; top: 14px; }
    .ms-card .card-head { padding: 12px 18px 10px; }
    .ms-card .event-text { font-size: 14.5px; }
    .ms-card .cat-badge { font-size: 11px; padding: 3px 10px; }
    .ms-card .card-detail .inner { font-size: 13px; padding: 0 18px 14px; padding-top: 12px; }

    .summary { margin: 16px 28px; padding: 20px 12px; }
    .summary-num { font-size: 26px; }
    .summary-label { font-size: 11px; }
  }

  /* Safe area for notch phones */
  @supports (padding-top: env(safe-area-inset-top)) {
    .header { padding-top: max(16px, env(safe-area-inset-top)); }
    .footer { padding-bottom: max(16px, env(safe-area-inset-bottom)); }
  }
</style>
</head>
<body>

<div class="header">
  <h1>立讯精密 · 发展里程碑全景时间轴</h1>
  <p>1988 — 2026 · 按业务板块分区 · 点击卡片查看详情</p>
</div>

<div class="filter-bar" id="filterBar"></div>

<div class="timeline" id="timeline"></div>

<div class="summary" id="summary"></div>

<div class="footer">
  点击卡片展开事件详情与战略价值<br>
  顶部按钮可按板块筛选 · 再次点击恢复全部
</div>

<script>
const CATEGORIES = [
  { id: "foundation", label: "创业与资本", color: "#6B7280", bg: "#F3F4F6" },
  { id: "consumer",   label: "消费电子",   color: "#2563EB", bg: "#EFF6FF" },
  { id: "auto",       label: "汽车",       color: "#DC2626", bg: "#FEF2F2" },
  { id: "ai",         label: "AI 算力",    color: "#7C3AED", bg: "#F5F3FF" },
  { id: "robot",      label: "机器人",     color: "#059669", bg: "#ECFDF5" },
];

const milestones = [
  { year: 1988, cat: "foundation", event: "王来春进入富士康深圳工厂，从产线做到课长，前后十年", value: "奠定与鸿海体系的师承关系，积累连接器制造 Know-how" },
  { year: 1997, cat: "foundation", event: "王来春离开富士康自主创业", value: "完成从打工到创业的关键人生转折" },
  { year: 1999, cat: "foundation", event: "王来春与兄长王来胜共同收购香港立讯股权，\"立讯系\"资本母体成形", value: "兄妹联手控股架构形成，后续所有资本运作的底层股权结构" },
  { year: 2004, cat: "foundation", event: "立讯精密工业（深圳）有限公司成立", value: "A 股上市主体诞生，承担规模化制造与产业升级" },
  { year: 2009, cat: "consumer", event: "鸿海体系富港电子入股 3.08%；首次切入苹果供应链", value: "老东家背书导入苹果订单，开启二十年果链深度绑定" },
  { year: 2010, cat: "foundation", event: "深交所中小板挂牌上市，全年营收 10.11 亿元", value: "拿到资本市场入场券，开启并购 + 苹果订单双轮加速" },
  { year: 2010, cat: "consumer", event: "收购博硕科技 75% 股权", value: "横向扩品类，进入线束业务及 Sony PS4/Xbox 供应链" },
  { year: 2011, cat: "consumer", event: "以约 11.8 亿元收购昆山联滔电子", value: "深度切入苹果 iPad 连接器供应，进入果链一线的标志性并购" },
  { year: 2011, cat: "auto", event: "成立汽车事业部", value: "提前 14 年布局汽车赛道，为日后收购莱尼埋下伏笔" },
  { year: 2012, cat: "auto", event: "收购福建源光电装 55% 股权", value: "为汽车第二曲线落地具体业务载体（汽车线束）" },
  { year: 2013, cat: "consumer", event: "收购科尔通，切入华为、艾默生通信及医疗市场", value: "客户结构从单一消费电子向通讯、医疗扩展" },
  { year: 2016, cat: "foundation", event: "全年营收突破百亿（137.6 亿元）", value: "跨越百亿企业门槛，确立中型电子制造龙头地位" },
  { year: 2017, cat: "consumer", event: "获苹果 AirPods 整机代工资格", value: "立讯发展史上最重要转折点：从零部件升级为整机 ODM" },
  { year: 2017, cat: "consumer", event: "库克亲赴昆山产线参观；\"能与凤凰同飞的必是俊鸟\"", value: "被苹果公开加冕为战略级合作伙伴" },
  { year: 2018, cat: "auto", event: "控股股东收购德国采埃孚（ZF）旗下 BCS 车身控制系统业务", value: "母公司平台先行储备欧洲 Tier1 汽车电子技术" },
  { year: 2019, cat: "consumer", event: "AirPods 独家/主力代工确立，TWS 爆发股价飙升 240%", value: "单一爆品引爆估值，王来春登顶中国商界女首富" },
  { year: 2020, cat: "consumer", event: "以 33 亿元收购纬创昆山工厂", value: "首次拿下 iPhone 整机组装，成为内地首家 iPhone 代工商" },
  { year: 2020, cat: "consumer", event: "联合控股日铠电脑（约 60 亿元增资）", value: "补齐铝合金机壳能力，形成垂直一体化闭环" },
  { year: 2021, cat: "foundation", event: "营收 1539.7 亿元，首破千亿；超越鸿海成苹果第一大供应商", value: "跻身全球电子代工第一梯队" },
  { year: 2022, cat: "auto", event: "与奇瑞集团战略合作，入股奇瑞汽车 100.54 亿元", value: "介入新能源整车 ODM 模式，坚守不造整车边界" },
  { year: 2022, cat: "ai", event: "以约 11 亿港元收购汇聚科技 74.67% 股权", value: "拓展高速电缆能力，为 AI 数据中心 224G 铜连接埋下伏笔" },
  { year: 2023, cat: "consumer", event: "确定为 Apple Vision Pro 独家组装厂商", value: "在头显新品类上复制 AirPods 路径" },
  { year: 2023, cat: "consumer", event: "以 21.08 亿元收购和硕子公司世硕 62.5%", value: "进一步扩张 iPhone/iPad 组装份额" },
  { year: 2024, cat: "consumer", event: "Apple Vision Pro 正式发售，立讯独家组装", value: "验证精密光机电系统集成能力" },
  { year: 2024, cat: "auto", event: "汽车业务营收突破百亿（137.58 亿元，+48.7%）", value: "第二曲线跨过百亿临界点，从陪跑升级为独立引擎" },
  { year: 2024, cat: "ai", event: "Intel Capital 入股东莞立讯技术 3%", value: "全球芯片巨头战略卡位，AI 算力业务获国际客户认可" },
  { year: 2024, cat: "auto", event: "以 41 亿元收购德国莱尼汽车线束业务", value: "跃居全球前三大汽车线束供应商" },
  { year: 2024, cat: "consumer", event: "闻泰科技公告转让消费电子 ODM 业务", value: "补齐安卓阵营 ODM 能力" },
  { year: 2025, cat: "consumer", event: "完成对闻泰首批子公司收购；闻泰 ODM 议案通过", value: "安卓大客户（三星、小米）进入立讯体系" },
  { year: 2025, cat: "auto", event: "通过新加坡子公司完成莱尼交割", value: "汽车业务全球化最大单笔并购落地" },
  { year: 2025, cat: "robot", event: "机器人总部基地落户常熟，投资 50 亿元", value: "落地机器人 + 智能装备新业务支柱" },
  { year: 2025, cat: "foundation", event: "递交 H 股上市申请；奇瑞港股上市；市值破 5000 亿", value: "启动 A+H 双资本平台，资本运作多点开花" },
  { year: 2025, cat: "foundation", event: "营收 3323 亿元，净利润 166 亿元，总资产破 3000 亿", value: "跨过 3000 亿营收门槛，进入超大型制造平台序列" },
  { year: 2026, cat: "foundation", event: "声明澄清代工传闻，定位升级为全球领先智能制造平台", value: "主动管理资本市场预期，反代工叙事的正式表态" },
  { year: 2026, cat: "ai", event: "AI 算力全面发力：224G 铜连接量产，800G/1.6T 光模块导入大客户", value: "3000 亿体量上保持双位数增长，AI/汽车/ODM 三引擎接力" },
];

const ALL_YEARS = [...new Set(milestones.map(m => m.year))].sort((a, b) => a - b);
let activeFilter = null;
let expandedCard = null;

function getCat(id) { return CATEGORIES.find(c => c.id === id); }

// === FILTERS ===
function renderFilters() {
  const bar = document.getElementById('filterBar');
  bar.innerHTML = '';
  // "All" button
  const allBtn = document.createElement('button');
  const allActive = activeFilter === null;
  allBtn.className = 'filter-btn';
  allBtn.style.background = allActive ? '#1a1a2e' : '#E5E7EB';
  allBtn.style.color = allActive ? '#fff' : '#6B7280';
  allBtn.innerHTML = `全部<span class="cnt">${milestones.length}</span>`;
  allBtn.onclick = () => { activeFilter = null; render(); };
  bar.appendChild(allBtn);

  CATEGORIES.forEach(cat => {
    const btn = document.createElement('button');
    const isActive = activeFilter === cat.id;
    btn.className = 'filter-btn';
    btn.style.background = isActive ? cat.color : '#E5E7EB';
    btn.style.color = isActive ? '#fff' : '#6B7280';
    const cnt = milestones.filter(m => m.cat === cat.id).length;
    btn.innerHTML = `<span class="fdot" style="background:${isActive ? '#fff' : cat.color}"></span>${cat.label}<span class="cnt">${cnt}</span>`;
    btn.onclick = () => { activeFilter = activeFilter === cat.id ? null : cat.id; render(); };
    bar.appendChild(btn);
  });
}

// === TIMELINE ===
function renderTimeline() {
  const tl = document.getElementById('timeline');
  tl.innerHTML = '';
  expandedCard = null;

  ALL_YEARS.forEach(year => {
    const yearItems = milestones.filter(m => m.year === year);
    const visibleItems = yearItems.filter(m => activeFilter === null || m.cat === activeFilter);
    if (visibleItems.length === 0) return;

    // Year separator
    const sep = document.createElement('div');
    sep.className = 'year-sep';
    sep.innerHTML = `<span class="year-circle">${String(year).slice(-2)}</span>${year}`;
    tl.appendChild(sep);

    // Cards
    visibleItems.forEach((item, idx) => {
      const cat = getCat(item.cat);
      const card = document.createElement('div');
      card.className = 'ms-card';
      card.style.setProperty('--cat-color', cat.color);
      card.innerHTML = `
        <div class="card-dot"></div>
        <div class="card-head">
          <span class="cat-badge">${cat.label}</span>
          <span class="event-text">${item.event}</span>
          <span class="expand-icon">▼</span>
        </div>
        <div class="card-detail">
          <div class="inner"><strong>战略价值：</strong>${item.value}</div>
        </div>
      `;
      card.onclick = (e) => {
        e.stopPropagation();
        const wasExpanded = card.classList.contains('expanded');
        // Close all
        tl.querySelectorAll('.ms-card.expanded').forEach(c => c.classList.remove('expanded'));
        if (!wasExpanded) {
          card.classList.add('expanded');
          expandedCard = card;
        } else {
          expandedCard = null;
        }
      };
      tl.appendChild(card);
    });
  });
}

// Close on tap outside
document.addEventListener('click', () => {
  if (expandedCard) {
    expandedCard.classList.remove('expanded');
    expandedCard = null;
  }
});

// === SUMMARY ===
function renderSummary() {
  const el = document.getElementById('summary');
  el.innerHTML = '';
  CATEGORIES.forEach(cat => {
    const cnt = milestones.filter(m => m.cat === cat.id).length;
    const d = document.createElement('div');
    d.className = 'summary-item';
    d.innerHTML = `<div class="summary-num" style="color:${cat.color}">${cnt}</div><div class="summary-label">${cat.label}</div>`;
    el.appendChild(d);
  });
  const total = document.createElement('div');
  total.className = 'summary-item';
  total.innerHTML = `<div class="summary-num" style="color:#1a1a2e">${milestones.length}</div><div class="summary-label">总计</div>`;
  el.appendChild(total);
}

function render() {
  renderFilters();
  renderTimeline();
  renderSummary();
}

render();
</script>
</body>
</html>
