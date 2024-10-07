## Задание 1

Вывести служебную информацию о пакете matplotlib (Python). Разобрать основные элементы содержимого файла со служебной информацией из пакета.

Решение:

```
pip3 show matplotlib
```
![2024-09-30 01 34 42](https://github.com/user-attachments/assets/63283440-2b9c-4e1f-a937-38c69b97a8be)

Получить пакет без менеджера пакетов, прямо из репозитория можно командой

```
git clone https://github.com/matplotlib/matplotlib.git
```

![2024-09-30 01 41 02](https://github.com/user-attachments/assets/1d240162-3245-4554-a88a-e36c91da31fc)


## Задание 2

Вывести служебную информацию о пакете express (JavaScript). Разобрать основные элементы содержимого файла со служебной информацией из пакета.

<img width="1171" alt="Снимок экрана 2024-09-30 в 01 54 06" src="https://github.com/user-attachments/assets/6928a0dc-3b75-4b36-ad52-4392bad4b842">\


Получить пакет без менеджера пакетов, прямо из репозитория можно командой
```

git clone https://github.com/expressjs/express.git
```

<img width="650" alt="Снимок экрана 2024-09-30 в 01 56 12" src="https://github.com/user-attachments/assets/6c26aecc-542d-4252-b950-ccae931b7058">


## Задание 3

Сформировать graphviz-код и получить изображения зависимостей matplotlib и express.

Скачиваем необходимые инструменты:

```
brew install graphviz
```
Для получения изображений зависимости mathplotlib нужно выполнить следующие команды:
```
echo 'digraph G { node [shape=box]; matplotlib [label="matplotlib"]; numpy [label="numpy"]; pillow [label="pillow"]; cycler [label="cycler"]; matplotlib -> numpy; matplotlib -> pillow; matplotlib -> cycler; }' > matplotlib.dot
dot -Tpng matplotlib.dot -o matplotlib.png
open matplotlib.png
```
<img width="1114" alt="Снимок экрана 2024-09-30 в 02 10 23" src="https://github.com/user-attachments/assets/9f91029e-9bf7-42a0-b921-41f690c9b943">

Аналогично с express:
```
echo 'digraph G { node [shape=box]; express [label="express"]; accepts [label="accepts"]; array_flatten [label="array-flatten"]; content_type [label="content-type"]; express -> accepts; express -> array_flatten; express -> content_type; }' > express.dot
dot -Tpng express.dot -o express.png
open express.png
```
<img width="1120" alt="Снимок экрана 2024-09-30 в 02 15 59" src="https://github.com/user-attachments/assets/6e0f04b0-cb26-4c26-b88b-0b1dec471acc">

## Задание 4

Изучить основы программирования в ограничениях. Установить MiniZinc, разобраться с основами его синтаксиса и работы в IDE.

Решить на MiniZinc задачу о счастливых билетах. Добавить ограничение на то, что все цифры билета должны быть различными (подсказка: используйте all_different). Найти минимальное решение для суммы 3 цифр. Код программы:
```
include "alldifferent.mzn"; % импорт ограничения на уникальность значений

array[1..6] of var 0..9: digits; % набор из 6 цифр

constraint sum(digits[1..3]) = sum(digits[4..6]); % условие равенства сумм

constraint all_different(digits); % все цифры должны быть различными

solve minimize sum(digits[1..3]); % минимизация суммы первых трех цифр

output ["digits: \(digits)"]; % вывод значений
```
<img width="786" alt="Снимок экрана 2024-09-30 в 02 22 09" src="https://github.com/user-attachments/assets/6a454e01-2ec8-4a95-86a6-f2515505e275">

## Задание 5

Решить на MiniZinc задачу о зависимостях пакетов для рисунка, приведенного ниже.

![pubgrub](https://github.com/user-attachments/assets/7219fe50-77e4-4719-bcf2-8a98b52d8950)

```
% определение пакетов
  enum PACKAGES = {
      root, menu_1_0_0, menu_1_1_0, menu_1_2_0, menu_1_3_0, menu_1_4_0, menu_1_5_0,
      dropdown_2_0_0, dropdown_2_1_0, dropdown_2_2_0, dropdown_2_3_0, dropdown_1_8_0,
      icons_1_0_0, icons_2_0_0  };
  % переменные, показывающие, установлен пакет (1) или нет (0)
  array[PACKAGES] of var 0..1: installed;
  %  установка root
  constraint
      installed[root] == 1;
  % ограничения зависимостей
  constraint
  (installed[root] == 1) -> (installed[menu_1_0_0] == 1 /\ installed[menu_1_5_0] == 1 /\ installed[icons_1_0_0] == 1) /\
  (installed[menu_1_5_0] == 1) -> (installed[dropdown_2_3_0] == 1 /\ installed[dropdown_2_0_0] == 1) /\
  (installed[menu_1_4_0] == 1) -> (installed[dropdown_2_3_0] == 1 /\ installed[dropdown_2_0_0] == 1) /\
  (installed[menu_1_3_0] == 1) -> (installed[dropdown_2_3_0] == 1 /\ installed[dropdown_2_0_0] == 1) /\
  (installed[menu_1_2_0] == 1) -> (installed[dropdown_2_3_0] == 1 /\ installed[dropdown_2_0_0] == 1) /\
  (installed[menu_1_1_0] == 1) -> (installed[dropdown_2_3_0] == 1 /\ installed[dropdown_2_0_0] == 1) /\
  (installed[menu_1_0_0] == 1) -> (installed[dropdown_1_8_0] == 1) /\
  (installed[dropdown_2_0_0] == 1) -> (installed[icons_2_0_0] == 1) /\
  (installed[dropdown_2_1_0] == 1) -> (installed[icons_2_0_0] == 1) /\
  (installed[dropdown_2_2_0] == 1) -> (installed[icons_2_0_0] == 1) /\
  (installed[dropdown_2_3_0] == 1) -> (installed[icons_2_0_0] == 1);
  % минимизация количества установленных пакетов
  solve minimize sum(installed);
  % вывод результата
  output [ "Installed packages: ", show(installed) ];
```
<img width="1440" alt="Снимок экрана 2024-09-30 в 02 30 54" src="https://github.com/user-attachments/assets/0d7c19cc-1eef-4629-8809-efeb511d8f86">

## Задание 6

Решить на MiniZinc задачу о зависимостях пакетов для следующих данных:
```
root 1.0.0 зависит от foo ^1.0.0 и target ^2.0.0.
foo 1.1.0 зависит от left ^1.0.0 и right ^1.0.0.
foo 1.0.0 не имеет зависимостей.
left 1.0.0 зависит от shared >=1.0.0.
right 1.0.0 зависит от shared <2.0.0.
shared 2.0.0 не имеет зависимостей.
shared 1.0.0 зависит от target ^1.0.0.
target 2.0.0 и 1.0.0 не имеют зависимостей.
```
```
enum PACKAGES = {root,  menu_1_0_0, menu_1_1_0, menu_1_2_0, menu_1_3_0, menu_1_4_0, menu_1_5_0,
 dropdown_2_0_0, dropdown_2_1_0, dropdown_2_2_0, dropdown_2_3_0, dropdown_1_8_0, icons_1_0_0, icons_2_0_0 };
  
  array[PACKAGES] of var 0..1: installed;
  constraint installed[root] == 1;
  
  constraint (installed[root] == 1) -> (installed[menu_1_0_0] == 1 /\ installed[menu_1_5_0] == 1 /\ installed[icons_1_0_0] == 1) /\
      (installed[menu_1_5_0] == 1) -> (installed[dropdown_2_3_0] == 1 /\ installed[dropdown_2_0_0] == 1) /\
      (installed[menu_1_4_0] == 1) -> (installed[dropdown_2_3_0] == 1 /\ installed[dropdown_2_0_0] == 1) /\
      (installed[menu_1_3_0] == 1) -> (installed[dropdown_2_3_0] == 1 /\ installed[dropdown_2_0_0] == 1) /\
      (installed[menu_1_2_0] == 1) -> (installed[dropdown_2_3_0] == 1 /\ installed[dropdown_2_0_0] == 1) /\
      (installed[menu_1_1_0] == 1) -> (installed[dropdown_2_3_0] == 1 /\ installed[dropdown_2_0_0] == 1) /\
      (installed[menu_1_0_0] == 1) -> (installed[dropdown_1_8_0] == 1) /\ (installed[dropdown_2_0_0] == 1) -> (installed[icons_2_0_0] == 1) /\
      (installed[dropdown_2_1_0] == 1) -> (installed[icons_2_0_0] == 1) /\ (installed[dropdown_2_2_0] == 1) -> (installed[icons_2_0_0] == 1) /\
      (installed[dropdown_2_3_0] == 1) -> (installed[icons_2_0_0] == 1);

  solve minimize sum(installed); 
  output [ "Installed packages: ", show(installed) ];
```
<img width="1440" alt="Снимок экрана 2024-09-30 в 02 36 30" src="https://github.com/user-attachments/assets/2261b628-fb51-49a5-847a-158499de9f30">


## Задание 7

Представить на MiniZinc задачу о зависимостях пакетов в общей форме, чтобы конкретный экземпляр задачи описывался только своим набором данных.

Решение:

```
type Version = tuple(int, int, int);
type Package = record(string: name, Version: version);
type Dependency = record(Package: package, Package: require, string: interval);

predicate major_e__minor_e__patch_e(Version:versionV, Version:versionP) = 
  versionV.1==versionP.1 /\ versionV.2==versionP.2 /\ versionV.3==versionV.3;

predicate major_e__minor_e__patch_b(Version:versionV, Version:versionP) = 
  versionV.1==versionP.1 /\ versionV.2==versionP.2 /\ versionV.3>versionV.3;
    
predicate major_e__minor_e__patch_l(Version:versionV, Version:versionP) = 
  versionV.1==versionP.1 /\ versionV.2==versionP.2 /\ versionV.3<versionV.3;
    
predicate major_e__minor_b(Version:versionV, Version:versionP) =
  versionV.1==versionP.1 /\ versionV.2>versionP.2;
  
predicate major_e__minor_l(Version:versionV, Version:versionP) = 
  versionV.1==versionP.1 /\ versionV.2<versionP.2;

predicate major_b(Version:versionV, Version:versionP) = 
  versionV.1>versionP.1;

predicate major_l(Version:versionV, Version:versionP) = 
  versionV.1<versionP.1;

array[_] of Package: packages;
array[_] of Dependency: dependencies;
string: target_name;

int: N = length(packages);
int: M = length(dependencies);

%%% Массив установленных пакетов (1 - установлен, 0 - не установлен)
array[1..N] of var 0..1: installed;

%%% Одноименный пакет должен быть установлен не более 1 версии
constraint forall(i in 1..N)(sum(j in 1..N where packages[i].name == packages[j].name)(installed[j]) <= 1);
  
%%% Целевой пакет должен быть установлен одной версии
constraint (sum(j in 1..N where target_name == packages[j].name)(installed[j]) == 1);

%%% Ограничение на зависимости
% Для всех установленных пакетов
constraint forall (p in 1..N where installed[p] == 1)  
(
  % Для всех зависимостей, которые нужно для пакета p
  forall(d in 1..M where dependencies[d].package == packages[p]) 
  (
    % Существует хотя бы одна другая зависимость с таким же пакетом и таким же именем требуемого пакета
    exists(ad in 1..M where dependencies[ad].require.name == dependencies[d].require.name /\ dependencies[ad].package == dependencies[d].package) 
    (
      % Такая, что для этой зависимости существует установленный пакет, у которого такое же имя и подходящая версия под интервал
      exists(dp in 1..N where packages[dp].name == dependencies[ad].require.name) 
      (
        installed[dp] == 1 /\
        (
          if dependencies[ad].interval = "^"
          then (    
            major_e__minor_e__patch_e(packages[dp].version, dependencies[ad].require.version) \/
            major_e__minor_e__patch_b(packages[dp].version, dependencies[ad].require.version) \/
            major_e__minor_b(packages[dp].version, dependencies[ad].require.version)
          )
          elseif dependencies[ad].interval = "~"
          then (    
            major_e__minor_e__patch_e(packages[dp].version, dependencies[ad].require.version) \/
            major_e__minor_e__patch_b(packages[dp].version, dependencies[ad].require.version)
          )
          elseif dependencies[ad].interval = ">="
          then (
            major_e__minor_e__patch_e(packages[dp].version, dependencies[ad].require.version) \/
            major_e__minor_e__patch_b(packages[dp].version, dependencies[ad].require.version) \/
            major_e__minor_b(packages[dp].version, dependencies[ad].require.version) \/
            major_b(packages[dp].version, dependencies[ad].require.version)
          )
          elseif dependencies[ad].interval = ">"
          then (
            major_e__minor_e__patch_b(packages[dp].version, dependencies[ad].require.version) \/
            major_e__minor_b(packages[dp].version, dependencies[ad].require.version) \/
            major_b(packages[dp].version, dependencies[ad].require.version)
          )
          elseif dependencies[ad].interval = "<="
          then (
            major_e__minor_e__patch_e(packages[dp].version, dependencies[ad].require.version) \/
            major_e__minor_e__patch_l(packages[dp].version, dependencies[ad].require.version) \/
            major_e__minor_l(packages[dp].version, dependencies[ad].require.version) \/
            major_l(packages[dp].version, dependencies[ad].require.version)
          )
          elseif dependencies[ad].interval = "<"
          then (
            major_e__minor_e__patch_l(packages[dp].version, dependencies[ad].require.version) \/
            major_e__minor_l(packages[dp].version, dependencies[ad].require.version) \/
            major_l(packages[dp].version, dependencies[ad].require.version)
          )
          else major_e__minor_e__patch_e(packages[dp].version, dependencies[ad].require.version)
          endif
        )
      )
    )
  )
);

solve minimize sum(i in 1..N)(installed[i]);

output["Целевой пакет: \(target_name)\n"];
output["\nИсходные зависимости:\n"];
output["\(dependencies[i])\n" | i in 1..M];
output["\nУстановленные пакеты (1 - установлен, 0 - не установлен):\n"];
output["\(installed[i]): \(packages[i])\n" | i in 1..N];
```

Входные данные:

```
% Пакет, который требуется установить
%
% target_name = <ИМЯ-ЦЕЛЕВОГО-ПАКЕТА>, где <ИМЯ-ЦЕЛЕВОГО-ПАКЕТА> - строчное наименование пакета
%
% Пример:
target_name = "root";


% Пакеты, доступные для установки
%
% Пакет записывается в виде:
% (name: <ИМЯ-ПАКЕТА>, version: <ВЕРСИЯ-ПАКЕТА>)
%
% <ИМЯ-ПАКЕТА> - строчное наименование пакета
%
% <ВЕРСИЯ-ПАКЕТА> записывается в виде (<a>,<b>,<c>), 
% где <a> - мажорная версия, <b> - минорная версия, <c> - патч-версия
%
% Пример:
packages = [
  (name: "root", version: (1,0,0)), 
  (name: "root", version: (1,1,0)),
  
  (name: "foo", version: (1,0,0)),
  (name: "foo", version: (1,2,3)),
  (name: "foo", version: (2,5,0))
];

% Зависимости, требуемые для установки пакета

% Зависимость записывается в виде:
% (package: <УСТАНАВЛИВАЕМЫЙ-ПАКЕТ>, require: <ТРЕБУЕМЫЙ-ПАКЕТ>, interval: <ИНТЕРВАЛ-ВЕРСИЙ>)
%
% <УСТАНАВЛИВАЕМЫЙ-ПАКЕТ> - Конкретный пакет, для которого требуется установка другого пакета. 
% Вид записи представлен выше
%
% <ТРЕБУЕМЫЙ-ПАКЕТ> - пакет, установка которого требуется. 
% Указывается требуемое имя, версия пакета используется для интервала
% Вид записи представлен выше
%
% <ИНТЕРВАЛ-ВЕРСИЙ> - строчное указание интервала, доступные интервалы:
% "^"; "~", ">=", ">", "<=", "<". При указании любой другой строки будет использован интервал "="
%
% Пример:
dependencies = [
  (package: (name: "root", version: (1,0,0)), require: (name: "foo", version: (1,0,0)), interval: "^"),
  (package: (name: "root", version: (1,0,0)), require: (name: "foo", version: (2,0,0)), interval: "~"),
  
  (package: (name: "root", version: (1,1,0)), require: (name: "foo", version: (3,3,3)), interval: "<=")
];
```
