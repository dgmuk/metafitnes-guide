# ПРОМПТ ДЛЯ CLAUDE CODE
# Лид-магнит 2: Гайд по построению тренировочного процесса
# Копируй и вставляй целиком

---

Создай одностраничный лендинг index.html — лид-магнит для фитнес-тренера Дмитрия Мухина. Это гайд по построению годового тренировочного процесса, который одновременно даёт пользу читателю и показывает тренера как эксперта. Всё в одном файле: HTML + CSS + JS. Никаких фреймворков и внешних зависимостей кроме Google Fonts.

---

## ДИЗАЙН-СИСТЕМА — ИСПОЛЬЗОВАТЬ ТОЧНО

```css
:root {
  --bg-main:        #0d0d0d;
  --bg-card:        #141414;
  --bg-elevated:    #1a1a1a;
  --bg-section:     #111111;
  --accent:         #c8f542;
  --accent-dim:     rgba(200, 245, 66, 0.10);
  --accent-glow:    rgba(200, 245, 66, 0.04);
  --text-primary:   #f0f0ee;
  --text-secondary: #8a8a82;
  --text-muted:     #444440;
  --border:         rgba(255,255,255,0.07);
  --border-accent:  rgba(200, 245, 66, 0.20);
  --border-strong:  rgba(255,255,255,0.12);
  --font-display:   'Unbounded', sans-serif;
  --font-body:      'Golos Text', sans-serif;
  --radius:         12px;
  --radius-lg:      20px;
  --radius-xl:      28px;
  --max-width:      720px;
  --max-width-wide: 960px;
}
```

Шрифты подключить в head:
```html
<link href="https://fonts.googleapis.com/css2?family=Unbounded:wght@400;600;700;900&family=Golos+Text:wght@400;500;600&display=swap" rel="stylesheet">
```

Общие правила стиля:
- Тёмная тема, чёрный фон, акцент #c8f542 (лаймовый)
- Unbounded для заголовков, Golos Text для текста
- Карточки с border: 1px solid var(--border), background: var(--bg-card)
- При hover на карточках: border-color переходит в var(--border-accent)
- Все секции с padding: 80px 20px на десктопе, 48px 20px на мобильном

---

## ТЕХНИЧЕСКИЕ ТРЕБОВАНИЯ

**Анимации при скролле (обязательно для всех блоков):**
```javascript
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('visible');
    }
  });
}, { threshold: 0.1 });
document.querySelectorAll('.fade-in').forEach(el => observer.observe(el));
```
```css
.fade-in { opacity: 0; transform: translateY(24px); transition: opacity 0.6s ease, transform 0.6s ease; }
.fade-in.visible { opacity: 1; transform: translateY(0); }
.fade-in:nth-child(2) { transition-delay: 0.1s; }
.fade-in:nth-child(3) { transition-delay: 0.2s; }
.fade-in:nth-child(4) { transition-delay: 0.3s; }
```

**Sticky navbar с blur при скролле:**
```javascript
window.addEventListener('scroll', () => {
  document.getElementById('navbar').classList.toggle('scrolled', window.scrollY > 50);
});
```
```css
#navbar { position: sticky; top: 0; z-index: 100; transition: background 0.3s, backdrop-filter 0.3s; }
#navbar.scrolled { background: rgba(13,13,13,0.92); backdrop-filter: blur(12px); border-bottom: 1px solid var(--border); }
```

**Плавный скролл к якорям:**
```javascript
document.querySelectorAll('a[href^="#"]').forEach(a => {
  a.addEventListener('click', e => {
    e.preventDefault();
    const target = document.querySelector(a.getAttribute('href'));
    if (target) target.scrollIntoView({ behavior: 'smooth' });
  });
});
```

**Мобильная адаптация:** все сетки переключаются в 1 колонку на ширине < 640px. Шрифты масштабируются через clamp(). Тапы работают так же как клики.

---

## СТРУКТУРА СТРАНИЦЫ — 13 БЛОКОВ

### БЛОК 1: НАВИГАЦИЯ

Sticky. Логотип слева, кнопка справа.

```html
<nav id="navbar">
  <div class="nav-inner">
    <a href="/" class="logo">
      <div class="logo-mark">ДМ</div>
      <div class="logo-text">
        <span class="logo-name">Дмитрий Мухин</span>
        <span class="logo-tagline">Тренер · 8 лет опыта</span>
      </div>
    </a>
    <a href="https://t.me/dgmukhin_adm" class="btn-nav" target="_blank">Написать →</a>
  </div>
</nav>
```

Стили logo-mark: 40x40px, background: var(--accent), border-radius: 10px, цвет текста #0d0d0d, шрифт Unbounded 700.
Кнопка btn-nav: border: 1px solid var(--border-accent), color: var(--accent), background: var(--accent-dim), border-radius: 100px, padding: 8px 18px, font-size: 13px.

---

### БЛОК 2: HERO (первый экран, min-height: 100vh)

Центрированный контент. Фоновый CSS паттерн сетки:
```css
.hero {
  background-image:
    linear-gradient(rgba(200,245,66,0.03) 1px, transparent 1px),
    linear-gradient(90deg, rgba(200,245,66,0.03) 1px, transparent 1px);
  background-size: 40px 40px;
  background-position: center center;
}
/* Градиент снизу перекрывает паттерн */
.hero::after {
  content: '';
  position: absolute;
  bottom: 0; left: 0; right: 0;
  height: 200px;
  background: linear-gradient(transparent, var(--bg-main));
}
```

Содержимое по центру:
```
[лейбл] НАУЧНЫЙ ПОДХОД К ТРЕНИРОВКАМ

[h1] Почему ты тренируешься
     и не получаешь результат

[подзаголовок] Полный разбор системы построения тренировочного
процесса на год вперёд. Как думает опытный тренер когда
составляет программу, и почему случайный набор упражнений
никогда не даст того что ты хочешь.

[три факта в строку]
⏱ 15 минут чтения    🧬 Научная база 2024    💡 Всё по делу

[кнопка] Начать читать ↓   (скролл к id="problem")
```

Стили h1: Unbounded 900, font-size: clamp(28px, 6vw, 52px), letter-spacing: -0.02em, color: var(--text-primary).
Лейбл: font-size: 11px, letter-spacing: 0.12em, text-transform: uppercase, color: var(--accent), margin-bottom: 20px.
Факты: display: flex, gap: 24px, font-size: 14px, color: var(--text-secondary).
Основная кнопка: background: var(--accent), color: #0d0d0d, font-weight: 700, padding: 14px 32px, border-radius: 100px.

---

### БЛОК 3: ПРОБЛЕМА (id="problem")

Фон: var(--bg-section).

Заголовок: "Знакомая картина?"

Три карточки в сетке 3 колонки (на мобильном 1 колонка). Каждая карточка:
- Иконка вверху (большой эмодзи, font-size: 32px)
- Заголовок карточки (жирный, 16px)
- Текст описания (var(--text-secondary), 15px)

Карточка 1 (🔄): "Меняешь программы каждые два месяца" / "Берёшь новую программу потому что старая перестала работать. Меняешь упражнения, добавляешь подходы. Топчешься на месте."

Карточка 2 (📉): "Делаешь всё правильно, но прогресса нет" / "Ходишь в зал регулярно, не пропускаешь, стараешься. И всё равно смотришь на те же результаты что год назад."

Карточка 3 (🤷): "Непонятно сколько и как тренироваться" / "Один говорит делать 5 подходов. Другой 20. Кто-то советует каждый день. Непонятно кому верить."

Текст после карточек (max-width: var(--max-width), по центру):
"Проблема не в том что ты ленишься или делаешь что-то принципиально неправильно. Проблема в том, что у тебя нет системы. А без системы даже правильные действия дают случайный результат. Дальше я объясню как эта система устроена."

---

### БЛОК 4: ШАГ 0 — ОПРЕДЕЛИТЬ ЦЕЛЬ

Заголовок: "Шаг 0. Без этого ничего не работает"

Вводный текст: "Прежде чем говорить о подходах, повторениях и программах, нужно ответить на один вопрос. Чего именно ты хочешь? От ответа зависит буквально всё: как строится программа, сколько нужно тренироваться, как считается нагрузка и что считать прогрессом."

Четыре раскрывающиеся карточки целей (аккордеон). При клике на карточку она раскрывается, остальные закрываются.

JavaScript для аккордеона:
```javascript
function toggleGoal(card) {
  const isActive = card.classList.contains('active');
  document.querySelectorAll('.goal-card').forEach(c => c.classList.remove('active'));
  if (!isActive) card.classList.add('active');
}
```

CSS для аккордеона:
```css
.goal-body { max-height: 0; overflow: hidden; transition: max-height 0.4s ease; }
.goal-card.active .goal-body { max-height: 200px; }
.goal-arrow { transition: transform 0.3s; }
.goal-card.active .goal-arrow { transform: rotate(180deg); }
```

Карточка 1 — НАБОР МЫШЕЧНОЙ МАССЫ 💪:
"Прогрессивная перегрузка, умеренный профицит калорий, объём тренировок в зоне MAV, правильная периодизация нагрузки. Программа строится на 3–6 месяцев с чёткими этапами накопления и восстановления."

Карточка 2 — СНИЖЕНИЕ ЖИРА 🔥:
"Дефицит калорий 300–500 ккал, высокий белок (2.2–2.5 г/кг), силовые тренировки как приоритет над кардио. Объём чуть ниже обычного, но интенсивность сохраняется. Плюс рефиды каждые 10 дней."

Карточка 3 — РАЗВИТИЕ СИЛЫ 🏋️:
"Блоковая периодизация, работа в диапазоне 85–100% от максимума, меньший объём и больший отдых между подходами. Нейромышечная адаптация идёт иначе чем при наборе массы."

Карточка 4 — ОБЩАЯ ФОРМА И ЗДОРОВЬЕ ⚡:
"Сбалансированный подход, фулбади 3 раза в неделю, умеренный объём, больше внимания к технике и восстановлению. Цель не рекорды, а стабильный долгосрочный прогресс."

Текст после карточек: "Один и тот же человек с разными целями получает принципиально разные программы. Это не маркетинг, это физиология."

---

### БЛОК 5: ИНТЕРАКТИВ 1 — КАЛЬКУЛЯТОР УРОВНЯ

Заголовок: "Определи свой уровень за 2 минуты"
Подзаголовок: "От уровня зависит стартовый объём тренировок и скорость прогрессии."

Вопросы оформить как стилизованные radio-группы (не стандартные input, а кастомные карточки-чекбоксы).

```css
.radio-option { display: block; padding: 12px 16px; border: 1px solid var(--border); border-radius: var(--radius); cursor: pointer; transition: border-color 0.2s, background 0.2s; }
.radio-option:hover { border-color: var(--border-strong); background: var(--bg-elevated); }
.radio-option.selected { border-color: var(--accent); background: var(--accent-dim); color: var(--text-primary); }
```

Вопрос 1 (data-points): "Как давно тренируешься регулярно?"
- "Менее 1 года" → 0
- "1–3 года" → 2
- "Более 3 лет" → 4

Вопрос 2: "Присед со штангой — сколько делаешь?"
- "Не делаю / вес тела" → 0
- "До 1 своего веса" → 1
- "1–1.5 своего веса" → 2
- "Более 1.5 своего веса" → 3

Вопрос 3: "Как давно не было заметного прогресса?"
- "Прогресс есть, растут веса" → 0
- "Несколько месяцев" → 2
- "Больше полугода" → 3

Вопрос 4: "Сколько дней в неделю тренируешься?"
- "1–2 дня" → 0
- "3–4 дня" → 1
- "5+ дней" → 2

Кнопка "Узнать результат" — по клику считает сумму очков и показывает нужный блок результата (скрыт по умолчанию).

```javascript
function calcLevel() {
  let total = 0;
  document.querySelectorAll('.radio-option.selected').forEach(opt => {
    total += parseInt(opt.dataset.points || 0);
  });
  document.querySelectorAll('.level-result').forEach(r => r.style.display = 'none');
  if (total <= 4) document.getElementById('level-beginner').style.display = 'block';
  else if (total <= 8) document.getElementById('level-intermediate').style.display = 'block';
  else document.getElementById('level-advanced').style.display = 'block';
  document.getElementById('level-results').scrollIntoView({ behavior: 'smooth', block: 'nearest' });
}
```

Три результата (level-result, скрыты изначально):

НОВИЧОК (0–4): "Тебе доступен самый приятный период в тренировках: любой адекватный стимул вызывает рост. Стартовый объём для тебя — 8–12 рабочих подходов на мышечную группу в неделю. Программа простая, линейная прогрессия. Твой приоритет прямо сейчас — освоить технику базовых упражнений и выработать регулярность."

СРЕДНИЙ (5–8): "Самый интересный и самый сложный этап. Тело уже адаптировалось к базовым стимулам и требует системного подхода. Именно здесь большинство застревает на годы. Рабочий объём для тебя — 10–18 подходов на группу, волнообразная периодизация, правильные мезоциклы. Без системы прогресс практически останавливается."

ПРОДВИНУТЫЙ (9+): "Ты достиг точки где случайные тренировки не работают совсем. Нужна точная настройка объёма (16–22 подхода на группу), периодизация нагрузки, контроль маркеров восстановления. Каждый следующий процент прогресса требует значительно больше усилий и точности."

Стиль блоков результата: border-left: 3px solid var(--accent), padding: 20px 24px, background: var(--bg-elevated), border-radius: 0 var(--radius) var(--radius) 0.

Текст под результатами: "Это общий ориентир. Реальный расчёт учитывает ещё десяток параметров: сон, стресс, тип работы, наличие травм, цель и доступное оборудование."

---

### БЛОК 6: ГОДОВОЙ МАКРОЦИКЛ

Заголовок: "Как устроен год тренировок"
Вводный текст: "Профессиональный тренинг устроен как архитектурный проект: сначала фундамент, потом стены, потом отделка. Каждый этап готовит почву для следующего. Это называется периодизация."

Визуальная горизонтальная шкала года (4 цветных сегмента):
```css
.year-timeline { display: flex; gap: 4px; height: 12px; border-radius: 6px; overflow: hidden; margin: 32px 0; }
.year-segment { flex: 1; background: var(--bg-elevated); border: 1px solid var(--border); cursor: pointer; transition: background 0.2s; }
.year-segment.active { background: var(--accent); }
```
Сегменты с метками под ними: "Адаптация Нед 1–10", "Основной рост Нед 11–28", "Сила Нед 29–40", "Реализация Нед 41–52".

При клике на сегмент — показывается соответствующий блок описания.

Описания четырёх блоков (показывать/скрывать при клике на сегмент):

Адаптация: "Самый важный и самый недооценённый этап. Тело учится работать с нагрузкой: суставы, сухожилия, нервная система. Объём начинается с минимального эффективного и плавно нарастает. Те кто пропускает этот этап и сразу бомбит с максимальной нагрузки, потом удивляются болям и застою через 2 месяца."

Основной рост: "Главный рабочий период. Объём нагрузки находится в зоне MAV для каждой мышечной группы. Диапазоны повторений меняются от тренировки к тренировке. Каждые 4–5 недель идёт делоад — неделя снижения нагрузки для суперкомпенсации."

Сила: "Накопленная масса конвертируется в силу. Работа с весами 80–95% от максимума, меньший объём, больший отдых. После этого блока возвращение к гипертрофийному тренингу даст значительно больший отклик."

Реализация: "Закрепление результатов, переходный период, оценка прогресса за год. Конец года это начало нового цикла, и он всегда начинается на более высоком уровне."

---

### БЛОК 7: МЕЗОЦИКЛ

Заголовок: "Внутри каждого блока: мезоцикл"
Вводный текст: "Каждый большой блок состоит из мезоциклов. Мезоцикл это 4–5 недель тренировок с определённой логикой нагрузки. Не случайный набор недель, а управляемое нарастание стимула с обязательным восстановлением."

Визуальная диаграмма баров (5 штук, нарастающая высота):
```html
<div class="meso-chart">
  <div class="meso-bar" style="height: 40px;" data-label="Нед 1" data-zone="MEV"></div>
  <div class="meso-bar" style="height: 60px;" data-label="Нед 2" data-zone="MAV↓"></div>
  <div class="meso-bar" style="height: 80px;" data-label="Нед 3" data-zone="MAV"></div>
  <div class="meso-bar" style="height: 100px;" data-label="Нед 4" data-zone="MRV"></div>
  <div class="meso-bar deload" style="height: 30px;" data-label="Нед 5" data-zone="ДЕЛОАД"></div>
</div>
```
Бары: background: var(--bg-elevated), border: 1px solid var(--border-accent), border-radius: 6px 6px 0 0. Бар делоада: background: var(--accent-dim), border-color: var(--accent).

Таблица под диаграммой:
| Неделя | Тип | Подходы | Интенсивность | RIR |
|--------|-----|---------|---------------|-----|
| 1 | Вводная | MEV (6–8) | 60–65% | 3–4 |
| 2 | Накопление | 8–10 | 65–70% | 2–3 |
| 3 | Накопление | 10–14 | 70–75% | 2 |
| 4 | Пик | 14–20 | 75–85% | 0–1 |
| 5 | Делоад | MV (4–6) | 50–60% | 4–5 |

Стилизованная HTML таблица: thead с background: var(--bg-elevated), td с border-bottom: 1px solid var(--border), строка делоада с color: var(--accent).

Текст после: "Делоад это не потеря времени. Именно в период восстановления происходит суперкомпенсация — тело реализует весь накопленный за цикл прогресс."

---

### БЛОК 8: ИНТЕРАКТИВ 2 — КАЛЬКУЛЯТОР ОБЪЁМА

Заголовок: "Посчитай свой недельный объём"
Подзаголовок: "Введи данные и узнай сколько рабочих подходов нужно делать на каждую мышечную группу в неделю."

Четыре поля формы (стилизованные select и toggle):

```html
<select class="calc-select" id="calc-level">
  <option value="beginner">Новичок (0–1 год)</option>
  <option value="intermediate">Средний (1–3 года)</option>
  <option value="advanced">Продвинутый (3+ лет)</option>
</select>
```

Поля: уровень подготовки, часов сна, уровень стресса, физически тяжёлая работа (да/нет).

Стили select: background: var(--bg-elevated), border: 1px solid var(--border), color: var(--text-primary), padding: 12px 16px, border-radius: var(--radius), width: 100%.

JavaScript расчёт (РЕАЛЬНАЯ ЛОГИКА):
```javascript
const baseMAV = {
  beginner: { chest: 12, back: 12, quads: 10, hams: 8, glutes: 10, delts: 10, biceps: 10, triceps: 10 },
  intermediate: { chest: 15, back: 16, quads: 14, hams: 12, glutes: 12, delts: 15, biceps: 12, triceps: 12 },
  advanced: { chest: 18, back: 20, quads: 16, hams: 14, glutes: 15, delts: 18, biceps: 14, triceps: 14 }
};
const baseMEV = {
  beginner: { chest: 6, back: 8, quads: 6, hams: 4, glutes: 4, delts: 6, biceps: 4, triceps: 4 },
  intermediate: { chest: 8, back: 10, quads: 8, hams: 6, glutes: 5, delts: 8, biceps: 6, triceps: 6 },
  advanced: { chest: 10, back: 12, quads: 10, hams: 8, glutes: 8, delts: 10, biceps: 8, triceps: 8 }
};

function calcVolume() {
  const level = document.getElementById('calc-level').value;
  const sleep = document.getElementById('calc-sleep').value;
  const stress = document.getElementById('calc-stress').value;
  const heavyWork = document.getElementById('calc-work').checked;

  let coeff = 1.0;
  if (sleep === 'under6') coeff *= 0.85;
  else if (sleep === '6to7') coeff *= 0.90;
  if (stress === 'high') coeff *= 0.90;
  if (heavyWork) coeff *= 0.85;

  const muscles = ['chest', 'back', 'quads', 'hams', 'glutes', 'delts', 'biceps', 'triceps'];
  const names = { chest: 'Грудные', back: 'Спина', quads: 'Квадрицепсы', hams: 'Бицепс бедра', glutes: 'Ягодичные', delts: 'Дельты', biceps: 'Бицепс', triceps: 'Трицепс' };

  let tableHTML = '<tr><th>Мышца</th><th>MEV (старт)</th><th>Твой MAV</th><th>Не более</th></tr>';
  muscles.forEach(m => {
    const mev = baseMEV[level][m];
    const mav = Math.round(baseMAV[level][m] * coeff);
    const mrv = Math.round(mav * 1.25);
    tableHTML += `<tr><td>${names[m]}</td><td>${mev}</td><td>${mav}</td><td>${mrv}</td></tr>`;
  });

  document.getElementById('volume-table').innerHTML = tableHTML;
  document.getElementById('volume-result').style.display = 'block';
  document.getElementById('volume-result').scrollIntoView({ behavior: 'smooth', block: 'nearest' });
}
```

Таблица результата (скрыта по умолчанию, id="volume-result"):
Стиль как у таблицы мезоцикла. Строка MAV с акцентным цветом.

Предупреждение под таблицей (стиль как у врезки):
"⚠️ Это общий расчёт без учёта травм, особенностей восстановления и конкретной цели. Реальные цифры в персональной программе будут точнее."

---

### БЛОК 9: ЖЕНСКИЙ ТРЕНИНГ

Заголовок: "Для женщин: как цикл меняет всё"
Вводный текст: "Большинство программ созданы для мужчин и механически применяются к женщинам. Это ошибка, потому что женская физиология принципиально отличается по параметрам которые напрямую влияют на нагрузку и восстановление."

Четыре фазы в виде вертикального таймлайна (на мобильном) / горизонтального (на десктопе):

```css
.cycle-timeline { display: grid; grid-template-columns: repeat(4, 1fr); gap: 16px; }
@media (max-width: 640px) { .cycle-timeline { grid-template-columns: 1fr; } }
.cycle-phase { padding: 20px; background: var(--bg-card); border: 1px solid var(--border); border-radius: var(--radius-lg); }
.cycle-phase-label { font-size: 11px; font-weight: 600; letter-spacing: 0.08em; text-transform: uppercase; color: var(--accent); margin-bottom: 8px; }
```

Фаза 1 — ФОЛЛИКУЛЯРНАЯ (дни 1–13):
"Оптимальное время для максимальных нагрузок. Эстроген ускоряет восстановление, чувствительность к инсулину высокая. Именно сюда ставится пиковая неделя мезоцикла."

Фаза 2 — ОВУЛЯЦИЯ (день 14 ±2):
"Пик эластичности связок. Риск травм коленей выше. Обязательная нейромышечная разминка 10–15 минут, контроль техники в приседаниях."

Фаза 3 — ЛЮТЕИНОВАЯ (дни 15–28):
"Базальная температура выше, восстановление медленнее. Объём снижается на 10–15% при сохранении интенсивности. Углеводы перед тренировкой обязательны."

Фаза 4 — ПРЕ-МЕНСТРУАЛЬНАЯ (дни 25–28):
"Качество сна хуже, ВСР снижена. Идеальное время для делоада. Если делоад совпадает с этой фазой — двойная польза."

---

### БЛОК 10: ИНТЕРАКТИВ 3 — ТЕСТ "КАКАЯ ПРОБЛЕМА"

Заголовок: "Что конкретно мешает твоему прогрессу?"
Подзаголовок: "5 вопросов — и ты поймёшь где главная точка роста."

Каждый вопрос: стилизованные radio-карточки как в блоке 5.

Для каждого варианта ответа добавить data-type="volume|recovery|structure":

В1: "Как давно росли рабочие веса?"
- "Растут каждые 1–2 недели" → data-type="none"
- "Росли 1–3 месяца назад" → data-type="volume"
- "Не помню когда" → data-type="structure"

В2: "Как чувствуешь себя после тренировки?"
- "Нормальная усталость" → data-type="none"
- "Очень тяжело, долго восстанавливаюсь" → data-type="recovery"
- "Почти не чувствую тренировки" → data-type="volume"

В3: "Как спишь?"
- "7–8 часов, качественно" → data-type="none"
- "6–7 часов" → data-type="recovery"
- "Меньше 6 или плохо" → data-type="recovery"

В4: "Есть план на следующие 3 месяца?"
- "Да, чёткий план" → data-type="none"
- "Примерно понимаю" → data-type="structure"
- "Нет, по ситуации" → data-type="structure"

В5: "Что происходит с телом последние 2 месяца?"
- "Есть прогресс" → data-type="none"
- "Стагнация" → data-type="volume"
- "Ухудшение" → data-type="recovery"

JavaScript: считает какой тип встречается чаще, показывает соответствующий результат.

```javascript
function diagnose() {
  const counts = { volume: 0, recovery: 0, structure: 0 };
  document.querySelectorAll('.diag-option.selected').forEach(opt => {
    const type = opt.dataset.type;
    if (counts[type] !== undefined) counts[type]++;
  });
  const max = Object.entries(counts).sort((a,b) => b[1]-a[1])[0][0];
  document.querySelectorAll('.diag-result').forEach(r => r.style.display = 'none');
  document.getElementById('diag-' + max).style.display = 'block';
}
```

Три результата (скрыты):

ПРОБЛЕМА С ОБЪЁМОМ: "Скорее всего ты или перегружаешь тело (работаешь выше своего MRV) или недогружаешь (остаёшься в зоне MEV без роста). Решение: правильный расчёт объёма под твои данные и периодизация нагрузки внутри мезоцикла."

ПРОБЛЕМА С ВОССТАНОВЛЕНИЕМ: "Тренировочный стимул есть, но тело не успевает адаптироваться из-за недосыпа, стресса или слишком частых тренировок. Рост происходит не во время тренировки, а после неё."

ПРОБЛЕМА СО СТРУКТУРОЙ: "Отсутствует системный план. Тренировки есть, усердие есть, а программы как таковой нет. Именно это чаще всего стоит за долгосрочной стагнацией."

---

### БЛОК 11: ПОЧЕМУ САМОМУ СЛОЖНО

Заголовок: "Почему это трудно сделать самому"
Вводный текст: "Всё что написано выше это общие принципы. Реальная программа это 20+ переменных которые нужно учесть одновременно и которые постоянно меняются."

Сетка 3x2 карточек (маленькие, без иконок):
1. "Твой точный MRV для каждой мышцы"
2. "Тип периодизации под твою цель"
3. "Дробный объём синергистов"
4. "Коррекция нагрузки под сон и стресс"
5. "Точки прогрессии и признаки застоя"
6. "Адаптация программы под травмы"

Стиль карточек: padding: 16px, border: 1px solid var(--border), border-radius: var(--radius), font-size: 14px, font-weight: 500.

Текст после: "Каждый из этих параметров влияет на программу. И каждый меняется каждые несколько недель. Профессиональный тренер не просто составляет программу один раз. Он отслеживает маркеры, корректирует нагрузку и двигает тебя по плану который действительно соответствует твоему состоянию прямо сейчас."

---

### БЛОК 12: АВТОР

Заголовок: "Кто это написал"

Карточка автора: горизонтальная на десктопе (flex, gap: 32px), вертикальная на мобильном.

```html
<div class="author-card">
  <div class="author-avatar">ДМ</div>  <!-- заглушка вместо фото, 80x80px, bg: var(--accent), цвет #0d0d0d, Unbounded 700 -->
  <div class="author-info">
    <div class="author-name">Дмитрий Мухин</div>
    <div class="author-role">Фитнес-тренер · 8 лет опыта</div>
    <p>Работаю с занятыми людьми которые хотят получить результат а не просто ходить в зал. Строю программы на основе актуальных исследований по физиологии и периодизации нагрузок.</p>
    <p>Мой подход: объясняю почему, а не просто говорю что. За каждым решением в программе стоит конкретная физиологическая причина.</p>
    <a href="https://t.me/BodyBal" target="_blank" class="author-link">→ Telegram канал</a>
  </div>
</div>
```

Три цифры под карточкой (flex, три блока):
- "8 лет" / "опыта"
- "100+" / "клиентов"
- "0" / "шаблонных программ"

Стиль цифр: font-family: var(--font-display), font-size: 36px, font-weight: 700, color: var(--accent).

---

### БЛОК 13: ФИНАЛЬНЫЙ CTA (id="cta-final")

Полноэкранная секция. Фон с радиальным свечением акцентного цвета:
```css
.cta-final {
  background:
    radial-gradient(ellipse at 50% 100%, rgba(200,245,66,0.08) 0%, transparent 70%),
    var(--bg-main);
  border-top: 1px solid var(--border-accent);
  text-align: center;
  padding: 100px 20px;
}
```

Лейбл: "ГОТОВ РАБОТАТЬ СИСТЕМНО?"

Заголовок (крупный, Unbounded):
"Хочешь такую же систему
под свои данные?"

Подзаголовок: "Я строю программы индивидуально. Не шаблон с твоим именем, а реальный план с учётом твоего уровня, цели, расписания, травм и данных восстановления. С еженедельными корректировками по результатам."

Две кнопки в ряд (на мобильном колонкой):
```html
<div class="cta-buttons">
  <a href="https://t.me/dgmukhin_adm" class="btn-primary" target="_blank">Написать Дмитрию →</a>
  <a href="#offer" class="btn-outline">Узнать об условиях</a>
</div>
```
btn-primary: background: var(--accent), color: #0d0d0d, font-weight: 700, padding: 16px 36px, border-radius: 100px.
btn-outline: border: 1px solid var(--border-accent), color: var(--accent), padding: 16px 36px, border-radius: 100px, background: var(--accent-dim).

Текст под кнопками: "Или начни с бесплатного шага: заполни короткую анкету и получи программу на первую неделю автоматически в Telegram."

Ссылка: "→ Получить пробную программу бесплатно" → href="https://t.me/[бот]?start=quiz"

---

### ФУТЕР

```html
<footer>
  <div class="footer-inner">
    <div class="footer-brand">© Дмитрий Мухин · Тренер</div>
    <div class="footer-links">
      <a href="https://t.me/BodyBal" target="_blank">Telegram канал</a>
      <a href="https://t.me/dgmukhin_adm" target="_blank">Написать</a>
    </div>
  </div>
</footer>
```

---

## МОБИЛЬНАЯ АДАПТАЦИЯ (обязательно)

```css
@media (max-width: 640px) {
  /* Все сетки → 1 колонка */
  .cards-grid { grid-template-columns: 1fr; }
  .cycle-timeline { grid-template-columns: 1fr; }
  /* Hero h1 */
  .hero h1 { font-size: 28px; }
  /* Факты hero → колонка */
  .hero-facts { flex-direction: column; gap: 12px; }
  /* CTA кнопки → колонка */
  .cta-buttons { flex-direction: column; align-items: center; }
  /* Секции padding */
  section { padding: 48px 20px; }
  /* Навбар логотип tagline — скрыть */
  .logo-tagline { display: none; }
  /* Автор — вертикально */
  .author-card { flex-direction: column; align-items: center; text-align: center; }
}
```

---

## КОНСТАНТЫ (подставить перед деплоем)

```
ССЫЛКА_НА_TELEGRAM:   https://t.me/dgmukhin_adm
ССЫЛКА_НА_КАНАЛ:      https://t.me/BodyBal
ССЫЛКА_НА_КВИЗ:       https://t.me/[бот]?start=quiz
ССЫЛКА_НА_ОФФЕР:      #offer
```

---

## ИТОГОВАЯ КОМАНДА

Создай полностью рабочий index.html используя всю структуру и логику выше. Все три интерактивных блока с реальным JavaScript расчётом. Все 13 блоков в правильном порядке. Sticky navbar с blur. Анимации появления при скролле для всех секций. Полная мобильная адаптация. Никаких внешних зависимостей кроме Google Fonts. Весь CSS и JS внутри одного файла.
