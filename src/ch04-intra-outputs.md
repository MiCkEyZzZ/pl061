# 4.5.1. Внутрикристальные выходы

К внутрикристальным выходам относятся `GPAFIN[7:0]`, `GPIOMIS[7:0]`
и `GPIOINTR`.

## Автономная проверка

Когда контроллер испытывается отдельно:

1. Записать `1` в бит `ITEN`. Это выбирает путь от `GPIOITOP1[7:0]`
   к внутрикристальному выходу `GPIOMIS[7:0]`, а также путь
   от `GPIOITOP3[7:0]` к `GPAFIN[7:0]`.
2. Записать `1`, затем `0` в биты `GPIOITOP1[7:0]` и прочитать
   те же биты в `GPIOMIS[7:0]`.
3. `GPIOINTR` можно прочитать через `GPIOITOP2` независимо
   от `GPIOITCR`.
4. Записать единицы и нули в `GPIOITOP3[7:0]` и прочитать те же
   биты обратно.

## Проверка в составе системы

Когда контроллер встроен в систему:

1. Записать `1` в `ITEN`. Это выбирает тестовый путь от внутренних
   линий `GPIOITOP1[7:0]` к внешним выходам `GPIOMIS[7:0]`,
   а также путь от `GPIOITOP3[7:0]` к `GPAFIN[7:0]`.
2. Переключать биты `GPIOITOP1[7:0]` из `1` в `0`, проверяя связь
   между контроллером прерываний и PL061. Результат читается
   из внутренних тестовых регистров контроллера прерываний.
3. Подавать единицы и нули в `GPIOITOP3[7:0]` и читать те же биты
   из внутреннего регистра приёмника `GPAFIN`, чтобы проверить
   соединение.

## Пример

```c
#define GPIO_ITOP1  (GPIO_BASE + 0x60C)
#define GPIO_ITOP2  (GPIO_BASE + 0x610)
#define GPIO_ITOP3  (GPIO_BASE + 0x614)
#define GPIO_MIS    (GPIO_BASE + 0x418)

static void test_intra_outputs(void)
{
    *(volatile uint32_t *)(GPIO_BASE + 0x600) = 1;   /* ITEN */

    for (int bit = 0; bit < 8; bit++) {
        uint32_t p = 1u << bit;

        *(volatile uint32_t *)GPIO_ITOP1 = p;
        if ((*(volatile uint32_t *)GPIO_MIS & 0xFF) != p)
            report_error(4, bit);

        if (!(*(volatile uint32_t *)GPIO_ITOP2 & 1))
            report_error(5, bit);

        *(volatile uint32_t *)GPIO_ITOP1 = 0;
        if (*(volatile uint32_t *)GPIO_ITOP2 & 1)
            report_error(6, bit);

        *(volatile uint32_t *)GPIO_ITOP3 = p;
        if ((*(volatile uint32_t *)GPIO_ITOP3 & 0xFF) != p)
            report_error(7, bit);
    }
}
```

## Что выявляет

- Обрыв между `GPIOMIS` и контроллером прерываний.
- Неисправность объединения в `GPIOINTR`.
- Ошибку в соединении `GPAFIN`.
