﻿# Кракозябры, кириллица, кодировки и юникод

Настоящей проблемой при локализации всегда были операции с различными наборами символов. Годами, кодируя текстовые строки как последовательности однобайтовых символов с завершающим нулём в конце, большинство программистов так к этому привыкло, что это стало чуть ли не второй их натурой.

## Историческа справка

### Кодировки

Графическое начертание символа определялось по специальной кодировочной таблице. Например, байту 68 соответствовала картинка «D», а байту 119 — картинка «w».

Таким способом можно было представить 256 графических начертаний символов. Интерпретация первых 127 байт в таблицах обычно совпадала, в других таблицах картинка менялись в зависимости языка и локализации. Этих таблиц было создано очень много. Во времена DOS для кириллицы корпорация Микрософт создала кодировку 866, с появлением Windows родилась кодировка 1251, ещё были кодировки ГОСТ и КОИ. Один и тот же байт в разных кодировочных таблицах означал разные картинки.

### Юникод

Всё изменилось, когда тридцать лет назад пришёл юникод. Юникод предложил такую таблицу, в которой содержатся все символы в мире. Символ кодировался беззнаковым целым числом.

Например, числу 61 соответствовал символ «=», числу 1025 — символ «Ё», а числу 8721 — символ «∑».

Для кодирования символа стало недостаточно одного байта. Фундаментальная константа длины одного символа равной одному байту трещала по швам прямо на глазах. Символ теперь занимал произвольное количество байт, размер строки в байтах не был равен количеству символов в строке!

Впрочем, не все программисты согласны с тем, что длину строки теперь следует измерять в символах, а не в байтах.

## Хранение строк в памяти

### Хранение символа в памяти

Как же хранить юникодный символ в памяти, если он не влезает в один байт? Для этого придумали специальные юникодные кодировки.

| Название             | Номер | Количество байт на символ | Суррогатные пары
|----------------------|-------|---------------------------|---------|
| UCS-2                | 1200  | 2                         | Не поддерживаются |
| UTF-16 Little Endian | 1200  | 2 или 4                   | Поддерживаются |
| UTF-16 Big Endian    | 1201  | 2 или 4                   | Поддерживаются |
| UCS-4                |       | 4                         | Не поддерживаются |
| UTF-32 Little Endian |       | 4 или 8                   | Теоретически возможны, но на практике не нужны |
| UTF-32 Big Endian    |       | 4 или 8                   | Теоретически возможны, но на практике не нужны |
| UTF-8                | 65001 | от 1 до 6                 | Не поддерживаются |

Разница между Little Endian (LE) и Big Endian (BE) в порядке следования байт: у Big Endian байты записываются в привычном «от старшего к младшему», у Little Endian — от младшего к старшему.

### В виндоуз

Windows NT в воплощениях 2000, XP, Vista, 7, 8 и 10 — операционные системы целиком и полностью построенные на юникоде. Все базовые функции для создания окон, вывода текста, операций со строками и так далее ожидают передачи юникодных строк. Если какой‐то функции Windows передаётся ANSI‐строка, она сначала преобразуется в юникод и лишь потом передаётся операционной системе. Если ты ждёшь результата функции в виде ANSI‐строки, то операционная система преобразует строку перед возвратом в приложение из юникода в ANSI. Все эти операции протекают скрытно, но на них тратятся лишнее время и память.

Например, функция WriteConsole, вызываемая с ANSI‐строками, должна, выделив дополнительные блоки памяти (в стандартной куче процесса) преобразовать эти строки в юникод и, сохранив результат в выделенных блоках памяти, вызвать юникодную версию WriteConsole. Для функций, заполняющих строками выделенные буферы прежде чем программа сможет их обработать, системе нужно преобразовать строки из юникодных в ANSI. Из‐за этого твоё приложение потребует больше памяти и будет работать медленнее. Поэтому гораздо эффективнее разрабатывать программу с самого начала ориентируясь на юникод.

FreeBASIC как наследник Microsoft QuickBasic по умолчанию считает, что исходный текст написан в кодировке DOS. При выводе русского текста на консоль появляются кракозябры из‐за того, что один и тот же символ имеет разные коды в кодировках 866 и 1251. Эту проблему можно решить несколькими способами.

## Кодировка исходных текстов


