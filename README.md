# qinghuanandejiangshi.github.io
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Hermes Windows 一键安装器</title>
  <style>
    /* ── Reset & Base ── */
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
    :root {
      --bg: #0d0f14;
      --surface: #13161e;
      --surface2: #1a1e2a;
      --border: #2a2f3e;
      --accent: #6c63ff;
      --accent2: #00d4aa;
      --accent3: #ff6b6b;
      --text: #e8eaf0;
      --muted: #7a8099;
      --code-bg: #0a0c10;
      --radius: 14px;
    }
    html { scroll-behavior: smooth; }
    body {
      background: var(--bg);
      color: var(--text);
      font-family: 'Segoe UI', 'PingFang SC', 'Microsoft YaHei', sans-serif;
      line-height: 1.7;
      font-size: 16px;
    }
    a { color: var(--accent); text-decoration: none; }
    a:hover { text-decoration: underline; }
    img { max-width: 100%; border-radius: var(--radius); display: block; }

    /* ── Nav ── */
    nav {
      position: sticky; top: 0; z-index: 100;
      background: rgba(13,15,20,0.85);
      backdrop-filter: blur(12px);
      border-bottom: 1px solid var(--border);
      padding: 0 2rem;
      height: 56px;
      display: flex; align-items: center; justify-content: space-between;
    }
    .nav-logo {
      font-size: 1.1rem; font-weight: 700;
      background: linear-gradient(90deg, var(--accent), var(--accent2));
      -webkit-background-clip: text; -webkit-text-fill-color: transparent;
    }
    .nav-links { display: flex; gap: 1.5rem; font-size: 0.9rem; color: var(--muted); }
    .nav-links a { color: var(--muted); }
    .nav-links a:hover { color: var(--text); text-decoration: none; }

    /* ── Hero ── */
    .hero {
      min-height: 92vh;
      display: flex; flex-direction: column; align-items: center; justify-content: center;
      text-align: center; padding: 4rem 1.5rem;
      background: radial-gradient(ellipse 80% 60% at 50% 0%, rgba(108,99,255,0.15) 0%, transparent 70%),
                  radial-gradient(ellipse 60% 40% at 80% 80%, rgba(0,212,170,0.08) 0%, transparent 60%);
      position: relative; overflow: hidden;
    }
    .hero-badge {
      display: inline-flex; align-items: center; gap: 0.5rem;
      background: rgba(108,99,255,0.15); border: 1px solid rgba(108,99,255,0.4);
      color: #a09aff; font-size: 0.8rem; font-weight: 600;
      padding: 0.35rem 1rem; border-radius: 99px; margin-bottom: 1.8rem;
      letter-spacing: 0.05em;
    }
    .hero-badge span { width: 7px; height: 7px; background: var(--accent2); border-radius: 50%; animation: pulse 2s infinite; }
    @keyframes pulse { 0%,100%{opacity:1;transform:scale(1)} 50%{opacity:.5;transform:scale(1.4)} }
    .hero h1 {
      font-size: clamp(2.2rem, 6vw, 4rem);
      font-weight: 800; line-height: 1.15;
      letter-spacing: -0.02em; margin-bottom: 1rem;
    }
    .hero h1 .gradient {
      background: linear-gradient(135deg, #a09aff 0%, #6c63ff 40%, #00d4aa 100%);
      -webkit-background-clip: text; -webkit-text-fill-color: transparent;
    }
    .hero-sub {
      font-size: clamp(1rem, 2vw, 1.2rem); color: var(--muted);
      max-width: 600px; margin: 0 auto 2.5rem;
    }
    .hero-btns { display: flex; gap: 1rem; flex-wrap: wrap; justify-content: center; }
    .btn {
      display: inline-flex; align-items: center; gap: 0.5rem;
      padding: 0.75rem 1.8rem; border-radius: 10px;
      font-size: 0.95rem; font-weight: 600; cursor: pointer;
      transition: transform .15s, box-shadow .15s;
    }
    .btn:hover { transform: translateY(-2px); text-decoration: none; }
    .btn-primary {
      background: linear-gradient(135deg, var(--accent), #4f46e5);
      color: #fff; box-shadow: 0 4px 24px rgba(108,99,255,0.4);
    }
    .btn-primary:hover { box-shadow: 0 8px 32px rgba(108,99,255,0.6); }
    .btn-ghost {
      background: transparent; color: var(--text);
      border: 1px solid var(--border);
    }
    .btn-ghost:hover { background: var(--surface2); border-color: var(--accent); }

    /* hero grid-lines background */
    .hero::before {
      content: '';
      position: absolute; inset: 0;
      background-image: linear-gradient(var(--border) 1px, transparent 1px),
                        linear-gradient(90deg, var(--border) 1px, transparent 1px);
      background-size: 60px 60px;
      opacity: 0.18; pointer-events: none;
      mask-image: radial-gradient(ellipse at center, black 30%, transparent 75%);
    }

    /* ── Section wrapper ── */
    section { padding: 5rem 1.5rem; }
    .container { max-width: 1100px; margin: 0 auto; }
    .section-title {
      font-size: clamp(1.6rem, 3.5vw, 2.2rem); font-weight: 700;
      margin-bottom: 0.6rem; text-align: center;
    }
    .section-sub { color: var(--muted); text-align: center; margin-bottom: 3.5rem; font-size: 1rem; }
    .tag {
      display: inline-block; font-size: 0.7rem; font-weight: 700;
      text-transform: uppercase; letter-spacing: 0.1em;
      color: var(--accent2); background: rgba(0,212,170,0.1);
      border: 1px solid rgba(0,212,170,0.3);
      padding: 0.2rem 0.7rem; border-radius: 99px; margin-bottom: 0.8rem;
    }

    /* ── Feature cards ── */
    .features-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(290px, 1fr));
      gap: 1.2rem;
    }
    .feature-card {
      background: var(--surface); border: 1px solid var(--border);
      border-radius: var(--radius); padding: 1.6rem 1.8rem;
      transition: border-color .2s, transform .2s, box-shadow .2s;
      position: relative; overflow: hidden;
    }
    .feature-card::before {
      content: ''; position: absolute; top: 0; left: 0; right: 0; height: 2px;
      background: linear-gradient(90deg, var(--accent), var(--accent2));
      opacity: 0; transition: opacity .2s;
    }
    .feature-card:hover { border-color: var(--accent); transform: translateY(-3px); box-shadow: 0 8px 32px rgba(108,99,255,0.15); }
    .feature-card:hover::before { opacity: 1; }
    .feature-icon { font-size: 1.8rem; margin-bottom: 0.8rem; }
    .feature-card h3 { font-size: 1rem; font-weight: 700; margin-bottom: 0.5rem; }
    .feature-card p { font-size: 0.88rem; color: var(--muted); }

    /* ── Steps ── */
    .steps { display: flex; flex-direction: column; gap: 1rem; max-width: 680px; margin: 0 auto; }
    .step {
      display: flex; gap: 1.4rem; align-items: flex-start;
      background: var(--surface); border: 1px solid var(--border);
      border-radius: var(--radius); padding: 1.4rem 1.6rem;
      transition: border-color .2s;
    }
    .step:hover { border-color: var(--accent2); }
    .step-num {
      flex-shrink: 0; width: 36px; height: 36px;
      background: linear-gradient(135deg, var(--accent), var(--accent2));
      border-radius: 50%; display: flex; align-items: center; justify-content: center;
      font-weight: 800; font-size: 0.9rem; color: #fff;
    }
    .step-body h3 { font-size: 0.95rem; font-weight: 700; margin-bottom: 0.3rem; }
    .step-body p { font-size: 0.85rem; color: var(--muted); }
    .step-body code {
      background: var(--code-bg); border: 1px solid var(--border);
      border-radius: 5px; padding: 0.15rem 0.45rem;
      font-size: 0.82rem; color: var(--accent2);
      font-family: 'Consolas', 'Monaco', monospace;
    }

    /* ── Screenshots ── */
    .screenshots-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
      gap: 1.5rem;
    }
    .screenshot-card {
      background: var(--surface2); border: 1px solid var(--border);
      border-radius: var(--radius); overflow: hidden;
      transition: transform .2s, box-shadow .2s;
    }
    .screenshot-card:hover { transform: translateY(-4px); box-shadow: 0 12px 40px rgba(0,0,0,0.4); }
    .screenshot-card img { width: 100%; object-fit: cover; border-radius: 0; }
    .screenshot-caption {
      padding: 0.8rem 1.1rem;
      font-size: 0.82rem; color: var(--muted); border-top: 1px solid var(--border);
    }
    .screenshot-caption strong { color: var(--text); display: block; font-size: 0.88rem; margin-bottom: 0.15rem; }

    /* ── Providers ── */
    .providers-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
      gap: 1rem;
    }
    .provider-card {
      background: var(--surface); border: 1px solid var(--border);
      border-radius: var(--radius); padding: 1.2rem 1.4rem;
      display: flex; align-items: center; gap: 1rem;
      transition: border-color .2s;
    }
    .provider-card:hover { border-color: var(--accent2); }
    .provider-icon { font-size: 1.6rem; flex-shrink: 0; }
    .provider-card h4 { font-size: 0.9rem; font-weight: 700; margin-bottom: 0.15rem; }
    .provider-card p { font-size: 0.78rem; color: var(--muted); }
    .badge-rec {
      font-size: 0.65rem; font-weight: 700; text-transform: uppercase;
      background: rgba(0,212,170,0.15); color: var(--accent2);
      border: 1px solid rgba(0,212,170,0.3); border-radius: 4px;
      padding: 0.1rem 0.4rem; margin-left: 0.4rem; vertical-align: middle;
    }

    /* ── Flow diagram ── */
    .flow {
      background: var(--code-bg); border: 1px solid var(--border);
      border-radius: var(--radius); padding: 2rem 2.5rem;
      font-family: 'Consolas', 'Monaco', monospace; font-size: 0.82rem;
      line-height: 1.9; overflow-x: auto; color: #c9d1d9;
      max-width: 640px; margin: 0 auto;
    }
    .flow .c-purple { color: #a09aff; }
    .flow .c-green  { color: var(--accent2); }
    .flow .c-orange { color: #ffa657; }
    .flow .c-muted  { color: var(--muted); }

    /* ── Accordion FAQ ── */
    .faq-list { max-width: 700px; margin: 0 auto; display: flex; flex-direction: column; gap: 0.6rem; }
    details {
      background: var(--surface); border: 1px solid var(--border);
      border-radius: var(--radius); overflow: hidden;
    }
    details[open] { border-color: var(--accent); }
    summary {
      padding: 1rem 1.4rem; cursor: pointer; font-weight: 600; font-size: 0.93rem;
      list-style: none; display: flex; justify-content: space-between; align-items: center;
      user-select: none;
    }
    summary::-webkit-details-marker { display: none; }
    summary::after { content: '+'; color: var(--accent); font-size: 1.1rem; transition: transform .2s; }
    details[open] summary::after { transform: rotate(45deg); }
    .faq-body { padding: 0 1.4rem 1.1rem; font-size: 0.88rem; color: var(--muted); }
    .faq-body code {
      background: var(--code-bg); border: 1px solid var(--border);
      border-radius: 5px; padding: 0.15rem 0.45rem;
      font-size: 0.82rem; color: var(--accent2);
      font-family: 'Consolas', monospace;
    }

    /* ── Requirements table ── */
    .req-table { width: 100%; border-collapse: collapse; margin-top: 1.5rem; }
    .req-table th, .req-table td {
      text-align: left; padding: 0.75rem 1.2rem;
      border-bottom: 1px solid var(--border); font-size: 0.88rem;
    }
    .req-table th { color: var(--muted); font-weight: 600; font-size: 0.78rem; text-transform: uppercase; letter-spacing: 0.05em; }
    .req-table tr:last-child td { border-bottom: none; }
    .req-table tr:hover td { background: var(--surface2); }

    /* ── Footer ── */
    footer {
      border-top: 1px solid var(--border);
      padding: 2.5rem 1.5rem; text-align: center;
      color: var(--muted); font-size: 0.85rem;
    }
    footer a { color: var(--accent); }

    /* ── Divider ── */
    .divider { border: none; border-top: 1px solid var(--border); margin: 0; }

    /* ── Responsive ── */
    @media (max-width: 600px) {
      nav .nav-links { display: none; }
      .flow { padding: 1.2rem; font-size: 0.75rem; }
      .step { gap: 1rem; }
    }
  </style>
</head>
<body>

<!-- ══ NAV ══════════════════════════════════════════════ -->
<nav>
  <div class="nav-logo">⚡ Hermes-one-clink</div>
  <div class="nav-links">
    <a href="#features">功能</a>
    <a href="#screenshots">截图</a>
    <a href="#quickstart">快速开始</a>
    <a href="#providers">模型支持</a>
    <a href="#faq">FAQ</a>
    <a href="https://github.com/qinghuanandejiangshi/Hermes-one-clink" target="_blank">GitHub ↗</a>
  </div>
</nav>

<!-- ══ HERO ══════════════════════════════════════════════ -->
<div class="hero">
  <div class="hero-badge"><span></span>Windows 一键安装 · 无需命令行</div>
  <h1>
    让所有人都能用上<br/>
    <span class="gradient">Hermes AI 助手</span>
  </h1>
  <p class="hero-sub">
    把复杂的 WSL2 + Ubuntu + Python 环境配置，压缩成一次双击。<br/>
    面向非技术用户设计，全程图形化操作，10分钟开始对话。
  </p>
  <div class="hero-btns">
    <a href="#quickstart" class="btn btn-primary">🚀 立即开始</a>
    <a href="https://github.com/qinghuanandejiangshi/Hermes-one-clink" class="btn btn-ghost" target="_blank">⭐ GitHub</a>
    <a href="https://github.com/qinghuanandejiangshi/Hermes-one-clink/blob/main/%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E%EF%BC%88%E5%B0%8F%E7%99%BD%E7%89%88%EF%BC%89.md" class="btn btn-ghost">📖 使用说明</a>
  </div>
</div>

<!-- ══ FEATURES ══════════════════════════════════════════ -->
<section id="features">
  <div class="container">
    <div class="tag">Features</div>
    <h2 class="section-title">为什么选择这个安装器？</h2>
    <p class="section-sub">官方安装需要 15+ 个手动步骤。这里只需要一次双击。</p>
    <div class="features-grid">
      <div class="feature-card">
        <div class="feature-icon">🖱️</div>
        <h3>真正的一键安装</h3>
        <p>自动完成 WSL2 → Ubuntu → Hermes Agent → WebUI 全流程，6 个步骤全自动，有问题自动重试。</p>
      </div>
      <div class="feature-card">
        <div class="feature-icon">📦</div>
        <h3>离线安装支持</h3>
        <p>将 <code style="color:var(--accent2);background:var(--code-bg);padding:2px 6px;border-radius:4px;">hermes-agent.zip</code> 放入 <code style="color:var(--accent2);background:var(--code-bg);padding:2px 6px;border-radius:4px;">resources/</code> 即可在无网环境运行，适合 U 盘分发。</p>
      </div>
      <div class="feature-card">
        <div class="feature-icon">🖼️</div>
        <h3>图形化 API Key 配置</h3>
        <p>Windows 原生弹窗输入框，支持 DeepSeek / OpenAI / OpenRouter，无需打开终端或编辑配置文件。</p>
      </div>
      <div class="feature-card">
        <div class="feature-icon">♾️</div>
        <h3>持久后台运行</h3>
        <p>使用独立 WSL 进程技术，WebUI 服务在关闭安装窗口后继续运行，不会断连。</p>
      </div>
      <div class="feature-card">
        <div class="feature-icon">⏸️</div>
        <h3>断点续装</h3>
        <p>记录安装进度检查点，中途断网或出错后重新运行，自动从断点继续，不重复已完成步骤。</p>
      </div>
      <div class="feature-card">
        <div class="feature-icon">🇨🇳</div>
        <h3>国内网络优化</h3>
        <p>自动切换阿里云 PyPI 镜像，DeepSeek / Kimi 等国内模型开箱即用，无需科学上网即可安装。</p>
      </div>
    </div>
  </div>
</section>

<hr class="divider" />

<!-- ══ SCREENSHOTS ════════════════════════════════════════ -->
<section id="screenshots">
  <div class="container">
    <div class="tag">Screenshots</div>
    <h2 class="section-title">实际效果预览</h2>
    <p class="section-sub">安装完成后即可在浏览器中使用完整的 AI 对话界面</p>
    <div class="screenshots-grid">
      <div class="screenshot-card">
        <img src="picture/Page UI.png" alt="Hermes WebUI 主界面" loading="lazy" />
        <div class="screenshot-caption">
          <strong>Hermes Web 界面</strong>
          安装完成后浏览器自动打开，支持多轮对话、文件上传、多模型切换
        </div>
      </div>
      <div class="screenshot-card">
        <img src="picture/console output.png" alt="安装进度控制台" loading="lazy" />
        <div class="screenshot-caption">
          <strong>安装进度输出</strong>
          全程显示详细进度，每步完成状态清晰可见，出错有提示
        </div>
      </div>
      <div class="screenshot-card">
        <img src="picture/GitHub project address.png" alt="GitHub 项目地址" loading="lazy" />
        <div class="screenshot-caption">
          <strong>开源项目</strong>
          完全开源，欢迎 Star 和贡献代码
        </div>
      </div>
    </div>
  </div>
</section>

<hr class="divider" />

<!-- ══ QUICK START ════════════════════════════════════════ -->
<section id="quickstart">
  <div class="container">
    <div class="tag">Quick Start</div>
    <h2 class="section-title">三步开始使用</h2>
    <p class="section-sub">整个过程约 10–30 分钟，取决于网络速度</p>
    <div class="steps">
      <div class="step">
        <div class="step-num">1</div>
        <div class="step-body">
          <h3>下载并运行安装程序</h3>
          <p>下载本项目（或复制到 U 盘），右键 <code>一键启动.bat</code> → <strong>以管理员身份运行</strong>。首次运行会弹出 UAC 权限请求，点「是」。</p>
        </div>
      </div>
      <div class="step">
        <div class="step-num">2</div>
        <div class="step-body">
          <h3>填写 API Key</h3>
          <p>安装到第 5 步时，弹出配置窗口。推荐国内用户填写 <strong>DeepSeek API Key</strong>（在 <a href="https://platform.deepseek.com/api_keys" target="_blank">platform.deepseek.com</a> 免费申请，填入 <code>sk-</code> 开头的字符串），其余可留空。</p>
        </div>
      </div>
      <div class="step">
        <div class="step-num">3</div>
        <div class="step-body">
          <h3>浏览器自动打开，开始使用</h3>
          <p>安装完成后浏览器自动打开 <code>http://127.0.0.1:8787</code>。按照首次配置向导选择模型，即可开始对话。</p>
        </div>
      </div>
      <div class="step">
        <div class="step-num">★</div>
        <div class="step-body">
          <h3>后续每次启动</h3>
          <p>安装完成后，以后每次只需右键 <code>后续启动.bat</code> → 以管理员身份运行，几秒内浏览器自动打开。</p>
        </div>
      </div>
    </div>
  </div>
</section>

<hr class="divider" />

<!-- ══ PROVIDERS ══════════════════════════════════════════ -->
<section id="providers">
  <div class="container">
    <div class="tag">Providers</div>
    <h2 class="section-title">支持的 AI 模型提供商</h2>
    <p class="section-sub">主流国内外模型均可使用，自定义端点支持 Ollama 等本地部署</p>
    <div class="providers-grid">
      <div class="provider-card">
        <div class="provider-icon">🐋</div>
        <div>
          <h4>DeepSeek <span class="badge-rec">推荐</span></h4>
          <p>国内首选，价格低廉，效果出色</p>
        </div>
      </div>
      <div class="provider-card">
        <div class="provider-icon">🌐</div>
        <div>
          <h4>OpenRouter</h4>
          <p>一个 Key 访问 100+ 模型</p>
        </div>
      </div>
      <div class="provider-card">
        <div class="provider-icon">🤖</div>
        <div>
          <h4>OpenAI</h4>
          <p>GPT-4o、o1、o3 等</p>
        </div>
      </div>
      <div class="provider-card">
        <div class="provider-icon">🌙</div>
        <div>
          <h4>Kimi（月之暗面）</h4>
          <p>通过自定义端点配置</p>
        </div>
      </div>
      <div class="provider-card">
        <div class="provider-icon">🦙</div>
        <div>
          <h4>Ollama</h4>
          <p>本地部署，完全离线</p>
        </div>
      </div>
      <div class="provider-card">
        <div class="provider-icon">🔌</div>
        <div>
          <h4>任意 OpenAI 兼容</h4>
          <p>自定义 Base URL 即可接入</p>
        </div>
      </div>
    </div>
  </div>
</section>

<hr class="divider" />

<!-- ══ HOW IT WORKS ═══════════════════════════════════════ -->
<section id="howto">
  <div class="container">
    <div class="tag">How It Works</div>
    <h2 class="section-title">工作原理</h2>
    <p class="section-sub">幕后做了什么（技术人员可以看看）</p>
    <div class="flow">
<span class="c-purple">Windows PowerShell</span>
│
├── <span class="c-orange">步骤 01</span>  启用 WSL2 + VirtualMachinePlatform 功能
├── <span class="c-orange">步骤 02</span>  <code>wsl --install -d Ubuntu</code>
├── <span class="c-orange">步骤 03</span>  <code>uv tool install hermes-agent</code>  <span class="c-muted">← 优先离线 ZIP</span>
├── <span class="c-orange">步骤 04</span>  解压 hermes-webui，安装 Python 依赖
├── <span class="c-orange">步骤 05</span>  Windows Forms 弹窗 → 写入 API Key 到 ~/.bashrc
└── <span class="c-orange">步骤 06</span>  <span class="c-green">Start-Process wsl.exe -WindowStyle Hidden</span>
             └─→ <code>exec python server.py</code>  <span class="c-muted">← 独立进程，不会被 Kill</span>
                   └─→ <span class="c-green">http://127.0.0.1:8787</span>  🎉
    </div>

    <!-- Requirements -->
    <div style="max-width:560px;margin:3rem auto 0;">
      <h3 style="text-align:center;font-size:1.1rem;margin-bottom:.5rem;">系统要求</h3>
      <table class="req-table">
        <thead>
          <tr><th>项目</th><th>要求</th></tr>
        </thead>
        <tbody>
          <tr><td>操作系统</td><td>Windows 10 (build 2004+) 或 Windows 11</td></tr>
          <tr><td>内存</td><td>建议 8 GB 以上</td></tr>
          <tr><td>磁盘空间</td><td>至少 10 GB 可用</td></tr>
          <tr><td>网络</td><td>首次安装需联网（或提前下载离线包）</td></tr>
          <tr><td>权限</td><td>需要管理员权限（仅首次安装）</td></tr>
        </tbody>
      </table>
    </div>
  </div>
</section>

<hr class="divider" />

<!-- ══ FAQ ════════════════════════════════════════════════ -->
<section id="faq">
  <div class="container">
    <div class="tag">FAQ</div>
    <h2 class="section-title">常见问题</h2>
    <p class="section-sub">遇到问题先看这里</p>
    <div class="faq-list">
      <details>
        <summary>安装时弹出「执行策略」或「安全警告」怎么办？</summary>
        <div class="faq-body">这是 Windows 对 PowerShell 脚本的正常安全提示。出现选项时选 <strong>「A（全是）」</strong> 即可继续安装。</div>
      </details>
      <details>
        <summary>安装卡在某步很久，要等多久？</summary>
        <div class="faq-body">WSL2 和 Ubuntu 安装受网络速度影响，国内网络下约需 10–20 分钟。如超过 30 分钟无进展，关闭窗口重新运行 <code>一键启动.bat</code>，程序会从断点继续。</div>
      </details>
      <details>
        <summary>打开浏览器后显示「Connection lost」？</summary>
        <div class="faq-body">等待 5–10 秒，页面通常会自动重连。若仍无法使用，关闭浏览器后重新运行 <code>后续启动.bat</code>。</div>
      </details>
      <details>
        <summary>显示「Cross-origin request rejected」？</summary>
        <div class="faq-body">这是浏览器缓存导致的 origin 不匹配。解决方法：改用无痕模式打开（Chrome / Edge: <code>Ctrl+Shift+N</code>），访问 <code>http://127.0.0.1:8787</code>。</div>
      </details>
      <details>
        <summary>忘记 API Key 了，怎么修改？</summary>
        <div class="faq-body">重新运行 <code>一键启动.bat</code>，程序检测到已安装后会询问是否重新配置 API Key，选「是」即可。</div>
      </details>
      <details>
        <summary>想添加 Kimi / 自定义模型怎么操作？</summary>
        <div class="faq-body">在 WebUI 中进入 <strong>Settings → Profiles → New Profile</strong>，填写 Base URL（如 <code>https://api.moonshot.cn/v1</code>）和对应的 API Key 即可，无需修改任何配置文件。</div>
      </details>
      <details>
        <summary>想完全卸载怎么办？</summary>
        <div class="faq-body">打开 PowerShell（管理员），执行 <code>wsl --unregister Ubuntu</code>，然后在「设置 → 应用」中卸载「适用于 Linux 的 Windows 子系统」即可彻底删除。</div>
      </details>
    </div>
  </div>
</section>

<hr class="divider" />

<!-- ══ CTA ═════════════════════════════════════════════════ -->
<section style="text-align:center;padding:5rem 1.5rem;">
  <div class="container">
    <h2 style="font-size:clamp(1.8rem,4vw,2.6rem);font-weight:800;margin-bottom:1rem;">
      准备好了吗？
    </h2>
    <p style="color:var(--muted);margin-bottom:2.5rem;font-size:1rem;">
      双击运行，10 分钟后你就可以和 Hermes AI 对话了
    </p>
    <div style="display:flex;gap:1rem;justify-content:center;flex-wrap:wrap;">
      <a href="https://github.com/qinghuanandejiangshi/Hermes-one-clink" class="btn btn-primary" target="_blank">⬇️ 下载 / GitHub</a>
      <a href="https://github.com/qinghuanandejiangshi/Hermes-one-clink/blob/main/%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E%EF%BC%88%E5%B0%8F%E7%99%BD%E7%89%88%EF%BC%89.md.md" class="btn btn-ghost">📖 查看完整使用说明</a>
    </div>
  </div>
</section>

<!-- ══ FOOTER ════════════════════════════════════════════════ -->
<footer>
  <p>
    基于 <a href="https://github.com/NousResearch/hermes-agent" target="_blank">Hermes Agent</a>
    和 <a href="https://github.com/nesquena/hermes-webui" target="_blank">Hermes WebUI</a> 构建 ·
    MIT License
  </p>
  <p style="margin-top:.5rem;color:#4a5068;font-size:0.8rem;">
    灵感来源于 <a href="https://github.com/dongsheng123132/u-claw" target="_blank">u-claw</a> 项目
  </p>
</footer>

</body>
</html>
