# 4.5.2. Первичные выходы

Первичные выходы — `nGPEN[7:0]` и `GPOUT[7:0]`. Они проверяются
тем же интеграционным trickbox, что и первичные входы.

## Шлейф

Выходные контакты замыкаются на входные, причём вход получает
исключающее ИЛИ пары выходов:

```text
GPIN[n] = nGPEN[n] XOR GPOUT[n]
```

## Порядок проверки

1. Включить тестовый режим и аппаратное управление: `ITEN = 1`,
   `GPIOAFSEL = 0xFF`.
2. Через `GPIOITIP2[7:0]` задать `nGPEN`, через `GPIOITIP1[7:0]`
   задать `GPOUT`.
3. Прочитать вернувшееся значение через `GPIODATA`.

## Пример

```c
static void test_primary_outputs(void)
{
    *(volatile uint32_t *)(GPIO_BASE + 0x600) = 1;      /* ITEN */
    *(volatile uint32_t *)(GPIO_BASE + 0x420) = 0xFF;   /* AFSEL */

    const uint32_t patterns[] = { 0xFF, 0x00, 0x55, 0xAA };

    for (unsigned i = 0; i < sizeof(patterns) / sizeof(patterns[0]); i++) {
        uint32_t pen  = patterns[i];
        uint32_t dout = ~patterns[i] & 0xFF;

        *(volatile uint32_t *)(GPIO_BASE + 0x608) = pen;
        *(volatile uint32_t *)(GPIO_BASE + 0x604) = dout;

        wait_pclk(2);

        uint32_t got = *(volatile uint32_t *)(GPIO_BASE + 0x3FC) & 0xFF;
        if (got != ((pen ^ dout) & 0xFF))
            report_error(8, i);
    }

    *(volatile uint32_t *)(GPIO_BASE + 0x600) = 0;
    *(volatile uint32_t *)(GPIO_BASE + 0x420) = 0;
}
```

## Замечание о полярности

`nGPEN` активен низким уровнем: ноль разрешает выход. Поэтому
в шаблонах эта линия обычно инвертируется относительно `GPOUT`,
иначе исключающее ИЛИ даст постоянный уровень и проверка окажется
неполной.

## Замечание о задержке

Сигнал, поступивший на `GPIN[7:0]`, потенциально асинхронен
к `PCLK`. Перед чтением `GPIODATA` нужно подать два передних
фронта `PCLK`.
