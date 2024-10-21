## Задача 1

Реализовать на Jsonnet приведенный ниже пример в формате JSON. Использовать в реализации свойство программируемости и принцип DRY.

```json
{
  "groups": [
    "ИКБО-1-20",
    "ИКБО-2-20",
    "ИКБО-3-20",
    "ИКБО-4-20",
    "ИКБО-5-20",
    "ИКБО-6-20",
    "ИКБО-7-20",
    "ИКБО-8-20",
    "ИКБО-9-20",
    "ИКБО-10-20",
    "ИКБО-11-20",
    "ИКБО-12-20",
    "ИКБО-13-20",
    "ИКБО-14-20",
    "ИКБО-15-20",
    "ИКБО-16-20",
    "ИКБО-17-20",
    "ИКБО-18-20",
    "ИКБО-19-20",
    "ИКБО-20-20",
    "ИКБО-21-20",
    "ИКБО-22-20",
    "ИКБО-23-20",
    "ИКБО-24-20"
  ],
  "students": [
    {
      "age": 19,
      "group": "ИКБО-4-20",
      "name": "Иванов И.И."
    },
    {
      "age": 18,
      "group": "ИКБО-5-20",
      "name": "Петров П.П."
    },
    {
      "age": 18,
      "group": "ИКБО-5-20",
      "name": "Сидоров С.С."
    },
    <добавьте ваши данные в качестве четвертого студента>
  ],
  "subject": "Конфигурационное управление"
} 
```

### Решение:

```jsonnet
local x = 18;
local y = "ИКБО-%d-20";

local my_student(age=19, group=y % 10, name="Журавлева А.Х.") =
{
age: age,
group: group,
name: name
};

{
groups: [y % i for i in std.range(1, 24)],
students: [
my_student(19, y % 4, "Иванов И.И."),
my_student(x, y % 5, "Петров П.П."),
my_student(x, y % 5, "Сидоров С.С."),
my_student()
],
subject: "Конфигурационное управление"
}
```

## Задача 2

Реализовать на Dhall приведенный ниже пример в формате JSON. Использовать в реализации свойство программируемости и принцип DRY.

```json
{
  "groups": [
    "ИКБО-1-20",
    "ИКБО-2-20",
    "ИКБО-3-20",
    "ИКБО-4-20",
    "ИКБО-5-20",
    "ИКБО-6-20",
    "ИКБО-7-20",
    "ИКБО-8-20",
    "ИКБО-9-20",
    "ИКБО-10-20",
    "ИКБО-11-20",
    "ИКБО-12-20",
    "ИКБО-13-20",
    "ИКБО-14-20",
    "ИКБО-15-20",
    "ИКБО-16-20",
    "ИКБО-17-20",
    "ИКБО-18-20",
    "ИКБО-19-20",
    "ИКБО-20-20",
    "ИКБО-21-20",
    "ИКБО-22-20",
    "ИКБО-23-20",
    "ИКБО-24-20"
  ],
  "students": [
    {
      "age": 19,
      "group": "ИКБО-4-20",
      "name": "Иванов И.И."
    },
    {
      "age": 18,
      "group": "ИКБО-5-20",
      "name": "Петров П.П."
    },
    {
      "age": 18,
      "group": "ИКБО-5-20",
      "name": "Сидоров С.С."
    },
    <добавьте ваши данные в качестве четвертого студента>
  ],
  "subject": "Конфигурационное управление"
} 
```

### Решение:

```Dhall
let range = https://prelude.dhall-lang.org/List/generate
let index = https://prelude.dhall-lang.org/List/index

let group_with_ind = \(i : Natural) -> "ИКБО-${Natural/show (i + 1)}-20"

let my_Student = 
  \(name : Text) -> 
  \(age : Natural) -> 
  \(group_ind : Natural) ->
{ 
  name = name, 
  group = group_with_ind group_ind, 
  age = age 
}

let group_list : List Text = range 24 Text group_with_ind

let get_group = \(i : Natural) -> index i Text group_list

in
{ 
  groups = group_list, 
  students = [ 
  my_Student "Иванов И.И." 19 3, 
  my_Student "Петров П.П." 18 4, 
  my_Student "Сидоров С.С." 18 4,
  my_Student "Журавлева А.Ж." 19 9
],
  subject = "Конфигурационное управление"
}
```

Для решения дальнейших задач потребуется программа на Питоне, представленная ниже. Разбираться в самом языке Питон при этом необязательно.

```Python
import random


def parse_bnf(text):
    '''
    Преобразовать текстовую запись БНФ в словарь.
    '''
    grammar = {}
    rules = [line.split('=') for line in text.strip().split('\n')]
    for name, body in rules:
        grammar[name.strip()] = [alt.split() for alt in body.split('|')]
    return grammar


def generate_phrase(grammar, start):
    '''
    Сгенерировать случайную фразу.
    '''
    if start in grammar:
        seq = random.choice(grammar[start])
        return ''.join([generate_phrase(grammar, name) for name in seq])
    return str(start)


BNF = '''
E = a
'''

for i in range(10):
    print(generate_phrase(parse_bnf(BNF), 'E'))

```

Реализовать грамматики, описывающие следующие языки (для каждого решения привести БНФ). Код решения должен содержаться в переменной BNF:

## Задача 3

Язык нулей и единиц.

```
10
100
11
101101
000
```

### Решение:

```BNF
E = B | B B E
B = 1 | 0
```

### Результат:

<img width="1007" alt="Снимок экрана 2024-10-21 в 13 19 03" src="https://github.com/user-attachments/assets/a8378eb7-2f27-42bc-9ab8-4b345aaece8f">

## Задача 4

Язык правильно расставленных скобок двух видов.

```
(({((()))}))
{}
{()}
()
{}
```

### Решение:

```BNF
E = L1 R1 | L2 R2 | L1 E R1 | L2 E R2
L1 = [
L2 = {
R1 = ]
R2 = }
```

### Результат:

<img width="1010" alt="Снимок экрана 2024-10-21 в 13 20 11" src="https://github.com/user-attachments/assets/ce2ade26-3937-4730-b101-8547b7f9b600">

## Задача 5

Язык выражений алгебры логики.

```
((~(y & x)) | (y) & ~x | ~x) & x
y & ~(y)
(~(y) & y & ~y)
~x
~((x) & y | (y) | (x)) & x | x | (y & ~y)
```

### Решение:

```BNF
E = var | ~ E | E op E | ( E )
var = x | y
op = & | V
```

### Результат:

<img width="1272" alt="Снимок экрана 2024-10-21 в 13 21 25" src="https://github.com/user-attachments/assets/d16e09c5-a8eb-4829-afff-1e1be4457489">

