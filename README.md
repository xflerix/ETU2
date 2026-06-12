<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Эмоциональный компас</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;0,600;1,300;1,400&family=Inter:wght@300;400;500&display=swap');

  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --bg: #0e0f14;
    --surface: #161720;
    --border: #2a2c3a;
    --text: #e8e6f0;
    --muted: #6b6880;
    --accent: #9b7fe8;
    --accent-glow: rgba(155,127,232,0.15);
  }

  body {
    font-family: 'Inter', sans-serif;
    background: var(--bg);
    color: var(--text);
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  header {
    width: 100%;
    max-width: 900px;
    padding: 48px 24px 0;
    text-align: center;
  }

  .eyebrow {
    font-size: 11px;
    letter-spacing: 0.22em;
    text-transform: uppercase;
    color: var(--accent);
    margin-bottom: 18px;
  }

  h1 {
    font-family: 'Cormorant Garamond', serif;
    font-size: clamp(38px, 7vw, 64px);
    font-weight: 300;
    line-height: 1.05;
    letter-spacing: -0.01em;
    color: var(--text);
  }

  h1 em {
    font-style: italic;
    color: var(--accent);
  }

  .subtitle {
    margin-top: 16px;
    font-size: 15px;
    font-weight: 300;
    color: var(--muted);
    max-width: 420px;
    margin-left: auto;
    margin-right: auto;
    line-height: 1.6;
  }

  .step-line {
    width: 1px;
    height: 40px;
    background: linear-gradient(to bottom, transparent, var(--border));
    margin: 0 auto;
  }

  /* === EMOTION GRID === */
  .section {
    width: 100%;
    max-width: 900px;
    padding: 0 24px;
    margin-top: 32px;
  }

  .section-label {
    font-size: 11px;
    letter-spacing: 0.18em;
    text-transform: uppercase;
    color: var(--muted);
    margin-bottom: 20px;
  }

  .emotion-clusters {
    display: flex;
    flex-direction: column;
    gap: 24px;
  }

  .cluster-row {
    display: flex;
    flex-direction: column;
    gap: 10px;
  }

  .cluster-name {
    font-family: 'Cormorant Garamond', serif;
    font-size: 13px;
    font-weight: 400;
    letter-spacing: 0.06em;
    color: var(--muted);
    text-transform: uppercase;
  }

  .emotion-pills {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
  }

  .pill {
    padding: 8px 16px;
    border-radius: 100px;
    border: 1px solid var(--border);
    background: var(--surface);
    font-size: 13px;
    font-weight: 400;
    color: var(--muted);
    cursor: pointer;
    transition: all 0.18s ease;
    user-select: none;
  }

  .pill:hover {
    border-color: var(--accent);
    color: var(--text);
    background: var(--accent-glow);
  }

  .pill.selected {
    border-color: var(--accent);
    background: var(--accent-glow);
    color: var(--accent);
    box-shadow: 0 0 12px rgba(155,127,232,0.2);
  }

  /* === INTENSITY SLIDER === */
  .intensity-block {
    margin-top: 32px;
    padding: 24px;
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 16px;
    display: none;
  }

  .intensity-block.visible { display: block; }

  .intensity-label {
    font-size: 13px;
    color: var(--muted);
    margin-bottom: 16px;
  }

  .intensity-label span {
    color: var(--accent);
    font-weight: 500;
  }

  input[type=range] {
    width: 100%;
    -webkit-appearance: none;
    appearance: none;
    height: 3px;
    background: var(--border);
    border-radius: 2px;
    outline: none;
    cursor: pointer;
  }

  input[type=range]::-webkit-slider-thumb {
    -webkit-appearance: none;
    width: 18px;
    height: 18px;
    border-radius: 50%;
    background: var(--accent);
    box-shadow: 0 0 10px rgba(155,127,232,0.5);
    cursor: pointer;
  }

  .intensity-markers {
    display: flex;
    justify-content: space-between;
    margin-top: 8px;
    font-size: 11px;
    color: var(--muted);
  }

  /* === CONTEXT TEXTAREA === */
  .context-block {
    margin-top: 20px;
    display: none;
  }

  .context-block.visible { display: block; }

  textarea {
    width: 100%;
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 16px;
    color: var(--text);
    font-family: 'Inter', sans-serif;
    font-size: 14px;
    font-weight: 300;
    line-height: 1.6;
    resize: none;
    outline: none;
    transition: border-color 0.2s;
  }

  textarea:focus { border-color: var(--accent); }
  textarea::placeholder { color: var(--muted); }

  /* === BUTTON === */
  .analyze-btn {
    display: none;
    margin: 32px auto 0;
    padding: 16px 40px;
    background: var(--accent);
    color: #fff;
    border: none;
    border-radius: 100px;
    font-family: 'Inter', sans-serif;
    font-size: 14px;
    font-weight: 500;
    letter-spacing: 0.04em;
    cursor: pointer;
    transition: all 0.2s;
    box-shadow: 0 0 24px rgba(155,127,232,0.3);
  }

  .analyze-btn:hover {
    transform: translateY(-1px);
    box-shadow: 0 0 36px rgba(155,127,232,0.5);
  }

  .analyze-btn.visible { display: block; }
  .analyze-btn:disabled { opacity: 0.5; cursor: not-allowed; transform: none; }

  /* === RESULT === */
  .result-block {
    width: 100%;
    max-width: 900px;
    padding: 0 24px;
    margin-top: 48px;
    display: none;
    animation: fadeUp 0.5s ease both;
  }

  .result-block.visible { display: block; }

  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(16px); }
    to   { opacity: 1; transform: translateY(0); }
  }

  .result-card {
    border: 1px solid var(--border);
    border-radius: 20px;
    overflow: hidden;
  }

  .result-header {
    padding: 28px 32px;
    background: linear-gradient(135deg, rgba(155,127,232,0.12), rgba(155,127,232,0.04));
    border-bottom: 1px solid var(--border);
  }

  .result-tag {
    font-size: 11px;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    color: var(--accent);
    margin-bottom: 12px;
  }

  .result-title {
    font-family: 'Cormorant Garamond', serif;
    font-size: 28px;
    font-weight: 300;
    line-height: 1.2;
  }

  .result-body {
    padding: 32px;
    display: flex;
    flex-direction: column;
    gap: 28px;
  }

  .result-section h3 {
    font-family: 'Cormorant Garamond', serif;
    font-size: 18px;
    font-weight: 400;
    color: var(--accent);
    margin-bottom: 12px;
    letter-spacing: 0.02em;
  }

  .result-section p {
    font-size: 14px;
    font-weight: 300;
    line-height: 1.75;
    color: #c8c5d8;
  }

  .practice-box {
    background: var(--surface);
    border: 1px solid var(--border);
    border-left: 3px solid var(--accent);
    border-radius: 0 12px 12px 0;
    padding: 20px 24px;
  }

  .practice-box p {
    font-size: 14px;
    font-weight: 300;
    line-height: 1.75;
    color: #c8c5d8;
  }

  .divider {
    width: 100%;
    height: 1px;
    background: var(--border);
  }

  /* === LOADING === */
  .loading-state {
    display: none;
    align-items: center;
    gap: 12px;
    color: var(--muted);
    font-size: 13px;
    margin: 32px auto 0;
    width: fit-content;
  }

  .loading-state.visible { display: flex; }

  .dots {
    display: flex;
    gap: 4px;
  }

  .dot {
    width: 6px;
    height: 6px;
    border-radius: 50%;
    background: var(--accent);
    animation: pulse 1.2s ease infinite;
  }

  .dot:nth-child(2) { animation-delay: 0.2s; }
  .dot:nth-child(3) { animation-delay: 0.4s; }

  @keyframes pulse {
    0%, 80%, 100% { opacity: 0.2; transform: scale(0.8); }
    40% { opacity: 1; transform: scale(1); }
  }

  /* === RESET === */
  .reset-btn {
    display: none;
    margin: 20px auto 60px;
    background: none;
    border: 1px solid var(--border);
    color: var(--muted);
    padding: 10px 28px;
    border-radius: 100px;
    font-family: 'Inter', sans-serif;
    font-size: 13px;
    cursor: pointer;
    transition: all 0.2s;
  }

  .reset-btn:hover { border-color: var(--accent); color: var(--text); }
  .reset-btn.visible { display: block; }

  footer {
    padding: 40px 24px;
    text-align: center;
    font-size: 12px;
    color: var(--muted);
  }

  /* Mood constellation — signature visual */
  .constellation {
    width: 100%;
    height: 80px;
    margin: 0 auto 8px;
    position: relative;
  }

  /* responsive */
  @media (max-width: 600px) {
    .result-body { padding: 20px; }
    .result-header { padding: 20px; }
  }
</style>
</head>
<body>

<header>
  <p class="eyebrow">Эмоциональный компас</p>
  <h1>Что ты<br>сейчас <em>чувствуешь?</em></h1>
  <p class="subtitle">Выбери эмоции, которые резонируют прямо сейчас. Нет правильных ответов.</p>
</header>

<div class="step-line" style="margin-top:36px"></div>

<div class="section">
  <p class="section-label">Выбери одну или несколько эмоций</p>

  <div class="emotion-clusters" id="emotionGrid">

    <div class="cluster-row">
      <span class="cluster-name">Радость</span>
      <div class="emotion-pills">
        <span class="pill" data-cluster="joy">Счастье</span>
        <span class="pill" data-cluster="joy">Восторг</span>
        <span class="pill" data-cluster="joy">Благодарность</span>
        <span class="pill" data-cluster="joy">Умиротворение</span>
        <span class="pill" data-cluster="joy">Воодушевление</span>
        <span class="pill" data-cluster="joy">Нежность</span>
      </div>
    </div>

    <div class="cluster-row">
      <span class="cluster-name">Тревога и страх</span>
      <div class="emotion-pills">
        <span class="pill" data-cluster="fear">Тревога</span>
        <span class="pill" data-cluster="fear">Страх</span>
        <span class="pill" data-cluster="fear">Беспокойство</span>
        <span class="pill" data-cluster="fear">Неуверенность</span>
        <span class="pill" data-cluster="fear">Паника</span>
        <span class="pill" data-cluster="fear">Застылость</span>
      </div>
    </div>

    <div class="cluster-row">
      <span class="cluster-name">Грусть</span>
      <div class="emotion-pills">
        <span class="pill" data-cluster="sadness">Тоска</span>
        <span class="pill" data-cluster="sadness">Разочарование</span>
        <span class="pill" data-cluster="sadness">Одиночество</span>
        <span class="pill" data-cluster="sadness">Грусть</span>
        <span class="pill" data-cluster="sadness">Горе</span>
        <span class="pill" data-cluster="sadness">Усталость</span>
      </div>
    </div>

    <div class="cluster-row">
      <span class="cluster-name">Злость</span>
      <div class="emotion-pills">
        <span class="pill" data-cluster="anger">Раздражение</span>
        <span class="pill" data-cluster="anger">Злость</span>
        <span class="pill" data-cluster="anger">Обида</span>
        <span class="pill" data-cluster="anger">Ярость</span>
        <span class="pill" data-cluster="anger">Разочарование в других</span>
        <span class="pill" data-cluster="anger">Зависть</span>
      </div>
    </div>

    <div class="cluster-row">
      <span class="cluster-name">Стыд и вина</span>
      <div class="emotion-pills">
        <span class="pill" data-cluster="shame">Стыд</span>
        <span class="pill" data-cluster="shame">Вина</span>
        <span class="pill" data-cluster="shame">Неловкость</span>
        <span class="pill" data-cluster="shame">Смущение</span>
        <span class="pill" data-cluster="shame">Самокритика</span>
      </div>
    </div>

    <div class="cluster-row">
      <span class="cluster-name">Любопытство и интерес</span>
      <div class="emotion-pills">
        <span class="pill" data-cluster="curiosity">Любопытство</span>
        <span class="pill" data-cluster="curiosity">Интерес</span>
        <span class="pill" data-cluster="curiosity">Предвкушение</span>
        <span class="pill" data-cluster="curiosity">Удивление</span>
        <span class="pill" data-cluster="curiosity">Вдохновение</span>
      </div>
    </div>

    <div class="cluster-row">
      <span class="cluster-name">Смешанные</span>
      <div class="emotion-pills">
        <span class="pill" data-cluster="mixed">Опустошённость</span>
        <span class="pill" data-cluster="mixed">Онемение</span>
        <span class="pill" data-cluster="mixed">Ностальгия</span>
        <span class="pill" data-cluster="mixed">Амбивалентность</span>
        <span class="pill" data-cluster="mixed">Меланхолия</span>
        <span class="pill" data-cluster="mixed">Тихая радость</span>
      </div>
    </div>

  </div>

  <!-- Intensity -->
  <div class="intensity-block" id="intensityBlock">
    <p class="intensity-label">Насколько сильно ты это ощущаешь? <span id="intensityVal">умеренно</span></p>
    <input type="range" id="intensitySlider" min="1" max="5" value="3">
    <div class="intensity-markers">
      <span>едва заметно</span>
      <span>умеренно</span>
      <span>очень сильно</span>
    </div>
  </div>

  <!-- Context -->
  <div class="context-block" id="contextBlock">
    <textarea id="contextText" rows="3" placeholder="Хочешь добавить контекст? Что произошло, о чём думаешь... (необязательно)"></textarea>
  </div>

  <!-- Button -->
  <button class="analyze-btn" id="analyzeBtn" onclick="analyze()">
    Исследовать моё состояние →
  </button>

  <!-- Loading -->
  <div class="loading-state" id="loadingState">
    <div class="dots">
      <div class="dot"></div>
      <div class="dot"></div>
      <div class="dot"></div>
    </div>
    <span>Анализирую твоё состояние…</span>
  </div>

</div>

<!-- Result -->
<div class="result-block" id="resultBlock">
  <div class="result-card">
    <div class="result-header">
      <p class="result-tag">Твоё состояние</p>
      <div class="result-title" id="resultTitle">—</div>
    </div>
    <div class="result-body">
      <div class="result-section">
        <h3>Что это говорит о тебе</h3>
        <p id="resultInsight">—</p>
      </div>
      <div class="divider"></div>
      <div class="result-section">
        <h3>Практика прямо сейчас</h3>
        <div class="practice-box">
          <p id="resultPractice">—</p>
        </div>
      </div>
      <div class="result-section">
        <h3>Вопрос для размышления</h3>
        <p id="resultQuestion" style="font-style:italic; color: var(--accent)">—</p>
      </div>
    </div>
  </div>
</div>

<button class="reset-btn" id="resetBtn" onclick="resetAll()">← Начать снова</button>

<footer>
  Это инструмент самоисследования, не диагностика.<br>
  Если тебе тяжело — поговори с близким или специалистом.
  <div style="margin-top: 24px; padding-top: 20px; border-top: 1px solid #2a2c3a; font-family: 'Cormorant Garamond', serif; font-style: italic; font-size: 15px; color: #9b7fe8; letter-spacing: 0.03em;">
    Made with care, especially for Aziza ✦
  </div>
</footer>

<script>
  const selected = new Set();
  const intensityLabels = ['', 'едва заметно', 'слегка', 'умеренно', 'сильно', 'очень сильно'];

  // Pill clicks
  document.querySelectorAll('.pill').forEach(pill => {
    pill.addEventListener('click', () => {
      pill.classList.toggle('selected');
      if (pill.classList.contains('selected')) {
        selected.add(pill.textContent.trim());
      } else {
        selected.delete(pill.textContent.trim());
      }
      updateUI();
    });
  });

  // Slider
  document.getElementById('intensitySlider').addEventListener('input', function() {
    document.getElementById('intensityVal').textContent = intensityLabels[+this.value];
  });

  function updateUI() {
    const hasSelection = selected.size > 0;
    document.getElementById('intensityBlock').classList.toggle('visible', hasSelection);
    document.getElementById('contextBlock').classList.toggle('visible', hasSelection);
    document.getElementById('analyzeBtn').classList.toggle('visible', hasSelection);
  }

  async function analyze() {
    const emotions = Array.from(selected).join(', ');
    const intensity = intensityLabels[+document.getElementById('intensitySlider').value];
    const context = document.getElementById('contextText').value.trim();

    document.getElementById('analyzeBtn').disabled = true;
    document.getElementById('loadingState').classList.add('visible');
    document.getElementById('resultBlock').classList.remove('visible');
    document.getElementById('resetBtn').classList.remove('visible');

    const prompt = `Ты мягкий, проницательный психолог, работающий в стиле гуманистической психологии (Карл Роджерс, Виктор Франкл).

Пользователь сообщает, что прямо сейчас испытывает: ${emotions}.
Интенсивность: ${intensity}.
${context ? `Контекст от пользователя: ${context}` : ''}

Дай ответ строго в формате JSON (никакого другого текста):
{
  "title": "Краткое поэтическое название состояния (3-6 слов, на русском)",
  "insight": "2-3 абзаца глубокого, тёплого психологического инсайта. Нормализуй эмоции, найди их функцию и смысл. Не оценивай. Говори с уважением и теплотой.",
  "practice": "Конкретная практика на 5-10 минут прямо сейчас — телесная, дыхательная или когнитивная. Пошагово, без воды.",
  "question": "Один глубокий открытый вопрос для самоисследования. Не риторический, а настоящий."
}`;

    try {
      const response = await fetch("https://api.anthropic.com/v1/messages", {
        method: "POST",
        headers: { "Content-Type": "application/json", "anthropic-dangerous-direct-browser-access": "true" },
        body: JSON.stringify({
          model: "claude-sonnet-4-6",
          max_tokens: 1000,
          messages: [{ role: "user", content: prompt }]
        })
      });

      if (!response.ok) {
        const errText = await response.text();
        throw new Error('HTTP ' + response.status + ': ' + errText);
      }

      const data = await response.json();
      console.log('API response:', JSON.stringify(data));

      if (!data.content || !Array.isArray(data.content)) {
        throw new Error('No content in response: ' + JSON.stringify(data));
      }

      const text = data.content
        .filter(b => b.type === 'text')
        .map(b => b.text)
        .join('');

      console.log('Raw text:', text);

      // Extract JSON from response
      const jsonMatch = text.match(/\{[\s\S]*\}/);
      if (!jsonMatch) throw new Error('No JSON found in: ' + text);

      const parsed = JSON.parse(jsonMatch[0]);

      document.getElementById('resultTitle').textContent = parsed.title || '—';
      document.getElementById('resultInsight').textContent = parsed.insight || '—';
      document.getElementById('resultPractice').textContent = parsed.practice || '—';
      document.getElementById('resultQuestion').textContent = parsed.question || '—';

      document.getElementById('resultBlock').classList.add('visible');
      document.getElementById('resetBtn').classList.add('visible');
      document.getElementById('resultBlock').scrollIntoView({ behavior: 'smooth', block: 'start' });

    } catch (e) {
      console.error('Full error:', e);
      document.getElementById('resultTitle').textContent = 'Что-то пошло не так';
      document.getElementById('resultInsight').textContent = 'Ошибка: ' + e.message;
      document.getElementById('resultPractice').textContent = '';
      document.getElementById('resultQuestion').textContent = '';
      document.getElementById('resultBlock').classList.add('visible');
      document.getElementById('resetBtn').classList.add('visible');
    } finally {
      document.getElementById('loadingState').classList.remove('visible');
      document.getElementById('analyzeBtn').disabled = false;
    }
  }

  function resetAll() {
    selected.clear();
    document.querySelectorAll('.pill').forEach(p => p.classList.remove('selected'));
    document.getElementById('intensitySlider').value = 3;
    document.getElementById('intensityVal').textContent = 'умеренно';
    document.getElementById('contextText').value = '';
    document.getElementById('intensityBlock').classList.remove('visible');
    document.getElementById('contextBlock').classList.remove('visible');
    document.getElementById('analyzeBtn').classList.remove('visible');
    document.getElementById('analyzeBtn').disabled = false;
    document.getElementById('loadingState').classList.remove('visible');
    document.getElementById('resultBlock').classList.remove('visible');
    document.getElementById('resetBtn').classList.remove('visible');
    window.scrollTo({ top: 0, behavior: 'smooth' });
  }
</script>
</body>
</html>

