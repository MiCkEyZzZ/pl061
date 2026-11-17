# 3.3.12. Идентификация GPIOPCellID0-3

Четыре 8-битных регистра в диапазоне `0xFF0`–`0xFFC` образуют
стандартный идентификатор PrimeCell.

## Расположение

| Регистр | Смещение | Значение |
|---------|----------|----------|
| `GPIOPCellID0` | `0xFF0` | `0x0D` |
| `GPIOPCellID1` | `0xFF4` | `0xF0` |
| `GPIOPCellID2` | `0xFF8` | `0x05` |
| `GPIOPCellID3` | `0xFFC` | `0xB1` |

Составное слово: `0xB105F00D`.

## Назначение

Идентификатор `0xB105F00D` закреплён за семейством PrimeCell.
Он позволяет отличить компоненты PrimeCell от периферии других
производителей.

`GPIOPCellID` указывает на семейство, а `GPIOPeriphID` — на
конкретный компонент.

## Проверка

```c
static uint8_t pcell_byte(int i)
{
    return *(volatile uint32_t *)(GPIO_BASE + 0xFF0 + 4 * i) & 0xFF;
}

static int is_primecell(void)
{
    return pcell_byte(0) == 0x0D &&
           pcell_byte(1) == 0xF0 &&
           pcell_byte(2) == 0x05 &&
           pcell_byte(3) == 0xB1;
}
```

## Полная идентификация

```text
1. GPIOPCellID0-3 == 0xB105F00D  → это PrimeCell
2. GPIOPeriphID0-3 == 0x00041061 → это PL061
```

## Замечание

Регистры только для чтения, запись игнорируется. Обращения должны
быть 32-битными, значащий байт маскируется.
