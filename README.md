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
