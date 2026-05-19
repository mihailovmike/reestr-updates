# reestr-updates

Публичный фид обновлений платформы **evidpo LMS reestr**. Читается клиентским JS на странице `/notifs` и инжектором бейджа сайдбара через GitHub raw.

## Файлы

- `changelog.json` — массив обновлений, новейшие сверху.

## Формат записи

```json
{
  "id": "2026-05-19-feature-x",
  "date": "2026-05-19",
  "time": "14:30",
  "title": "Краткий заголовок",
  "body": "Расширенное описание для пользователя.",
  "severity": "info",
  "pinned": false,
  "link": "/cw/{cwid}?tab=tab_1761249139051"
}
```

| Поле | Тип | Обяз. | Описание |
|---|---|---|---|
| `id` | string | ✓ | Уникальный slug. Рекомендуем `YYYY-MM-DD-short-slug`. По нему трекается «прочитано» в localStorage. |
| `date` | string | ✓ | `YYYY-MM-DD`. |
| `time` | string | — | `HH:MM`. Если есть — рендерится после даты. |
| `title` | string | ✓ | Короткий заголовок. |
| `body` | string | ✓ | Тело уведомления, plain text. |
| `severity` | string | — | `info` / `warning` / `critical`. По умолчанию `info`. Бейдж колокольчика считает только `warning + critical` и `pinned`. |
| `pinned` | bool | — | Закрепить наверху списка. Считается «важным» для бейджа. |
| `link` | string | — | Куда повести пользователя на «Открыть». Поддерживает `{cwid}` placeholder — заменяется на текущий cwid из URL. |

## Workflow добавления обновления

```bash
cd updates                # subdir в reestr-develop
nano changelog.json       # добавить запись в НАЧАЛО массива
git add changelog.json
git commit -m "feat: <title>"
git push
```

Пользователи увидят новую запись через ~5 минут (GitHub raw CDN cache).

## Связь с приватным репо

Этот репо — публичная зависимость приватного [`mihailovmike/reestr-develop`](https://github.com/mihailovmike/reestr-develop). Локально лежит в подпапке `reestr-develop/updates/`, гитигнорится parent-репозиторием.

На свежем чекауте `reestr-develop` подпапка `updates/` пуста — нужно отдельно склонировать:

```bash
cd ~/coding/evidpo/lms/reestr/reestr-develop
git clone git@github.com:mihailovmike/reestr-updates.git updates
```

## URL для GitHub raw

```
https://raw.githubusercontent.com/mihailovmike/reestr-updates/main/changelog.json
```

CORS: `Access-Control-Allow-Origin: *` — доступен с любого домена.
