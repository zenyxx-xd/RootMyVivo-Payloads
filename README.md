# RootMyVivo Payloads

Каталог пейлоадов для приложения [RootMyVivo](https://github.com/zenyxx-xd/RootMyVivo).
Приложение НЕ содержит эксплойтов — оно скачивает бинарники по описанию из
[`catalog/devices.json`](catalog/devices.json).

[English](README.en.md) · [中文](README.zh.md)

## Структура

```
catalog/devices.json        единственный манифест каталога (schemaVersion 5)
bin/                        каноничные библиотеки: имя = git-id сборки ядра
                            (bin/g1f71897ac249.so); для сборок без git-суффикса —
                            осмысленный id (pd2405-ap3a.so)
support/targets-vivo.json   LEGACY: каталог v4 для старых версий приложения.
                            НЕ редактировать — заморожен.
```

## Схема devices.json (v5)

- `builds` — справочник **сборок ядра** по GKI git-id. Один билд = один Image =
  один бинарь пейлоада. Поля: `match` (список паттернов uname: полная GKI-строка
  строже короткой версии; суффикс `.*` — префикс), `exploit`, `status`,
  `file` ({name,url,mirrors,sha256,size}), `env` (RMV_ATTEMPTS, RMV_RETRY_DELAY),
  `matchCondition` — происхождение оффсетов и доказательства.
- `devices` — по одной записи на **физическое тело**: `marketName` + `code`
  (V-код), алиасы `models` (Build.DEVICE) / `names` (Build.MODEL), `kernels` —
  все известные сборки ядра этого тела: `{build, note}`.
- `status` билда: `ready` (заявлено рабочим) · `off` (бинарь есть, не заявлено) ·
  `patched` (CVE закрыт в этой сборке) · `unsupported` (пейлоада нет).
- Сопоставление в приложении: устройство по `models`/`names`, ядро по `match` —
  короткая версия сопоставляется с uname release ровно, полная GKI-строка — как
  подстрока (это различает сборки одной модели: gf2c960562dc8 vs gb57af212129c).
  Fallback на «любой пейлоад этой модели» отсутствует.

## Как добавить устройство или сборку

1. достать точный kernel Image сборки (full OTA → boot → uname, kallsyms,
   наличие не-патченного remove_waiter).
2. Положить собранный .so в `bin/` с именем по git-id, записать sha256 и размер.
3. Дополнить `builds` и `devices` (note — источник: форма пользователей,
   upstream-релиз, тест на устройстве). Не придумывать коды моделей и статусы.

## Зеркала

`url` — релизная ассетка GitHub, `mirrors` — jsDelivr и raw-repo: ассет-домен у
части пользователей недоступен, когда основной хост работает.

## Старые версии приложения

Читают замороженный `support/targets-vivo.json` (v4, формат `{payloads:[…]}`).
Новые устройства и исправления — только в v5.
