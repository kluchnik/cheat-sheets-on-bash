# Примеры использования утилиты sed

## Множественная замена стройки с пересекающимися символами
```
echo -------------------------
echo 'Множественная замена стройки с пересекающимися символами'
txt='a\na\nb\nb\nc\nc\nd\nd'
echo -------------------------
echo 'Замена производится для последовательности:'
echo -e $txt
echo -------------------------
echo 'Неправильная замена с использованием конвеера: sed '\''s/b/c/'\'' | sed '\''s/c/b/'\'''
echo -e $txt | sed 's/b/c/' | sed 's/c/b/'
echo -------------------------
echo 'Правильная замена: sed '\''s/^b$/c/ ; t ; s/^c$/b/'\'''
echo -e $txt | sed 's/^b$/c/ ; t ; s/^c$/b/'
echo -------------------------
```
Результат выполнения:
```
-------------------------
Множественная замена стройки с пересекающимися символами
-------------------------
Замена производится для последовательности:
a
a
b
b
c
c
d
d
-------------------------
Неправильная замена с использованием конвеера: sed 's/b/c/' | sed 's/c/b/'
a
a
b
b
b
b
d
d
-------------------------
Правильная замена: sed 's/^b$/c/ ; t ; s/^c$/b/'
a
a
c
c
b
b
d
d
-------------------------
```
**Объяснение:**  
sed при работе в цикле читает строки по одной и к каждой строке пытается применить последовательность команд через точку с запятой.  
Обычно в качестве скрипта sed вызывается с одиночной командой sed 's/.../.../' , но вообще это может быть последовательность команд через символ ';' (точку с запятой).  
sed поддерживает управление выполнением. Все решение задачи кроется в команде 't'.  
Она в случае успешного выполнения команды s/// передает управление на опциональную метку, которая может быть ей передана в качестве аргумента.  
Это своеобразный goto. Если аргумента как в данном случае нет, то выполняется goto в конец скрипта, и вторая замена не выполняется.  
По сути реализуется условие ЕСЛИ b заменили на c, ТО ПЕРЕЙТИ в конец скрипта и начать новый цикл со следующей строкой, ИНАЧЕ попытаться выполнить замену c на b.  
