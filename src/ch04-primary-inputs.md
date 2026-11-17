# 4.4.2. Первичные входы

Первичные входы `GPIN[7:0]` приходят с контактных площадок кристалла.
Для их проверки применяется интеграционный векторный **trickbox** —
схема на плате или в тестовой среде, которая замыкает выходы
контроллера на его входы.

## Схема шлейфа

В trickbox выходы `nGPEN` и `GPOUT` попарно сворачиваются
по исключающему ИЛИ и подаются на вход `GPIN`:

```text
GPIN[n] = nGPEN[n] XOR GPOUT[n]
```

## Последовательность проверки

1. Записать `1` в бит `ITEN` управляющего регистра.
2. Выбрать аппаратный режим: записать `0xFF` в `GPIOAFSEL`.
3. Подать на выходные линии `nGPEN[7:0]` и `GPOUT[7:0]` шаблоны
   «все единицы», «все нули» и «бегущая единица» через регистры
   `GPIOITIP2[7:0]` и `GPIOITIP1[7:0]` соответственно.
4. Прочитать результат исключающего ИЛИ через `GPIODATA[7:0]`.

## Пример

```c
#define GPIO_ITCR    (GPIO_BASE + 0x600)
#define GPIO_ITIP1   (GPIO_BASE + 0x604)   /* GPOUT  */
#define GPIO_ITIP2   (GPIO_BASE + 0x608)   /* nGPEN  */
#define GPIO_AFSEL   (GPIO_BASE + 0x420)
#define GPIO_DATA    (GPIO_BASE + 0x3FC)

static void test_primary_inputs(void)
{
    *(volatile uint32_t *)GPIO_ITCR  = 1;      /* ITEN */
    *(volatile uint32_t *)GPIO_AFSEL = 0xFF;   /* аппаратный режим */

    for (int bit = 0; bit < 8; bit++) {
        uint32_t pen   = ~(1u << bit) & 0xFF;  /* активный низкий */
        uint32_t dout  = 1u << bit;
        uint32_t expect = (pen ^ dout) & 0xFF;

        *(volatile uint32_t *)GPIO_ITIP2 = pen;
        *(volatile uint32_t *)GPIO_ITIP1 = dout;

        /* Не менее двух передних фронтов PCLK */
        wait_pclk(2);

        uint32_t got = *(volatile uint32_t *)GPIO_DATA & 0xFF;
        if (got != expect)
            report_error(3, bit);
    }

    *(volatile uint32_t *)GPIO_ITCR  = 0;
    *(volatile uint32_t *)GPIO_AFSEL = 0;
}
```

## Важно про адресацию GPIODATA

`GPIODATA` использует младшие биты адреса как маску. Чтобы прочитать
все восемь бит сразу, нужен адрес со смещением `+0x3FC`.

## Важно про задержку

Данные, поступающие на `GPIN[7:0]`, считаются потенциально
асинхронными по отношению к `PCLK`. Поэтому перед чтением `GPIODATA`
должно пройти время двух периодов `PCLK`.

## Что проверяется

| Что подаётся | Что выявляется |
|--------------|----------------|
| Все единицы | Залипание на нуле |
| Все нули | Залипание на единице |
| Бегущая единица | Перепутанные или оборванные линии |
