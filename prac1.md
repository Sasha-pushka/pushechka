# Задача 1
Вывести отсортированный в алфавитном порядке список имен пользователей в файле passwd.

```
cut -d: -f1 /etc/passwd | sort
```

<img width="538" alt="Снимок экрана 2024-09-15 в 11 24 43" src="https://github.com/user-attachments/assets/444b260e-aa12-44c2-b6e2-0e195ea705f7">


# Задача 2
Вывести данные /etc/protocols в отформатированном и отсортированном порядке для 5 наибольших портов, как показано в примере ниже:

```
cat /etc/protocols | tail -n 5 | sort -nrk2 | awk '{print $2, $1}'
```

<img width="569" alt="Снимок экрана 2024-09-15 в 11 27 31" src="https://github.com/user-attachments/assets/5b8abefb-2a00-4e88-8f64-0cee78c2790c">


# Задача 3
Написать программу banner средствами bash для вывода текстов, как в следующем примере (размер баннера должен меняться!):
```bash
#!/bin/bash
string=$1
size=${#string}
echo -n "+"
for ((i=-2;i<size;i++))
do
echo -n "-"
done
echo "+"
echo "| $string |"
echo -n "+"
for ((i=-2;i<size;i++))
do
echo -n "-"
done
echo "+"
```
<img width="440" alt="Снимок экрана 2024-09-15 в 13 49 02" src="https://github.com/user-attachments/assets/6c57111d-7b34-4489-b0ee-591b79893e02">



# Задача 4

Написать программу для вывода всех идентификаторов (по правилам C/C++ или Java) в файле (без повторений).

```bash
grep -o '\b[a-zA-Z_][a-zA-Z0-9_]*\b' url.html | sort | uniq
```
<img width="772" alt="Снимок экрана 2024-09-15 в 13 14 57" src="https://github.com/user-attachments/assets/e8ed90b7-11c2-41b2-ad89-a3235dfb35c3">

# Задача 5

Написать программу для регистрации пользовательской команды (правильные права доступа и копирование в /usr/local/bin).


```bash
#!/bin/bash

chmod +x "$1"
sudo cp "$1" /usr/local/bin/
```
<img width="379" alt="Снимок экрана 2024-09-15 в 13 51 31" src="https://github.com/user-attachments/assets/029db845-f1e4-441b-8b5c-a3f4be59f860">

# Задача 6

Написать программу для проверки наличия комментария в первой строке файлов с расширением c, js и py.

```bash
#!/bin/bash

for file in "$@"; do
  # Проверка на наличие допустимого расширения
  if [[ "$file" =~ \.(c|js|py)$ ]]; then
    first_line=$(head -n 1 "$file")

    # Проверка на комментарий в первой строке для разных типов файлов
    if [[ "$file" =~ \.c$ && "$first_line" =~ ^// ]] || \
       [[ "$file" =~ \.js$ && "$first_line" =~ ^// ]] || \
       [[ "$file" =~ \.py$ && "$first_line" =~ ^# ]]; then
      echo "$file has a comment in the first line."
    else
      echo "$file does not have a comment in the first line."
    fi
  fi
done
```
<img width="472" alt="Снимок экрана 2024-09-15 в 14 12 38" src="https://github.com/user-attachments/assets/675ab885-98ac-4b37-8765-25f907e929d2">


# Задача 7

Написать программу для нахождения файлов-дубликатов (имеющих 1 или более копий содержимого) по заданному пути (и подкаталогам).

```bash
#!/bin/bash

find "$1" -type f -exec md5sum {} + | sort | uniq -w32 -dD
```
код вроде правильный, но проверить не могу (На MacOC нету w32)
<img width="547" alt="Снимок экрана 2024-09-15 в 14 00 41" src="https://github.com/user-attachments/assets/f2f2d6a5-9cba-4458-8fed-aa049b4f4dc7">

# Задача 8

Написать программу, которая находит все файлы в данном каталоге с расширением, указанным в качестве аргумента и архивирует все эти файлы в архив tar.

```bash
#!/bin/bash

find . -name "*.$1" -print0 -maxdepth 1 | tar -czvf archive.tar.gz --null -T -
```
<img width="427" alt="Снимок экрана 2024-09-15 в 14 05 00" src="https://github.com/user-attachments/assets/d2d4fa57-41a8-48ac-aba7-05f7dd5f9668">

# Задача 9

Написать программу, которая заменяет в файле последовательности из 4 пробелов на символ табуляции. Входной и выходной файлы задаются аргументами.

```

```


Задача 10

Написать программу, которая выводит названия всех пустых текстовых файлов в указанной директории. Директория передается в программу параметром.

```
#!/bin/bash

find "$1" -type f -empty -name "*.txt"
```
<img width="1097" alt="Снимок экрана 2024-09-15 в 14 25 32" src="https://github.com/user-attachments/assets/25d097d7-2fe8-469a-b1f7-34251cb268c1">
