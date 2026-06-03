---
title: Пошук груп малих порядків
teaching: 40
exercises: 15
---

::::::::::::::::::::::::::::::::::::::: objectives

- Використання бібліотеки груп малих порядків (Small Groups Library)
- Створення взаємоповʼязаних функцій

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- Модульне програмування
- Пошук прикладів та контрприкладів серед груп заданого порядку

::::::::::::::::::::::::::::::::::::::::::::::::::

У цьому розділі ми знайдемо нетривіальні групи з цікавою
властивістю, а саме, коли середній порядок елементів групи є цілим числом.

Дистрибутив GAP включає різноманітні бібліотеки даних (щоб їх знайти, наприклад, введіть слово "library" у рядок пошуку у [переліку пакетів, що розповсюджуються з GAP](https://www.gap-system.org/packages/)).
Однією з них є бібліотека груп малих порядків ([Small Groups Library](https://www.gap-system.org/Packages/sgl.html)) Ганса Ульріха Беше, Беттіни Айк та Імонна О'Брайена.

Бібліотека Small Groups Library надає інструментарій для визначення того, яка інформація
у ній зберігається, і надсилання запитів для пошуку груп з потрібними
властивостями. Основними функціями для запиту інформації є `SmallGroup`, `AllSmallGroups`,
`NrSmallGroups`, `SmallGroupsInformation` та `IdGroup`, наприклад:

```output
gap> NrSmallGroups(64);
267
gap> SmallGroupsInformation(64);

  There are 267 groups of order 64.
  They are sorted by their ranks.
     1 is cyclic.
     2 - 54 have rank 2.
     55 - 191 have rank 3.
     192 - 259 have rank 4.
     260 - 266 have rank 5.
     267 is elementary abelian.

  For the selection functions the values of the following attributes
  are precomputed and stored:
     IsAbelian, PClassPGroup, RankPGroup, FrattinifactorSize and
     FrattinifactorId.

  This size belongs to layer 2 of the SmallGroups library.
  IdSmallGroup is available for this size.

gap> G:=SmallGroup(64,2);

gap> AllSmallGroups(Size,64,NilpotencyClassOfGroup,5);
[  ,  ,
   ]
gap> List(last,IdGroup);
[ [ 64, 52 ], [ 64, 53 ], [ 64, 54 ] ]
```

Ми хотіли б використати нашу функцію для перевірки властивостей групи. Створимо цю функцію, використовуючи вбудовану нотацію для функцій з одним аргументом:

```gap
TestOneGroup := G -> IsInt( AvgOrdOfGroup(G) );
```

Тепер спробуйте, наприклад,

```gap
List([TrivialGroup(),Group((1,2))],TestOneGroup);
```

```output
[ true, false ]
```

```gap
gap> AllSmallGroups(Size,24,TestOneGroup,true);
```

```output
[ ]
```

:::::::::::::::::::::::::::::::::::::::::  callout

## Тут починається модульне програмування

Чому повертати логічні значення краще, ніж просто друкувати інформацію чи повертати рядок, наприклад, `"YES"`?

::::::::::::::::::::::::::::::::::::::::::::::::::

Наведемо простий приклад функції, яка перевіряє всі групи заданого порядку.
Ми створюємо одну групу на кожній ітерації, перевіряємо потрібну властивість та повертаємо ідентифікатор цієї групи, як тільки буде виявлено приклад. Якщо приклад не знайдено, функція повертає `fail`, що є особливим типом
логічної змінної в GAP.

```gap
TestOneOrderEasy := function(n)
local i;
for i in [1..NrSmallGroups(n)] do
  if TestOneGroup( SmallGroup( n, i ) ) then
    return [n,i];
  fi;
od;
return fail;
end;
```

Наприклад,

```gap
TestOneOrderEasy(1);
```

```output
[ 1, 1 ]
```

```gap
TestOneOrderEasy(24);
```

```output
fail
```

:::::::::::::::::::::::::::::::::::::::::  callout

## `AllSmallGroups` не вистачає пам'яті - що робити?

- Використовуйте цикл, як показано у функції вище, щоб створювати тільки одну групу на кожній ітерації
- Використовуйте функцію `IdsOfAllSmallGroups`, яка приймає ті самі аргументи, що й `AllSmallGroups`, але повертає ідентифікатори груп замість самих груп.

::::::::::::::::::::::::::::::::::::::::::::::::::

Перебір елементів зі списку `[1..NrSmallGroups(n)]` дає вам більше гнучкості, якщо вам потрібно
більш детально контролювати хід обчислення. Наприклад, наступна версія
нашої функції друкує додаткову інформацію про номер
групи, що розглядається. Вона також сприймає функцію для перевірки однієї групи як аргумент (як ви думаєте, чому це краще?).

```gap
TestOneOrder := function(f,n)
local i, G;
for i in [1..NrSmallGroups(n)] do
  Print(n, ":", i, "/", NrSmallGroups(n), "\r");
  G := SmallGroup( n, i );
  if f(G) then
    Print("\n");
    return [n,i];
  fi;
od;
Print("\n");
return fail;
end;
```

Наприклад,

```gap
TestOneOrder(TestOneGroup,64);
```

відображає лічильник ітерацій під час обчислення, а потім повертає `fail`:

```output
64:267/267
fail
```

Наступним кроком є інтеграція `TestOneOrder` у функцію, яка перевірить
усі порядки від 2 до `n` і зупиниться, щойно знайде приклад
групи, середній порядок елемента якої є цілим числом:

```gap
TestAllOrders:=function(f,n)
local i, res;
for i in [2..n] do
  res:=TestOneOrder(f,i);
  if res <> fail then
    return res;
  fi;
od;
return fail;
end;
```

Ми бачимо, що існує така група порядку 105:

```gap
TestAllOrders(TestOneGroup,128);
```

```output
2:1/1
3:1/1
4:2/2
5:1/1
6:2/2
7:1/1
8:5/5
...
...
...
100:16/16
101:1/1
102:4/4
103:1/1
104:14/14
105:1/2
[ 105, 1 ]
```

Щоб дослідити цю групу, ми можемо отримати її `StructureDescription` (див.
[документацію](https://www.gap-system.org/Manuals/doc/ref/chap39.html#X87BF1B887C91CA2E)
для пояснення нотації, яку використано у `StructureDescription`):

```gap
G:=SmallGroup(105,1); AvgOrdOfGroup(G); StructureDescription(G);
```

```output
17
"C5 x (C7 : C3)"
```

а потім перетворити її на скінченно представлену групу, щоб побачити її твірні та співвідношення:

```gap
H:=SimplifiedFpGroup(Image(IsomorphismFpGroup(G)));
RelatorsOfFpGroup(H);
```

```output
[ F1^3, F2^-1*F1^-1*F2*F1, F3^-1*F2^-1*F3*F2, F3^-1*F1^-1*F3*F1*F3^-1, F2^5,
F3^7 ]
```

Тепер ми розглянемо "більші" групи, починаючи з порядку 106 (але спочатку перевіримо,
що інша група порядку 105 не має такої властивості)

```gap
List(AllSmallGroups(105),AvgOrdOfGroup);
```

```output
[ 17, 301/5 ]
```

Щоб уникнути зайвих обчислень, додамо додатковий аргумент, який визначає порядок груп,
з якого потрібно починати:

```gap
TestRangeOfOrders:=function(f,n1,n2)
local n, res;
for n in [n1..n2] do
  res:=TestOneOrder(f,n);
  if res <> fail then
    return res;
  fi;
od;
return fail;
end;
```

Але тепер ми викликаємо

```gap
TestRangeOfOrders(TestOneGroup,106,256);
```

і виявляємо, що тестування 2328 груп порядку 128 і додатково 56092
груп порядку 256 вже займає занадто багато часу.

:::::::::::::::::::::::::::::::::::::::::  callout

## Без паніки!

Ви можете перервати роботу GAP, натиснувши Ctrl-C один раз. Після цього GAP виведе запрошення `brk>`. Ви можете перевірити поточні значення змінних, щоб краще зрозуміти контекст виконання. Для того, щоб вийти до головної сесії GAP, треба ввести `quit;` (стережіться натискання Ctrl-C двічі протягом секунди – це повністю завершить сеанс GAP).

::::::::::::::::::::::::::::::::::::::::::::::::::

Це ще одна ситуація, коли теоретичні знання допомагають набагато більше,
ніж підхід "грубої сили". Якщо група є _p_\-групою, то порядок кожного
класу спряженості нетотожного елемента групи ділиться на _p_;
а отже, середній порядок елемента групи не може бути цілим числом. Тому _p_\-групи можна не розглядати. Отже, нова версія коду має такий вигляд:

```gap
TestRangeOfOrders:=function(f,n1,n2)
local n, res;
for n in [n1..n2] do
  if not IsPrimePowerInt(n) then
     res:=TestOneOrder(f,n);
     if res <> fail then
       return res;
     fi;
   fi;
od;
return fail;
end;
```

Тепер ми можемо знайти групу порядку 357 з тією ж властивістю:

```gap
gap> TestRangeOfOrders(TestOneGroup,106,512);
```

```output
106:2/2
108:45/45
...
350:10/10
351:14/14
352:195/195
354:4/4
355:2/2
356:5/5
357:1/2
[ 357, 1 ]
```

Наступна функція демонструє ще більшу гнучкість: вона є варіативною, тобто
вона може приймати два або більше аргументів, перші два з яких будуть зберігатися у змінних `f` та `n`, а решта аргументів буде доступна у списку `r`
(що позначається трьома крапками `...` після `r`). Перший аргумент —
функція перевірки однієї групи, другий — порядок груп для перевірки, третій і четвертий —
номери першої та останньої груп цього порядку, які потрібно
перевірити. За замовчуванням останні два аргументи дорівнюють 1 та `NrSmallGroups(n)`, відповідно. Ця функція також показує, як перевірити вхідні дані
та створювати зручні повідомлення про помилки у випадку неприпустимих аргументів.

Крім того, ця функція демонструє, як використовувати повідомлення `Info`,
які можна вмикати та вимикати, встановивши відповідний рівень `Info`. Проблема, яку ми намагаємось вирішити, полягає в тому, щоб мати можливість легко перемикати рівні деталізації виводу замість того, щоб окремо "закоментовувати" та "розкоментовувати" кожний рядок з командою `Print`. Це досягається шляхом створення інформаційного класу (Info class):

```gap
gap> InfoSmallGroupsSearch := NewInfoClass("InfoSmallGroupsSearch");
```

```output
InfoSmallGroupsSearch
```

Тепер замість `Print("something");` можна використовувати
`Info( InfoSmallGroupsSearch, infolevel, "something");`,
де `infolevel` є додатним цілим числом, що визначає рівень деталізації.
Цей рівень можна змінити на `n` за допомогою команди
`SetInfoLevel( InfoSmallGroupsSearch, n);`. Зверніть увагу, як `Info` викликається у коді нижче:

```gap
TestOneOrderVariadic := function(f,n,r...)
local n1, n2, i;

if not Length(r) in [0..2] then
  Error("The number of arguments must be 2,3 or 4\n" );
fi;

if not IsFunction( f ) then
  Error("The first argument must be a function\n" );
fi;

if not IsPosInt( n ) then
  Error("The second argument must be a positive integer\n" );
fi;

if IsBound(r[1]) then
  n1:=r[1];
  if not n1 in [1..NrSmallGroups(n)] then
    Error("The 3rd argument, if present, must belong to ", [1..NrSmallGroups(n)], "\n" );
  fi;
else
  n1:=1;
fi;

if IsBound(r[2]) then
  n2:=r[2];
  if not n2 in [1..NrSmallGroups(n)] then
    Error("The 4th argument, if present, must belong to ", [1..NrSmallGroups(n)], "\n" );
  elif n2 < n1 then
    Error("The 4th argument, if present, must be greater or equal to the 3rd \n" );
  fi;
else
  n2:=NrSmallGroups(n);
fi;

Info( InfoSmallGroupsSearch, 1,
      "Checking groups ", n1, " ... ", n2, " of order ", n );
for i in [n1..n2] do
  if InfoLevel( InfoSmallGroupsSearch ) > 1 then
    Print(i, "/", NrSmallGroups(n), "\r");
  fi;
  if f(SmallGroup(n,i)) then
    Info( InfoSmallGroupsSearch, 1,
          "Discovered counterexample: SmallGroup( ", n, ", ", i, " )" );
    return [n,i];
  fi;
od;
Info( InfoSmallGroupsSearch, 1,
      "Search completed - no counterexample discovered" );
return fail;
end;
```

У наступному прикладі показано, як тепер можна керувати
виводом, перемикаючи рівень деталізації для `InfoSmallGroupsSearch`:

```output
gap> TestOneOrderVariadic(IsIntegerAverageOrder,24);
fail
gap> SetInfoLevel( InfoSmallGroupsSearch, 1 );
gap> TestOneOrderVariadic(IsIntegerAverageOrder,24);
#I  Checking groups 1 ... 15 of order 24
#I  Search completed - no counterexample discovered
fail
gap> TestOneOrderVariadic(IsIntegerAverageOrder,357);
#I  Checking groups 1 ... 2 of order 357
#I  Discovered counterexample: SmallGroup( 357, 1 )
[ 357, 1 ]
gap> SetInfoLevel( InfoSmallGroupsSearch, 0);
gap> TestOneOrderVariadic(IsIntegerAverageOrder,357);
[ 357, 1 ]
```

Звичайно, це вносить деякі складнощі для тестового файлу,
який порівнює фактичний вивід з еталонним виводом. Щоб вирішити
цю проблему, ми будемо запускати тести на інформаційному рівні 0, щоб заблокувати
всі додаткові виводи. Оскільки сеанс GAP, в якому ми запускаємо тести, вже може використовувати інший рівень деталізації, ми запам’ятаємо цей рівень, щоб відновити його після тесту:

```output
# Finding groups with integer average order
gap> INFO_SSS:=InfoLevel(InfoSmallGroupsSearch);;
gap> SetInfoLevel( InfoSmallGroupsSearch, 0);
gap> res:=[];;
gap> for n in [1..360] do
>      if not IsPrimePowerInt(n) then
>        t := TestOneOrderVariadic( IsIntegerAverageOrder,n,1,NrSmallGroups(n) );
>        if t <> fail then
>          Add(res,t);
>        fi;
>      fi;
>    od;
gap> res;
[ [ 1, 1 ], [ 105, 1 ], [ 357, 1 ] ]
gap> SetInfoLevel( InfoSmallGroupsSearch, INFO_SSS);
```

:::::::::::::::::::::::::::::::::::::::  challenge

## Чи містить бібліотека Small Groups Library іншу групу з цією властивістю?

- Що можна сказати про порядок знайдених груп із цією властивістю?

- Чи можна оцінити, скільки часу може зайняти перевірка всіх 408641062 груп порядку 1536?

- Скільки груп порядку не вище 2000 можна було б перевірити,
  за винятком _p_\-груп і груп порядку 1536?

- Чи можна знайти іншу групу з цією властивістю в бібліотеці Small Groups Library, за винятком груп порядку 1536?

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- Організовуйте код у вигляді функцій.
- Викликайте групи малих порядків з бібліотеки послідовно замість того, щоб створювати величезний список усіх груп одного порядку.
- Використання `SmallGroupsInformation` може допомогти зменшити кількість варіантів для пошуку.
- GAP не є чарівним інструментом: теоретичні знання можуть допомогти набагато більше, ніж підхід "грубої сили".

::::::::::::::::::::::::::::::::::::::::::::::::::


