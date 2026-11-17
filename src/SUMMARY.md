# Содержание

[Введение](README.md)

# Предисловие

- [О руководстве](preface.md)
- [Аудитория и соглашения](preface-audience.md)
- [Дополнительная литература](further-reading.md)

# 1. Введение

- [1.1. О контроллере GPIO (PL061)](ch01-about.md)
  - [1.1.1. Особенности](ch01-features.md)

# 2. Функциональное описание

- [2.1. Обзор контроллера GPIO](ch02-overview.md)
  - [2.1.1. Регистр направления данных](ch02-direction.md)
  - [2.1.2. Регистр данных](ch02-data.md)
  - [2.1.3. Регистры управления прерываниями](ch02-interrupt-regs.md)
  - [2.1.4. Регистр выбора режима](ch02-mode-select.md)
  - [2.1.5. Регистры идентификации](ch02-id-regs.md)
- [2.2. Функциональное описание](ch02-functional.md)
  - [2.2.1. Интерфейс AMBA APB](ch02-apb.md)
  - [2.2.2. Логика обнаружения прерываний](ch02-interrupt-detect.md)
  - [2.2.3. Управление режимом](ch02-mode-control.md)
- [2.3. Работа контроллера](ch02-operation.md)
  - [2.3.1. Сброс интерфейса](ch02-reset.md)
  - [2.3.2. Конфигурирование интерфейса](ch02-configuration.md)
  - [2.3.3. Работа линий ввода-вывода](ch02-io-operation.md)
  - [2.3.4. Работа прерываний](ch02-interrupt-operation.md)
  - [2.3.5. Управление режимом](ch02-mode-operation.md)

# 3. Модель программиста

- [3.1. Общие сведения](ch03-about.md)
- [3.2. Сводка регистров](ch03-summary.md)
- [3.3. Описание регистров](ch03-registers.md)
  - [3.3.1. Регистр данных GPIODATA](ch03-gpiodata.md)
  - [3.3.2. Направление данных GPIODIR](ch03-gpiodir.md)
  - [3.3.3. Чувствительность GPIOIS](ch03-gpiois.md)
  - [3.3.4. Оба фронта GPIOIBE](ch03-gpioibe.md)
  - [3.3.5. Событие GPIOIEV](ch03-gpioiev.md)
  - [3.3.6. Маска прерываний GPIOIE](ch03-gpioie.md)
  - [3.3.7. Сырой статус GPIORIS](ch03-gpioris.md)
  - [3.3.8. Маскированный статус GPIOMIS](ch03-gpiomis.md)
  - [3.3.9. Сброс прерываний GPIOIC](ch03-gpioic.md)
  - [3.3.10. Выбор режима GPIOAFSEL](ch03-gpioafsel.md)
  - [3.3.11. Идентификация GPIOPeriphID0-3](ch03-periphid.md)
  - [3.3.12. Идентификация GPIOPCellID0-3](ch03-pcellid.md)

# 4. Модель программиста для тестирования

- [4.1. Обзор тестовой обвязки](ch04-overview.md)
- [4.2. Scan-тестирование](ch04-scan.md)
- [4.3. Интеграционные тестовые регистры](ch04-test-registers.md)
  - [4.3.1. GPIOITCR](ch04-itcr.md)
  - [4.3.2. GPIOITIP1](ch04-itip1.md)
  - [4.3.3. GPIOITIP2](ch04-itip2.md)
  - [4.3.4. GPIOITOP1](ch04-itop1.md)
  - [4.3.5. GPIOITOP2](ch04-itop2.md)
  - [4.3.6. GPIOITOP3](ch04-itop3.md)
- [4.4. Тестирование входов](ch04-inputs.md)
  - [4.4.1. Внутрикристальные входы](ch04-intra-inputs.md)
  - [4.4.2. Первичные входы](ch04-primary-inputs.md)
- [4.5. Тестирование выходов](ch04-outputs.md)
  - [4.5.1. Внутрикристальные выходы](ch04-intra-outputs.md)
  - [4.5.2. Первичные выходы](ch04-primary-outputs.md)
- [4.6. Итоги интеграционного тестирования](ch04-summary.md)

# 5. Описание сигналов

- [5.1. Сигналы AMBA APB](ch05-apb.md)
- [5.2. Внутрикристальные сигналы](ch05-onchip.md)
- [5.3. Сигналы выводов](ch05-pads.md)

# Приложения

- [Приложение A. Глоссарий](glossary.md)
