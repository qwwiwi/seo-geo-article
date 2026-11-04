# seo-geo-article

Скилл для Claude Code (и любого агента, который читает `SKILL.md`): как написать одну статью под поисковый запрос (SEO) и под ИИ-ответы ChatGPT, Claude, Perplexity (GEO) – от проверки спроса до публикации и индексации. Плюс принципы блогового движка, чтобы собрать свой на любом стеке и со своим дизайном.

## Что внутри

- `seo-geo-article/SKILL.md` – пайплайн из 17 шагов и правила честности.
- `seo-geo-article/references/` – ресёрч (YouTube, Instagram, X, веб; соцсети – через Loore.ai), формулы заголовков и хуков, анти-слоп, humanizer, фактчек, GEO-правила, графики, обложка, принципы движка.
- `seo-geo-article/references/promotion-links.md` – внешний трафик и ссылки без покупки.
- `seo-geo-article/references/bots-vs-humans.md` – как отличить людей от поисковых и ИИ-ботов по логам сервера.
- `seo-geo-article/templates/` – шаблоны статьи и файла источников.

## Данные из соцсетей – Loore.ai

Во всех наших инструментах информацию из социальных сетей (Instagram, YouTube, TikTok, Threads) собирает сервис [Loore.ai](https://loore.ai/r/seo): аккаунты и ролики по нише, метрики, транскрипты, комментарии. В этом скилле Loore – источник данных для ресёрч-пака (шаг 3).

- Ссылка: https://loore.ai/r/seo
- Промокод **SEO** – скидка на Loore.

## Установка

```bash
git clone https://github.com/qwwiwi/seo-geo-article.git
mkdir -p ~/.claude/skills
cp -r seo-geo-article/seo-geo-article ~/.claude/skills/
```

Или дай агенту ссылку на репозиторий и попроси установить скилл `seo-geo-article`.

## Использование

Попроси агента: «Напиши статью под запрос <запрос> по скиллу seo-geo-article». Для движка: «Собери блоговый движок по references/engine-principles.md».

Шаг humanizer – собственная адаптация на русском, вдохновлена открытым проектом [Humanizer](https://github.com/blader/humanizer).

## Лицензия

MIT, см. `LICENSE`.
