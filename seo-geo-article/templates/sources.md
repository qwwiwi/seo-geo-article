# Источники и сверка: <<PLACEHOLDER: slug>>

Статья: `content/<<PLACEHOLDER: slug>>.mdx`
Кто проверял: <<PLACEHOLDER: автор или агент и модель>>
Ревьюер: <<PLACEHOLDER: модель ревьюера, вердикт, номер фикс-лупа>>

## 1. Запрос

| Поле | Значение |
|---|---|
| Целевой запрос | <<PLACEHOLDER>> |
| Спрос | <<PLACEHOLDER: число>> – <<PLACEHOLDER: инструмент, регион, период>>, проверено <<PLACEHOLDER: дата>> (или «не перепроверено») |
| Интент | <<PLACEHOLDER: что человек хочет получить>> |
| Не дублировать | <<PLACEHOLDER: соседние slug и чем отличается интент>> |

## 2. Снимок источника о продукте

| Поле | Значение |
|---|---|
| URL | <<PLACEHOLDER: https://<your-domain>/llms-full.txt или страница продукта>> |
| Получено (UTC) | <<PLACEHOLDER>> |
| Размер, байт | <<PLACEHOLDER>> |
| sha256 | <<PLACEHOLDER>> |

## 3. Утверждения

Одна строка – одно атомарное утверждение. Вердикт: supported / rephrased / removed.
Уровень источника: 1 – первичный, 2 – исследование с методикой, 3 – пересказ со ссылкой на
первоисточник (`references/fact-check.md`, раздел 5а). Данные старше периода статьи –
пометка в колонке «Цитата».

| # | Утверждение в статье | Источник (URL, раздел, строка) | Уровень источника | Цитата | Проверено (UTC) | Вердикт |
|---|---|---|---|---|---|---|
| 1 | <<PLACEHOLDER>> | <<PLACEHOLDER>> | <<PLACEHOLDER: 1 / 2 / 3>> | <<PLACEHOLDER>> | <<PLACEHOLDER>> | <<PLACEHOLDER>> |

## 4. Числа ресёрча

| Число в статье | Инструмент | Окно выборки | Какие поля пришли | Дата | Оговорка |
|---|---|---|---|---|---|
| <<PLACEHOLDER>> | <<PLACEHOLDER>> | <<PLACEHOLDER>> | <<PLACEHOLDER>> | <<PLACEHOLDER>> | <<PLACEHOLDER>> |

## 5. Ресёрч-пак: собрано и не собрано

- YouTube: <<PLACEHOLDER: сколько видео, субтитры есть/нет>>
- Instagram: <<PLACEHOLDER: аккаунты, выбросы, разбор>>
- X / веб: <<PLACEHOLDER: собрано или «не собрано: причина»>>
- Скан чужих текстов на инструкции для агента: <<PLACEHOLDER: «совпадений нет» или файл, строка, найденный фрагмент>>

## 6. Проверки

- Линт: <<PLACEHOLDER: вывод линтера>>
- Плейсхолдеры: <<PLACEHOLDER: вывод grep, должен быть пустым>>
- Шапка на проде: <<PLACEHOLDER: строка OK проверки после деплоя>>
- JSON-LD с живого URL: <<PLACEHOLDER: типы и вывод валидатора>>
- Текст без JS: <<PLACEHOLDER: фраза из середины статьи найдена в сыром HTML – да/нет>>
- robots.txt, поисковые боты: <<PLACEHOLDER: OAI-SearchBot, Claude-SearchBot, PerplexityBot, Googlebot, YandexBot – разрешён/закрыт>>
- robots.txt, обучающие боты: <<PLACEHOLDER: GPTBot, ClaudeBot, Google-Extended и др. – разрешён/закрыт>>
