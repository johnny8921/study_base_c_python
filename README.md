# Темы курса

### 1. Вводная лекция. Инструменты для разработчика на МК.

#### Теория

- Определение целей курса, организация работы разработчика на МК. IDE, компиляторы, отладчики.
- Система контроля версий. Git, GitFlow.
- Стандарты программирования

#### Практика

- Установка IDE для разработки ПО на С и Python.

### 2. Программирование на С.

#### Теория

- Введение в С.
- Синтаксис и семантика С.
- Функции в С.
- Структуры и массивы.
- Указатели и работа с ними.

#### Практика

- Запуск первой программы. Написание простейших программ АСУ ТП.

### 3. МК на базе Cortex-M. Структура проекта. Организация работы с памятью

#### Теория

- Обзор платформы Cortex-M. Организация доступа к памяти и перефирии. Обзор общих модулей. Стек.
- Обзор семейств контроллеров STM32. Основы выбора контроллера под задачу.
- Обзор специализированных инструментов STM32, CubeMX. Как не потерять наработки после перегенерации проекта
- HAL, LL

#### Практика

- Написание первых программ на STM32.

### 4. Указатели

#### Теория

- Приемы работы с указателями, так как очень большой пласт занятий


### 5. Работа с периферией на CortexM

#### Теория

- GPIO работа с ножками контроллеров
- IRQ&NVIC базовые определения

#### Практика

- Программа реагирующая на кнопки мигая светодиодом.


### 6. АЦП и ЦАП

#### Теория

- ADC&DAC работа в прямом режиме через HAL

#### Практика

- Считываем напряжение и зажигаем светодиод если напряжение вышло за границы
- Пересчет напряжения в показания датчиков.


### 7. Работа с таймерами. Логический анализатор сигналов

#### Теория

- логический анализатор - основа разработки Embedded Устройств
- режимы работы с таймеров.
- виды таймеров
- настройка таймеров
- использование таймеров

#### Практика

- Режим отладки "одна нога" - секрет отладки в прерываниях и при работе с таймером.
- мигание светодиодом с разными интервалами на таймерах

### 4. Tricks for Embedded Programmers

#### Теория

- Плохой код, или как не надо писать код на микроконтроллеры.
- Секреты написания неблокирующего кода для микроконтроллеров.

#### Практика

- Написание неблокирующего кода для микроконтроллеров

### 5. Основы алгоритмизации для микроконтроллеров.

#### Теория

- Суперлуп. Плюсы и минусы.
- FreeRTOS: обзор системы. Плюсы и минусы.
- С чего начинается написание программы для МК.
- Шаги проектирования программного обеспечения

#### Практика

- Написание алгоритмов управления примеров АСУ ТП
- Реализация конечных автоматов






### 9. Цифровые интерфейсы

#### Теория

- Работа через интерфейсы. DataSheet – основа успеха.
- I2C

#### Практика

- Опрос датчика температуры через I2C.

### 10. Цифровые интерфейсы

#### Теория

- SPI
- UART

#### Практика

- Написание программы работающей с SPI
- Отправка и прием информации через UART

### 11. Протоколы межплатной связи

#### Теория

- Обзор стандартных протоколов межплатной связи. RS-232, RS485, CAN

#### Практика

- Реализация обмена контроллера с компьютером через UART и АТ команды

### 12. Плюсы и минусы разработки на С. Ищем альтернативы и пытаемся писать быстро. MicroPython

#### Теория

- Резюме плюсов и минусов разработки на С.
- Обзор MicroPython. Плюсы и минусы. Обзор архитектуры.
- Обзор контроллеров работающих с MicroPython.

#### Практика

- Прошивка базового ядра micropython для ESP32.
- Написание первых программ для Micropython.

### 13. Работа с периферией на Micropython

#### Теория

- GPIO
- I2C
- SPI

#### Практика

- мигание светодиодом при нажатии кнопок
- отправка и прием данных в интерфейсы

### 14. ASYNCIO. Опыт применения MicroPython. Успешные и неудачные опыты внедрения. 

#### Теория

- Общие принципы асинхронности. Корутины и Мьютексы. Отличия от тредов.
- Особенности реализации асинхронности на компьютерах
- Особенности реализации асинхронности в МК
- Обзор успешных кейсов
- Обзор неудачных кейсов и как их превратить в удачные.

#### Практика

- Написание программ для работы с кнопками и светодиодами на асинхронных функциях.
- Написание программ для работы с дисплеем и флеш-картой. 

### 15. Итоговый проект (общий в рамках модуля)

Устройство опрашивающее умный сенсор (I2C, SPI, UART) и датчик температуры (АЦП)
отображающее данные на дисплее и сохраняющие значения на флешку по SPI

- сборка макета устройства
- написание кода работы с датчиками. Получение данных и их сохранение в ОЗУ

### 16. Итоговый проект (общий в рамках модуля) Продолжение

- вывод данных на экран
- сохранение данных на флешку
