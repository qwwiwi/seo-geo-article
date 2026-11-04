# seo-geo-article

Скилл для Claude Code (и любого агента, который читает `SKILL.md`): как написать одну статью под поисковый запрос (SEO) и под ИИ-ответы ChatGPT, Claude, Perplexity (GEO) – от проверки спроса до публикации и индексации. Плюс принципы блогового движка, чтобы собрать свой на любом стеке и со своим дизайном.

## Что внутри

- `seo-geo-article/SKILL.md` – пайплайн из 16 шагов и правила честности.
- `seo-geo-article/references/` – ресёрч (YouTube, Instagram, X, веб), формулы заголовков и хуков, анти-слоп, humanizer, фактчек, GEO-правила, графики, обложка, принципы движка.
- `seo-geo-article/templates/` – шаблоны статьи и файла источников.

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
