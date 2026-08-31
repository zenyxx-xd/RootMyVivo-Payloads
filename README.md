# RootMyVivo Payloads

Каталог payload'ов и конфигов для [RootMyVivo](https://github.com/zenyxx-xd/RootMyVivo).

## Формат

`support/targets-vivo.json` — главный каталог:
- `payloads[].models[]` — модели устройств
- `payloads[].kernelVersions[]` — совместимые версии ядра  
- `payloads[].exploit` — тип используемого эксплойта
- `payloads[].files.preload.so` — ссылка на прекомпилированный бинарь

## Добавление устройства

1. Сгенерируй оффсеты из boot.img (используй RootMyVivo app или vmlinux-to-elf)
2. Создай PR с новой записью в targets-vivo.json
3. Приложи preload.so в папке payloads/{configId}/

## Дисклеймер

Только для собственных устройств. Авторы не несут ответственности за последствия.

## Payloads

| Файл | Версия | Устройство | Источник |
|---|---|---|---|
| `preload-rmv.so` | v0.2.0 | iQOO Neo 11 (PD2520) | rmv-exploit — чистая сборка CVE-2026-43499 с стабилизационным слоем |

См. [rmv-exploit](https://github.com/zenyxx-xd/RootMyVivo) — исходники, отличия от upstream, конфигурация.
