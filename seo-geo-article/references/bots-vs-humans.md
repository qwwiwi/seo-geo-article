# Боты против людей без Cloudflare

Как понять, сколько на сайт пришло людей, сколько поисковых и ИИ-ботов, и отличить
настоящего бота от подделки. Всё на открытых инструментах и своём сервере.

## Почему в GA4 и Метрике ноль роботов – это нормально

GA4, Яндекс Метрика, Umami и другие JS-счётчики считают только тех, кто выполнил JavaScript
на странице. GPTBot, ClaudeBot, PerplexityBot и большинство краулеров JS не выполняют – в
счётчике их нет вовсе. Ноль ИИ-ботов в аналитике – ожидаемо, а не признак, что их нет.

Источник правды – логи веб-сервера: в них попадает каждый запрос. Отдельный CDN тоже может
показывать ботов, но для этой схемы он не нужен.

## Шаг 1. JSON-лог доступа

Caddy (пример):

```caddyfile
blog.example.com {
  root * /var/www/blog
  file_server
  log {
    output file /var/log/caddy/blog-access.log {
      roll_size 50MiB
      roll_keep 20
    }
    format json
  }
}
```

Затем `caddy validate` и `systemctl reload caddy`. Если сайт стоит за прокси или балансером,
настрой в Caddy доверенные прокси (`trusted_proxies`), иначе в логе будет IP прокси, а не
посетителя.

- **nginx** (`access_log`) и **Apache** (`CustomLog`): стандартный combined-формат – GoAccess
  читает его как `COMBINED`; для JSON-лога в nginx – `log_format ... escape=json` и свой `--log-format`.

## Шаг 2. GoAccess 1.12 или новее

[GoAccess](https://github.com/allinurl/goaccess) (MIT) читает лог Caddy встроенным форматом
`CADDY` и берёт IP из поля `request.client_ip`. В версии 1.12 появилась категория «AI
Crawlers»: GPTBot, OAI-SearchBot, ChatGPT-User, ClaudeBot, Claude-User, PerplexityBot,
meta-externalagent, Bytespider. Проверь `goaccess --version`: в пакетах дистрибутива может
оказаться старая версия без этой категории.

## Шаг 3. Два отчёта по расписанию

Cron или systemd-таймер, раз в 15–60 минут:

```bash
goaccess /var/log/caddy/blog-access.log --log-format=CADDY --ignore-crawlers --unknowns-as-crawlers -o /var/www/stats/humans.html
goaccess /var/log/caddy/blog-access.log --log-format=CADDY --crawlers-only -o /var/www/stats/bots.html
```

Первый – люди (боты и неизвестные UA выброшены), второй – только боты.

## Шаг 4. Закрыть /stats паролем

В отчётах IP и пути посетителей – наружу их не отдавать. В Caddy:

```caddyfile
basic_auth /stats/* {
  admin <хеш из caddy hash-password>
}
```

В nginx – `auth_basic` и `auth_basic_user_file`, в Apache – `AuthType Basic`.

## Список UA ИИ-краулеров

- Актуальные UA – из [ai.robots.txt](https://github.com/ai-robots-txt/ai.robots.txt)
  (MIT): справочник ИИ-агентов с оператором и назначением.
- Используй его для **учёта**, а не для блокировки: готовые правила из репозитория закрывают
  ботов, а цель – видеть, кто читает сайт.
- Список можно подключить к GoAccess через `--browsers-file`, чтобы ловить ботов, которых
  нет во встроенном списке.
- `Google-Extended` – только токен для robots.txt, отдельного UA у него нет, в логах его
  не увидеть.

## Настоящий бот или подделка

UA подделать легко, поэтому IP сверяй с официальными данными:

| Бот | Как проверить |
|---|---|
| Googlebot | обратный DNS → `googlebot.com`, `google.com` или `googleusercontent.com`, прямой DNS этого имени возвращает тот же IP. Диапазоны – JSON в `developers.google.com/static/crawling/ipranges/` |
| YandexBot | обратный DNS → `yandex.ru`, `yandex.net` или `yandex.com`, прямой DNS совпадает с IP |
| OpenAI | `openai.com/gptbot.json`, `searchbot.json`, `chatgpt-user.json` |
| Anthropic | `claude.com/crawling/bots.json` (ClaudeBot, Claude-User, Claude-SearchBot) |
| Perplexity | `perplexity.ai/perplexitybot.json`, `perplexity-user.json` |

Не совпало – поддельный UA, считай его подозрительным.

Признак датацентра – IP из облачных и хостинговых сетей. Открытые списки:
[lord-alfred/ipranges](https://github.com/lord-alfred/ipranges) (диапазоны Google, Bing, AWS
и др.), [bad-asn-list](https://github.com/brianhama/bad-asn-list) (ASN облаков и хостингов),
[X4BNet/lists_vpn](https://github.com/X4BNet/lists_vpn) (VPN и датацентры).

## Признаки ботов в JS-аналитике

Часть ботов JS выполняет и попадает в счётчик. Подозрительно, когда:

- время на странице 0 секунд, нет прокрутки и второго действия;
- странная география: города датацентров, страна, где нет аудитории;
- серия визитов с одинаковым устройством, разрешением и браузером;
- всплеск прямых заходов на одну страницу без источника.

## Переходы из ИИ-ассистентов в GA4

- Стандартный канал **«AI Assistant»**: срабатывает, если `utm_medium=ai-assistant` или
  реферер есть в списке Google. Полный список Google не публикует. AI Overviews и AI Mode
  Google относит к Organic Search.
- Своя группа каналов: Admin → Data display → Channel groups → канал «AI» с условием
  «Session source соответствует regex», поставить **выше Referral**:

```text
(^|\.)(chatgpt\.com|chat\.openai\.com|claude\.ai|perplexity\.ai|pplx\.ai|gemini\.google\.com|copilot\.microsoft\.com|copilot\.com|deepseek\.com|chat\.deepseek\.com|grok\.com|meta\.ai|you\.com|poe\.com|chat\.mistral\.ai|alice\.yandex\.ru)$
```

Основные домены сверены со списками открытых аналитик. Хвост (deepseek, grok, mistral,
alice.yandex) не проверен – сверь с реальными реферерами в своих отчётах. Смотри Traffic
acquisition и Explore: источник × страница входа.

## Self-hosted аналитика (опционально)

| Инструмент | Как фильтрует ботов |
|---|---|
| [Umami](https://github.com/umami-software/umami) (MIT) | UA через библиотеку `isbot`; есть отдельный канал для переходов из LLM-ассистентов |
| [GoatCounter](https://github.com/arp242/goatcounter) | Go-библиотека `isbot`; один бинарь и SQLite – самый лёгкий вариант |
| [Plausible CE](https://github.com/plausible/analytics) (AGPL-3.0) | UA через UAInspector, отсев headless-браузеров и спам-рефереров; есть канал «AI Assistants»; тяжёлая установка |

Любой из них – тоже JS-счётчик: ботов без JS он не увидит, логи всё равно нужны.

## Шаблон ежедневного отчёта

```text
Дата: <YYYY-MM-DD>, окно: <00:00–24:00 UTC>
Люди:              <визиты> (GoAccess humans.html; сверка с GA4: <визиты>)
  топ страниц:     <путь – визиты>, …
  из ИИ-ассистентов: <сессии> (GA4, канал AI / AI Assistant)
Поисковые боты:    Googlebot <запросы>, YandexBot <запросы>, Bingbot <запросы>
ИИ-боты:           GPTBot <n>, OAI-SearchBot <n>, ChatGPT-User <n>, ClaudeBot <n>,
                   Claude-User <n>, PerplexityBot <n>, прочие <n>
  топ страниц:     <путь – запросы>, …
Подозрительные:    UA бота без подтверждения IP <n>, датацентровые IP <n>,
                   визиты 0 с без прокрутки <n>
Вывод:             <что изменилось и что делать>
```
