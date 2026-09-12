# RootMyVivo Payloads

Каталог пейлоадов для [RootMyVivo](https://github.com/zenyxx-xd/RootMyVivo). Приложение ничего не несёт внутри — все эксплойты скачиваются отсюда.

[English](README.en.md) · [中文](README.zh.md)

## Что здесь лежит

- `support/targets-vivo.json` — каталог: какие устройства поддерживаются, каким эксплойтом, откуда качать бинарь
- Релизы — сами пейлоады. Каждому устройству — своя сборка, оффсеты от ядра к ядру не переезжают

## Как приложение выбирает пейлоад

Сначала модель (`models` / `marketNames`), потом ядро. Запись `kernelVersions` бывает трёх видов:

```json
"6.6.89"                              // короткая: любая 6.6.89
"6.6.89-android15-8-gb57af212129c"    // полная: конкретная сборка ядра (по uname)
"6.6.89-android15-8-*"                // префикс
```

Полная запись нужна, когда одна модель живёт на разных сборках ядра — например, Neo10 Pro встречается и на `gf2c960562dc8`, и на `b57af212129c` (общей с X200). Короткая запись подберёт не тот бинарь, а правильный слайд просто не сойдётся.

## Сборки

| Релиз | Что внутри |
|---|---|
| [v0.4.0-ports](https://github.com/zenyxx-xd/RootMyVivo-Payloads/releases/tag/v0.4.0-ports) | Порты RMV-движка: iQOO 13 India (I2401), Neo10 Pro (PD2426), X200 (PD2415), X200 Pro (PD2405) |
| [v0.4.0-neo11](https://github.com/zenyxx-xd/RootMyVivo-Payloads/releases/tag/v0.4.0-neo11) | Neo 11, ядро 6.6.89 |
| [v0.4.0-6.6.127-neo11](https://github.com/zenyxx-xd/RootMyVivo-Payloads/releases/tag/v0.4.0-6.6.127-neo11) | Neo 11, ядро 6.6.127 |
| [v0.2.1-neo11](https://github.com/zenyxx-xd/RootMyVivo-Payloads/releases/tag/v0.2.1-neo11) | Neo 11, ранняя сборка |

Оффсеты для портов взяты из публичных репозиторитов авторов соответствующих устройств (AmarnathCJD, sgswzglwlx, xiaohj233, CyberMeowfia) — благодарности в поле `verifiedBy` каждой записи каталога.

## Добавить своё устройство

Нужен `boot.img` твоей прошивки (без root, достаточно OTA-зипа) или дамп kallsyms. Дальше:

1. Собери target.h из kallsyms+BTF — см. [RootMyVivo-Exploit](https://github.com/zenyxx-xd/RootMyVivo-Exploit), там генератор
2. Собери preload.so и проверь, что цепочка проходит хотя бы до слайда
3. PR с записью в `targets-vivo.json`: модель, полная строка ядра, ссылка на бинарь, sha256

Пейлоады от других авторов тоже принимаются, если они не софт-ребутят телефон посреди установки — в этом весь смысл этой сборки.

## Дисклеймер

Только для собственных устройств. Авторы не отвечают за последствия.
