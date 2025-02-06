### Что такое GPIO?

**GPIO** расшифровывается как **General Purpose Input/Output** (Обобщенный вход/выход). Это цифровые порты
микроконтроллера, которые могут быть настроены для работы как входами, так и выходами. GPIO используются для
взаимодействия микроконтроллера с внешними устройствами, такими как светодиоды, кнопки, датчики и другие цифровые
устройства.

### Базовый код инициализации работы с GPIO на STM32

Для примера мы будем использовать микроконтроллер **STM32F407VG** и библиотеку **STM32CubeMX** для генерации базового
кода. Мы настроим один из пинов GPIO для управления светодиодом.

#### Шаг 1: Настройка проекта с использованием STM32CubeMX

1. **Откройте STM32CubeMX**.
2. **Выберите микроконтроллер** (например, **STM32F407VG**).
3. **Настройте частоту тактирования** (по умолчанию 168 MHz).
4. **Настройте GPIO**:
    - Перейдите в раздел **Pinout & Configuration**.
    - Найдите пин, который вы хотите использовать для управления светодиодом (например, **PD12**).
    - Нажмите правой кнопкой мыши на пине и выберите **GPIO Output**.
    - Настройте параметры пина (например, **Mode: Output PP**, **Speed: Low**, **Pull-up/Pull-down: No pull**).

5. **Сгенерируйте код**:
    - Перейдите в раздел **Project**.
    - Укажите имя проекта и место сохранения.
    - Выберите **Toolchain/IDE** (например, **Makefile** или **SW4STM32**).
    - Нажмите **Generate Code**.

#### Шаг 2: Настройка и запуск кода

1. **Откройте сгенерированный проект** в вашей IDE (например, **SW4STM32**).
2. **Добавьте код для управления GPIO** в файле `main.c`.

Пример базового кода для управления светодиодом на пине **PD12**:

```c
#include "main.h"

void SystemClock_Config(void);
static void MX_GPIO_Init(void);

int main(void) {
    // Инициализация HAL Library
    HAL_Init();

    // Настройка системного тактирования
    SystemClock_Config();

    // Инициализация GPIO
    MX_GPIO_Init();

    while (1) {
        // Включение светодиода
        HAL_GPIO_WritePin(GPIOD, GPIO_PIN_12, GPIO_PIN_SET);
        HAL_Delay(500);  // Задержка 500 мс

        // Выключение светодиода
        HAL_GPIO_WritePin(GPIOD, GPIO_PIN_12, GPIO_PIN_RESET);
        HAL_Delay(500);  // Задержка 500 мс
    }
}

void SystemClock_Config(void) {
    // Конфигурация системного тактирования
    RCC_OscInitTypeDef RCC_OscInitStruct = {0};
    RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};

    // Настройка HSE
    RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSE;
    RCC_OscInitStruct.HSEState = RCC_HSE_ON;
    RCC_OscInitStruct.PLL.PLLState = RCC_PLL_ON;
    RCC_OscInitStruct.PLL.PLLSource = RCC_PLLSOURCE_HSE;
    RCC_OscInitStruct.PLL.PLLM = 8;
    RCC_OscInitStruct.PLL.PLLN = 336;
    RCC_OscInitStruct.PLL.PLLP = RCC_PLLP_DIV2;
    RCC_OscInitStruct.PLL.PLLQ = 7;
    if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK) {
        Error_Handler();
    }

    // Настройка системного тактирования
    RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK | RCC_CLOCKTYPE_SYSCLK | RCC_CLOCKTYPE_PCLK1 | RCC_CLOCKTYPE_PCLK2;
    RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_PLLCLK;
    RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
    RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV4;
    RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV2;

    if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_5) != HAL_OK) {
        Error_Handler();
    }
}

static void MX_GPIO_Init(void) {
    GPIO_InitTypeDef GPIO_InitStruct = {0};

    // Включение тактирования порта D
    __HAL_RCC_GPIOD_CLK_ENABLE();

    // Настройка пина PD12 как выхода
    GPIO_InitStruct.Pin = GPIO_PIN_12;
    GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
    GPIO_InitStruct.Pull = GPIO_NOPULL;
    GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_LOW;
    HAL_GPIO_Init(GPIOD, &GPIO_InitStruct);
}

void Error_Handler(void) {
    // Обработка ошибок
    while (1) {
    }
}
```

**Объяснение:**

1. **Инициализация HAL Library**:
   ```c
   HAL_Init();
   ```
   Инициализирует библиотеку HAL, которая предоставляет высокоуровневые функции для работы с микроконтроллером.

2. **Настройка системного тактирования**:
   ```c
   SystemClock_Config();
   ```
   Конфигурирует системное тактирование микроконтроллера.

3. **Инициализация GPIO**:
   ```c
   MX_GPIO_Init();
   ```
   Инициализирует выбранные пины GPIO в соответствии с настройками, сделанными в STM32CubeMX.

4. **Основной цикл программы**:
   ```c
   while (1) {
       HAL_GPIO_WritePin(GPIOD, GPIO_PIN_12, GPIO_PIN_SET);
       HAL_Delay(500);
       HAL_GPIO_WritePin(GPIOD, GPIO_PIN_12, GPIO_PIN_RESET);
       HAL_Delay(500);
   }
   ```
   В бесконечном цикле включает и выключает светодиод на пине **PD12** с интервалом 500 миллисекунд.

5. **Функция настройки системного тактирования**:
   ```c
   void SystemClock_Config(void) {
       // Конфигурация HSE и PLL
       // ...
   }
   ```
   Настройка источника и частоты системного тактирования.

6. **Функция инициализации GPIO**:
   ```c
   static void MX_GPIO_Init(void) {
       GPIO_InitTypeDef GPIO_InitStruct = {0};

       // Включение тактирования порта D
       __HAL_RCC_GPIOD_CLK_ENABLE();

       // Настройка пина PD12 как выхода
       GPIO_InitStruct.Pin = GPIO_PIN_12;
       GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
       GPIO_InitStruct.Pull = GPIO_NOPULL;
       GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_LOW;
       HAL_GPIO_Init(GPIOD, &GPIO_InitStruct);
   }
   ```
   Включает тактирование порта **GPIOD** и настраивает пин **PD12** как выход с режимом push-pull.

7. **Функция обработки ошибок**:
   ```c
   void Error_Handler(void) {
       while (1) {
       }
   }
   ```
   Бесконечный цикл для обработки ошибок.

## Кнопки

### Что такое кнопки и GPIO?

**Кнопки** — это устройства ввода, которые позволяют пользователю отправлять сигналы в микроконтроллер. Когда кнопка
нажата, состояние соответствующего пина GPIO изменяется (например, с логического нуля на единицу или наоборот).

### Шаги для настройки кнопки на STM32

#### Шаг 1: Настройка проекта с использованием STM32CubeMX

1. **Откройте STM32CubeMX**.

2. **Настройте GPIO для кнопки**:
    - Найдите пин, который вы хотите использовать для кнопки (например, **PA0**).
    - Нажмите правой кнопкой мыши на пине и выберите **GPIO Input**.
    - Настройте параметры пина (например, **Mode: Input**, **Pull-up/Pull-down: Pull-up**).

3 **Сгенерируйте код**:

- Перейдите в раздел **Project**.
- Укажите имя проекта и место сохранения.
- Выберите **Toolchain/IDE** (например, **Makefile** или **SW4STM32**).
- Нажмите **Generate Code**.

#### Шаг 2: Настройка и запуск кода

1. **Откройте сгенерированный проект** в вашей IDE (например, **SW4STM32**).
2. **Добавьте код для управления GPIO** в файле `main.c`.

Пример базового кода для управления светодиодом с использованием кнопки:

```c
#include "main.h"

void SystemClock_Config(void);
static void MX_GPIO_Init(void);

int main(void) {
    // Инициализация HAL Library
    HAL_Init();

    // Настройка системного тактирования
    SystemClock_Config();

    // Инициализация GPIO
    MX_GPIO_Init();

    while (1) {
        // Проверка состояния кнопки
        if (HAL_GPIO_ReadPin(GPIOA, GPIO_PIN_0) == GPIO_PIN_RESET) {
            // Кнопка нажата
            HAL_GPIO_WritePin(GPIOD, GPIO_PIN_12, GPIO_PIN_SET);  // Включение светодиода
        } else {
            // Кнопка отпущена
            HAL_GPIO_WritePin(GPIOD, GPIO_PIN_12, GPIO_PIN_RESET);  // Выключение светодиода
        }
        HAL_Delay(10);  // Задержка для фильтрации шума
    }
}

void SystemClock_Config(void) {
    // Конфигурация системного тактирования
    RCC_OscInitTypeDef RCC_OscInitStruct = {0};
    RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};

    // Настройка HSE
    RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSE;
    RCC_OscInitStruct.HSEState = RCC_HSE_ON;
    RCC_OscInitStruct.PLL.PLLState = RCC_PLL_ON;
    RCC_OscInitStruct.PLL.PLLSource = RCC_PLLSOURCE_HSE;
    RCC_OscInitStruct.PLL.PLLM = 8;
    RCC_OscInitStruct.PLL.PLLN = 336;
    RCC_OscInitStruct.PLL.PLLP = RCC_PLLP_DIV2;
    RCC_OscInitStruct.PLL.PLLQ = 7;
    if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK) {
        Error_Handler();
    }

    // Настройка системного тактирования
    RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK | RCC_CLOCKTYPE_SYSCLK | RCC_CLOCKTYPE_PCLK1 | RCC_CLOCKTYPE_PCLK2;
    RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_PLLCLK;
    RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
    RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV4;
    RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV2;

    if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_5) != HAL_OK) {
        Error_Handler();
    }
}

static void MX_GPIO_Init(void) {
    GPIO_InitTypeDef GPIO_InitStruct = {0};

    // Включение тактирования порта D и порта A
    __HAL_RCC_GPIOD_CLK_ENABLE();
    __HAL_RCC_GPIOA_CLK_ENABLE();

    // Настройка пина PD12 как выхода
    GPIO_InitStruct.Pin = GPIO_PIN_12;
    GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
    GPIO_InitStruct.Pull = GPIO_NOPULL;
    GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_LOW;
    HAL_GPIO_Init(GPIOD, &GPIO_InitStruct);

    // Настройка пина PA0 как входа с подтяжкой к питанию
    GPIO_InitStruct.Pin = GPIO_PIN_0;
    GPIO_InitStruct.Mode = GPIO_MODE_INPUT;
    GPIO_InitStruct.Pull = GPIO_PULLUP;
    HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);
}

void Error_Handler(void) {
    // Обработка ошибок
    while (1) {
    }
}
```

**Объяснение:**

1. **Основной цикл программы**:
   ```c
   while (1) {
       if (HAL_GPIO_ReadPin(GPIOA, GPIO_PIN_0) == GPIO_PIN_RESET) {
           HAL_GPIO_WritePin(GPIOD, GPIO_PIN_12, GPIO_PIN_SET);  // Включение светодиода
       } else {
           HAL_GPIO_WritePin(GPIOD, GPIO_PIN_12, GPIO_PIN_RESET);  // Выключение светодиода
       }
       HAL_Delay(10);  // Задержка для фильтрации шума
   }
   ```
   В бесконечном цикле проверяется состояние кнопки на пине **PA0**. Если кнопка нажата (логический ноль), светодиод на
   пине **PD12** включается. Если кнопка отпущена, светодиод выключается. Задержка в 10 миллисекунд используется для
   фильтрации шума.

2. **Функция инициализации GPIO**:
   ```c
   static void MX_GPIO_Init(void) {
       GPIO_InitTypeDef GPIO_InitStruct = {0};

       // Включение тактирования порта D и порта A
       __HAL_RCC_GPIOD_CLK_ENABLE();
       __HAL_RCC_GPIOA_CLK_ENABLE();

       // Настройка пина PD12 как выхода
       GPIO_InitStruct.Pin = GPIO_PIN_12;
       GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
       GPIO_InitStruct.Pull = GPIO_NOPULL;
       GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_LOW;
       HAL_GPIO_Init(GPIOD, &GPIO_InitStruct);

       // Настройка пина PA0 как входа с подтяжкой к питанию
       GPIO_InitStruct.Pin = GPIO_PIN_0;
       GPIO_InitStruct.Mode = GPIO_MODE_INPUT;
       GPIO_InitStruct.Pull = GPIO_PULLUP;
       HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);
   }
   ```
   Включает тактирование портов **GPIOD** и **GPIOA** и настраивает пины **PD12** и **PA0** соответственно.

3. **Функция обработки ошибок**:
   ```c
   void Error_Handler(void) {
       while (1) {
       }
   }
   ```
   Бесконечный цикл для обработки ошибок.

### Дополнительные рекомендации

- **Фильтрация шума**: В реальных условиях кнопки могут создавать шум при нажатии и отпускании. Использование задержки (
  например, `HAL_Delay(10)`) помогает устранить этот шум.
- **Прерывания**: Для более точного и реактивного управления кнопками можно использовать прерывания. Это позволит
  обрабатывать события нажатия и отпускания кнопок без постоянного опроса в основном цикле.
- **Дебаунсинг**: Для предотвращения множественных срабатываний при нажатии кнопки можно реализовать дебаунсинг.

### Пример с использованием прерываний

Для более эффективного управления кнопкой можно использовать прерывания. Вот пример настройки прерывания для кнопки на
пине **PA0**.

#### Шаг 1: Настройка прерывания в STM32CubeMX

1. **Окройте CubeMX**
2. **Настройте прерывание для пина PA0**:
    - Перейдите в раздел **NVIC Settings**.
    - Найдите **EXTI Line0** (соответствует пину **PA0**) и включите его.
    - Настройте приоритет прерывания по необходимости.

3. **Сгенерируйте код**:
    - Нажмите **Generate Code**.

#### Шаг 2: Настройка и запуск кода с прерываниями

1. **Откройте сгенерированный проект** в вашей IDE (например, **SW4STM32**).
2. **Добавьте код для управления GPIO и обработки прерываний** в файле `main.c`.

Пример базового кода с использованием прерываний:

```c
#include "main.h"

void SystemClock_Config(void);
static void MX_GPIO_Init(void);
void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin);

volatile uint8_t button_pressed = 0;

int main(void) {
    // Инициализация HAL Library
    HAL_Init();

    // Настройка системного тактирования
    SystemClock_Config();

    // Инициализация GPIO
    MX_GPIO_Init();

    while (1) {
        if (button_pressed) {
            HAL_GPIO_TogglePin(GPIOD, GPIO_PIN_12);  // Переключение состояния светодиода
            button_pressed = 0;
            HAL_Delay(200);  // Задержка для предотвращения многократного срабатывания
        }
    }
}

void SystemClock_Config(void) {
    // Конфигурация системного тактирования
    RCC_OscInitTypeDef RCC_OscInitStruct = {0};
    RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};

    // Настройка HSE
    RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSE;
    RCC_OscInitStruct.HSEState = RCC_HSE_ON;
    RCC_OscInitStruct.PLL.PLLState = RCC_PLL_ON;
    RCC_OscInitStruct.PLL.PLLSource = RCC_PLLSOURCE_HSE;
    RCC_OscInitStruct.PLL.PLLM = 8;
    RCC_OscInitStruct.PLL.PLLN = 336;
    RCC_OscInitStruct.PLL.PLLP = RCC_PLLP_DIV2;
    RCC_OscInitStruct.PLL.PLLQ = 7;
    if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK) {
        Error_Handler();
    }

    // Настройка системного тактирования
    RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK | RCC_CLOCKTYPE_SYSCLK | RCC_CLOCKTYPE_PCLK1 | RCC_CLOCKTYPE_PCLK2;
    RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_PLLCLK;
    RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
    RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV4;
    RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV2;

    if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_5) != HAL_OK) {
        Error_Handler();
    }
}

static void MX_GPIO_Init(void) {
    GPIO_InitTypeDef GPIO_InitStruct = {0};

    // Включение тактирования порта D и порта A
    __HAL_RCC_GPIOD_CLK_ENABLE();
    __HAL_RCC_GPIOA_CLK_ENABLE();

    // Настройка пина PD12 как выхода
    GPIO_InitStruct.Pin = GPIO_PIN_12;
    GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
    GPIO_InitStruct.Pull = GPIO_NOPULL;
    GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_LOW;
    HAL_GPIO_Init(GPIOD, &GPIO_InitStruct);

    // Настройка пина PA0 как входа с подтяжкой к питанию
    GPIO_InitStruct.Pin = GPIO_PIN_0;
    GPIO_InitStruct.Mode = GPIO_MODE_IT_RISING_FALLING;
    GPIO_InitStruct.Pull = GPIO_PULLUP;
    HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);

    // Настройка прерывания EXTI Line0
    HAL_NVIC_SetPriority(EXTI0_IRQn, 0, 0);
    HAL_NVIC_EnableIRQ(EXTI0_IRQn);
}

void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin) {
    if (GPIO_Pin == GPIO_PIN_0) {
        button_pressed = 1;
    }
}

void Error_Handler(void) {
    // Обработка ошибок
    while (1) {
    }
}
```

**Объяснение:**

1. **Инициализация GPIO**:
   ```c
   MX_GPIO_Init();
   ```
   Инициализирует выбранные пины GPIO и настраивает прерывания.

2. **Основной цикл программы**:
   ```c
   while (1) {
       if (button_pressed) {
           HAL_GPIO_TogglePin(GPIOD, GPIO_PIN_12);  // Переключение состояния светодиода
           button_pressed = 0;
           HAL_Delay(200);  // Задержка для предотвращения многократного срабатывания
       }
   }
   ```
   В бесконечном цикле проверяется флаг `button_pressed`. Если флаг установлен, светодиод переключается, флаг
   сбрасывается, и добавляется задержка для предотвращения многократного срабатывания.

3. **Функция инициализации GPIO**:
   ```c
   static void MX_GPIO_Init(void) {
       GPIO_InitTypeDef GPIO_InitStruct = {0};

       // Включение тактирования порта D и порта A
       __HAL_RCC_GPIOD_CLK_ENABLE();
       __HAL_RCC_GPIOA_CLK_ENABLE();

       // Настройка пина PD12 как выхода
       GPIO_InitStruct.Pin = GPIO_PIN_12;
       GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
       GPIO_InitStruct.Pull = GPIO_NOPULL;
       GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_LOW;
       HAL_GPIO_Init(GPIOD, &GPIO_InitStruct);

       // Настройка пина PA0 как входа с подтяжкой к питанию и прерываниями
       GPIO_InitStruct.Pin = GPIO_PIN_0;
       GPIO_InitStruct.Mode = GPIO_MODE_IT_RISING_FALLING;
       GPIO_InitStruct.Pull = GPIO_PULLUP;
       HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);

       // Настройка прерывания EXTI Line0
       HAL_NVIC_SetPriority(EXTI0_IRQn, 0, 0);
       HAL_NVIC_EnableIRQ(EXTI0_IRQn);
   }
   ```
   Включает тактирование портов **GPIOD** и **GPIOA**, настраивает пины **PD12** и **PA0**, и включает прерывания для
   пина **PA0**.

4. **Функция обработки прерываний**:
   ```c
   void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin) {
       if (GPIO_Pin == GPIO_PIN_0) {
           button_pressed = 1;
       }
   }
   ```
   Обрабатывает прерывания от пина **PA0** и устанавливает флаг `button_pressed`.

5. **Функция обработки ошибок**:
   ```c
   void Error_Handler(void) {
       while (1) {
       }
   }
   ```
   Бесконечный цикл для обработки ошибок.


В итоге мы имеем микроконтроллер работающий со светодиодом и кнопкой.