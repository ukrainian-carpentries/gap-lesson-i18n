---
title: Перша сесія з GAP
teaching: 30
exercises: 10
---

::::::::::::::::::::::::::::::::::::::: objectives

- Поради та підказки, які заощадять час
- Використання довідкової системи GAP
- Базові об'єкти та конструкції в мові GAP

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- Working with the GAP command line

::::::::::::::::::::::::::::::::::::::::::::::::::

Якщо GAP встановлено правильно, ви повинні мати можливість його запустити. Як саме це зробити, залежатиме від вашої операційної системи та способу встановлення GAP. Після запуску, GAP виведе на екран свій _банер_, який відображає інформацію про версію системи та завантажені компоненти, а потім запрошення командного рядка `gap>`, наприклад:

```output
 ┌───────┐   GAP 4.9.2 of 04-Jul-2018
 │  GAP  │   https://www.gap-system.org
 └───────┘   Architecture: x86_64-apple-darwin16.7.0-default64
 Configuration:  gmp 6.1.2, readline
 Loading the library and packages ...
 Packages:   AClib 1.3, Alnuth 3.1.0, AtlasRep 1.5.1, AutPGrp 1.9,
             Browse 1.8.8, CRISP 1.4.4, Cryst 4.1.17, CrystCat 1.1.8,
             CTblLib 1.2.2, FactInt 1.6.2, FGA 1.4.0, GAPDoc 1.6.1, IO 4.5.1,
             IRREDSOL 1.4, LAGUNA 3.9.0, Polenta 1.3.8, Polycyclic 2.14,
             PrimGrp 3.3.1, RadiRoot 2.8, ResClasses 4.7.1, SmallGrp 1.3,
             Sophus 1.24, SpinSym 1.5, TomLib 1.2.6, TransGrp 2.0.2,
             utils 0.54
 Try '??help' for help. See also '?copyright', '?cite' and '?authors'
gap>
```

Щоб вийти з GAP, введіть `quit;` у командному рядку GAP. Пам’ятайте, що всі команди GAP, включно з цією, мають закінчуватися крапкою з комою! Потренуйтеся вводити `quit;`, щоб вийти з GAP, а потім починати новий сеанс GAP. Перш ніж продовжити, ви можливо забажаєте ввести наступну команду, щоб відображати запрошення GAP та команди, введені користувачем у різних кольорах:

```gap
 ColorPrompt(true);
```

Найпростіший шлях розпочати роботу з GAP - це використовувати GAP як калькулятор:

```gap
( 1 + 2^32 ) / (1 - 2*3*107 );
```

```output
-6700417
```

Якщо ви хочете записати те, що ви робили під час сеансу GAP, щоб ви могли переглянути це пізніше, ви можете ввімкнути ведення журналу за допомогою функції `LogTo`, як наведено далі.

```gap
LogTo("gap-intro.log");
```

Це створить файл `gap-intro.log` у поточному каталозі, який міститиме всі подальші вхідні та вихідні дані, які з’являтимуться у вашому терміналі.
Щоб припинити ведення журналу, ви можете викликати `LogTo` без аргументів, як у `LogTo();`, або залишити GAP. Зауважте, що `LogTo` очищає файл перед запуском, якщо він уже існує!

Може бути корисним залишити кілька коментарів у файлі журналу на випадок,
якщо ви повернетеся до нього в майбутньому. Коментар у GAP починається з символу `#` і
продовжується до кінця рядка. Ви можете ввести наступне після
підказки GAP:

```gap
# Урок Software Carpentry GAP
```

тоді після натискання клавіші Return GAP відобразить нову підказку, але коментар
буде записаний у файл журналу.

Файл журналу записує всю взаємодію з GAP, яка відбувається після виклику
`LogTo`, але не раніше. Ми можемо повторити наші обчислення вище,
якщо ми також хочемо їх записати. Замість того, щоб вводити їх повторно, ми будемо використовувати клавіші зі стрілками вгору та вниз
для прокручування _історії командного рядка_. Повторюйте це, доки знову не побачите
формулу, потім натисніть Return (розташування курсору в командному
рядку не має значення):

```gap
( 1 + 2^32 ) / (1 - 2*3*107 );
```

```output
-6700417
```

Ви також можете редагувати команди, що вже існують. Натисніть клавішу «Вгору» ще раз, а потім використовуйте
клавіші зі стрілками вліво та вправо, Delete або Backspace, щоб відредагувати їх та замінити 32 на 64 (інші корисні комбінації клавіш —
Ctrl-A та Ctrl-E, щоб перемістити курсор на початок і кінець
рядку, відповідно). Тепер натисніть клавішу Return (у будь-якій позиції
курсору в командному рядку):

```gap
( 1 + 2^64 ) / (1 - 2*3*107 );
```

```output
-18446744073709551617/641
```

Корисно знати, що якщо історія командного рядка велика, можна
виконати частковий пошук, ввівши початкову частину команди, а потім використовуючи
клавіші зі стрілками вгору та вниз, щоб прокрутити лише ті рядки, які починаються
з введених символів.

Якщо ви бажаєте зберегти значення для подальшого використання, ви можете присвоїти йому ім'я
за допомогою `:=`

```gap
universe := 6*7;
```

:::::::::::::::::::::::::::::::::::::::::  callout

## Оператори `:=`, `=` та `<>`

- В інших мовах Ви можете бути більш знайомі з використанням `=`, щоб присвоювати значення змінним, але GAP використовує `:=`.

- GAP використовує `=` для порівняння, якщо дві об'єкти однакові (де інші мови можуть використовувати `==`).

- Нарешті, GAP використовує `<>`, щоб перевірити, чи два об'єкти не рівні (замість `!=`, що ви могли бачити раніше).

::::::::::::::::::::::::::::::::::::::::::::::::::

Пробільні символи (тобто пробіли, табуляції та символи переводу рядка) не мають значення в GAP,
за винятком випадків, коли вони знаходяться всередині рядка. Наприклад, попереднє введеня
можна ввести без пробілів:

```gap
(1+2^64)/(1-2*3*107);
```

```output
-18446744073709551617/641
```

Пробіли часто використовуються для форматування більш складних команд
для кращої читабельності. Наприклад, наступне введення, яке створює матрицю 3×3: For example, the following input which creates a 3×3 matrix:

```gap
m:=[[1,2,3],[4,5,6],[7,8,9]];
```

```output
[ [ 1, 2, 3 ], [ 4, 5, 6 ], [ 7, 8, 9 ] ]
```

Замість цього ми можемо записати нашу матрицю в 3 рядки. У цьому випадку замість повної підказки
`gap>` відображатиметься часткова підказка `>`, доки користувач не завершить
введення крапкою з комою:

```gap
gap> m:=[[ 1, 2, 3 ],
>        [ 4, 5, 6 ],
>        [ 7, 8, 9 ]];
```

```output
[ [ 1, 2, 3 ], [ 4, 5, 6 ], [ 7, 8, 9 ] ]
```

Ви можете використовувати `Display` для красивого друку змінних, включаючи цю матрицю:

```gap
Display(m);
```

```output
[ [  1,  2,  3 ],
  [  4,  5,  6 ],
  [  7,  8,  9 ] ]
```

Загалом функції GAP, як наприклад `LogTo` і `Display`, викликаються за допомогою дужок,
які містять (можливо, порожній) список аргументів.

:::::::::::::::::::::::::::::::::::::::::  callout

## Функції також є об’єктами GAP

Check what happens if you forget to add brackets,
e.g. type `LogTo;` and `Factorial;`
We will explain the differences in these outputs later.

::::::::::::::::::::::::::::::::::::::::::::::::::

Нижче наведені декілька прикладів виклику інших функцій GAP:

```gap
Factorial(100);
```

```output
93326215443944152681699238856266700490715968264381621468\
59296389521759999322991560894146397615651828625369792082\
7223758251185210916864000000000000000000000000
```

(точна ширина виводу залежатиме від налаштувань вашого терміналу),

```gap
Determinant(m);
```

```output
0
```

та

```gap
Factors(2^64-1);
```

```output
[ 3, 5, 17, 257, 641, 65537, 6700417 ]
```

Функції можна комбінувати різними способами та
використовувати як аргументи інших функцій, наприклад,
функція `Filtered` приймає список і функцію, повертаючи
всі елементи списку, які задовольняють функцію.
`IsEvenInt` ("Is Even Integer" з англ. "чи є ціле число парним"), як не дивно, перевіряє, чи є ціле число парним!

```gap
Filtered( [2,9,6,3,4,5], IsEvenInt);
```

```output

```

Корисною функцією інтерфейсу командного рядка GAP, яка економить час, є заповнення
ідентифікаторів під час натискання клавіші Tab.  Наприклад, введіть `Fib`, а потім
натисніть клавішу Tab, щоб завершити введення `Fibonacci`:

```gap
Fibonacci(100);
```

```output
354224848179261915075
```

У випадку, якщо унікальне доповнення неможливе, GAP спробує виконати
часткове доповнення, а натискання клавіші Tab вдруге відобразить усі можливі
доповнення ідентифікатора. Спробуйте, наприклад, ввести `GroupHomomorphismByImages`
або `NaturalHomomorphismByNormalSubgroup` за допомогою доповнення.

Сподіваємось, те, як функції називаються в GAP, допоможе вам запам’ятовувати або навіть вгадувати назви
бібліотечних функцій. Якщо назва змінної складається з кількох слів,
то перша літера кожного слова пишеться з великої літери (пам’ятайте, що GAP чутливий до регістру!).
Подальші відомості про правила іменування, які використовуються в GAP,
задокументовані в посібнику GAP [тут](http://www.gap-system.org/Manuals/doc/ref/chap5.html#X81F732457F7BC851).
Функції з назвами `У_ВЕРХНЬОМУ_РЕГІСТРІ` є внутрішніми функціями, не призначеними
для загального використання.  Використовуйте їх з особливою обережністю!

Важливо пам’ятати, що GAP чутливий до регістру. Наприклад, наступне
введення викликає помилку:

```gap
factorial(100);
```

```error
Error, Variable: 'factorial' must have a value
not in any function at line 14 of *stdin*
```

тому що назва функції бібліотеки GAP – `Factorial`. Використання малих літер
замість великих або навпаки також впливає на доповнення назви.

Тепер давайте розглянемо таку задачу: для скінченної групи _G_ обчислити
середній порядок її елементів (тобто суму порядків її елементів, поділену
на порядок групи). З чого почати?

Введіть `?Group`, і ви побачите всі записи довідкової системи, що починаються з `Group`:

```output
┌──────────────────────────────────────────────────────────────────────────────┐
│   Choose an entry to view, 'q' for none (or use ?<nr> later):                │
│[1]    AutoDoc (not loaded): @Group                                           │
│[2]    loops (not loaded): group                                              │
│[3]    polycyclic: Group                                                      │
│[4]    RCWA (not loaded): Group                                               │
│[5]    Tutorial: Groups and Homomorphisms                                     │
│[6]    Tutorial: Group Homomorphisms by Images                                │
│[7]    Tutorial: group general mapping                                        │
│[8]    Tutorial: GroupHomomorphismByImages vs. GroupGeneralMappingByImages    │
│[9]    Tutorial: group general mapping single-valued                          │
│[10]   Tutorial: group general mapping total                                  │
│[11]   Reference: Groups                                                      │
│[12]   Reference: Group Elements                                              │
│[13]   Reference: Group Properties                                            │
│[14]   Reference: Group Homomorphisms                                         │
│[15]   Reference: GroupHomomorphismByFunction                                 │
│[16]   Reference: Group Automorphisms                                         │
│[17]   Reference: Groups of Automorphisms                                     │
│[18]   Reference: Group Actions                                               │
│[19]   Reference: Group Products                                              │
│[20]   Reference: Group Libraries                                             │
│ > > >                                                                        │
└─────────────── [ <Up>/<Down> select, <Return> show, q quit ] ────────────────┘
```

Ви можете використовувати клавіші зі стрілками для переміщення вгору та вниз по списку, а також відкривати сторінки довідки,
натискаючи клавішу Return. Для цієї вправи спочатку відкрийте `Tutorial: Groups and Homomorphisms`. Зверніть увагу на навігаційні інструкції внизу екрана. Перегляньте
перші дві сторінки, потім натисніть `q`, щоб повернутися до меню вибору. Далі перейдіть до елементу
`Reference: Groups` і відкрийте його. На перших двох сторінках ви знайдете
функцію `Group` та згадку `Order`.

Посібник GAP доступний у кількох форматах: текстовий зручний для перегляду в терміналі,
PDF зручний для друку, а HTML (особливо з підтримкою MathJax)
дуже ефективний для перегляду за допомогою браузера. Якщо ви використовуєте GAP на власному комп’ютері, ви можете встановити для перегляду довідки браузер за замовчуванням. If you are
running GAP on a remote machine, this (probably) will not work. (see
`?WriteGapIniFile` on how to make this setting permanent):

```gap
SetHelpViewer("browser");
```

Після цього викличте довідку знову та побачите різницю!

Давайте тепер скопіюємо наступні вхідні дані з першого прикладу глави довідкового посібника GAP
про групи. У ньому показано, як створювати перестановки та присвоювати значення
змінним. Це `Reference: Groups`. Ви можете вибрати його, ввівши `?11`, де
ви заміните `11` на те значення, яке з’явиться перед `Reference: Groups` на вашому комп'ютері.

Якщо ви переглядаєте документацію GAP у терміналі, вам може бути корисно
відкрити дві копії GAP, одну для читання документації та одну для написання коду!

Цей посібник показує, як перестановки в GAP записуються в циклічній нотації, а також
показує загальні функції, які використовуються з групами. Крім того, у деяких місцях використовуються дві крапки з комою
в кінці рядка. Це не дозволить GAP показувати результат обчислення.

```gap
a:=(1,2,3);;b:=(2,3,4);;
```

Далі, нехай `G` - група, утворена за допомогою `a` та `b`:

```gap
G:=Group(a,b);
```

```output
Group([ (1,2,3), (2,3,4) ])
```

Ми можемо дослідити деякі властивості групи `G` та її породжувачів:

```gap
Size(G); IsAbelian(G); StructureDescription(G); Order(a);
```

```output
12
false
"A4"
3
```

Нашим наступним завданням є з'ясувати, як отримати список елементів `G` та їх порядок.
Введіть `?elements` і перегляньте список тем довідки. Після перевірки запис у Tutorial не здається актуальним, але є запис у
довідковому посібнику (Reference). Тут також пояснюється різниця між використанням `AsSSortedList`
і `AsList`. Отже, це список елементів `G`:

```gap
AsList(G);
```

```output
[ (), (2,3,4), (2,4,3), (1,2)(3,4), (1,2,3), (1,2,4), (1,3,2), (1,3,4),
  (1,3)(2,4), (1,4,2), (1,4,3), (1,4)(2,3) ]
```

Повернений об’єкт є _списком_. Ми хотіли б призначити його змінній
для дослідження та повторного використання. Ми забули це зробити, коли обчислювали. Звичайно,
ми можемо використовувати історію командного рядка, щоб відновити останню команду, відредагувати
її та викликати знову. Але замість цього ми будемо використовувати `last`, яка є спеціальною змінною,
що містить останній результат, повернутий GAP:

```gap
elts:=last;
```

```output
[ (), (2,3,4), (2,4,3), (1,2)(3,4), (1,2,3), (1,2,4), (1,3,2), (1,3,4),
  (1,3)(2,4), (1,4,2), (1,4,3), (1,4)(2,3) ]
```

Це список. Списки в GAP індексуються від 1.
Наступні команди (сподіваємося!) не потребують пояснень:

```gap
gap> elts[1]; elts[3]; Length(elts);
```

```output
()
(2,4,3)
12
```

:::::::::::::::::::::::::::::::::::::::::  callout

## Lists are more than arrays

- May contain holes or be empty

- May dynamically change their length (with `Add`, `Append` or direct assigment)

- Not required to contain objects of the same type

- See more in [GAP Tutorial: Lists and Records](https://docs.gap-system.org/doc/tut/chap3.html)

::::::::::::::::::::::::::::::::::::::::::::::::::

Many functions in GAP refer to `Set`s. A set in GAP is just a list that happens to have
no repetitions, no holes, and elements in increasing order. Here are some examples:

```gap
gap> IsSet([1,3,5]); IsSet([1,5,3]); IsSet([1,3,3]);
```

```output
true
false
false
```

Now let us consider an interesting calculation -- the average order of elements
of `G`. There are many different ways to do this, we will consider a few of them
here.

A `for` loop in GAP allows you to do something for every member of a collection.
The general form of a `for` loop is:

```gap
for val in collection do
  <something with val>
od;
```

For example, to find the average order of our group `G` we can do.

```gap
s:=0;;
for g in elts do
  s := s + Order(g);
od;
s/Length(elts);
```

```output
31/12
```

Actually, we can just directly loop over the elements of `G` (in general GAP
will let you loop over most types of object). We have to switch to using `Size`
instead of `Length`, as groups don't have a length!

```gap
s:=0;;
for g in G do
  s := s + Order(g);
od;
s/Size(G);
```

```output
31/12
```

There are other ways of looping. For example, we can instead loop over a range of integers,
and accept `elts` like an array:

```gap
s:=0;;
for i in [ 1 .. Length(elts) ] do
  s := s + Order( elts[i] );
od;
s/Length(elts);
```

```output
31/12
```

However, often there are more compact ways of doing things. Here is a very
short way:

```gap
Sum( List( elts, Order ) ) / Length( elts );
```

```output
31/12
```

Let's break this last part down:

- `Order` finds the order of a single permutation.
- `List(L,F)` makes a new list where the function `F` is applied to each
  member of the list `L`.
- `Sum(L)` adds up the members of a list `L`.

:::::::::::::::::::::::::::::::::::::::::  callout

## Which approach is best?

Compare these approaches. Which one would you prefer to use?

::::::::::::::::::::::::::::::::::::::::::::::::::

GAP has very helpful list manipulation tools. We will now show a few more examples.

Sometimes, GAP does not have the exact function we want.
For example, `NrMovedPoints` gives the number of moved points of a permutation,
but what if we want to find all permutations which move `4` points? This is where
GAP's arrow notation comes in. `g -> e` makes a new function which takes one argument `g`,
and returns the value of the expression `e`. Here are some examples:

- finding all elements of `G` with no fixed points:

```gap
Filtered( elts, g -> NrMovedPoints(g) = 4 );
```

```output
[ (1,2)(3,4), (1,3)(2,4), (1,4)(2,3) ]
```

- finding a permutation in `G` that conjugates (1,2) to (2,3)

```gap
First( elts, g -> (1,2)^g = (2,3) );
```

```output
(1,2,3)
```

Let's check this (remember that in GAP permutations are multiplied from left to right!):

```gap
(1,2,3)^-1*(1,2)*(1,2,3)=(2,3);
```

```output
true
```

- checking whether all elements of `G` move the point 1 to 2:

```gap
ForAll( elts, g -> 1^g <> 2 );
```

```output
false
```

- checking whether there is an element in `G` which moves exactly two points:

```gap
ForAny( elts, g -> NrMovedPoints(g) = 2 );
```

```output
false
```

:::::::::::::::::::::::::::::::::::::::  challenge

## Use list operations to select from `elts` the stabiliser of the point 2 and the centraliser of the permutation (1,2)

- `Filtered( elts, g -> 2^g = 2 );`

- `Filtered( elts, g -> (1,2)^g = (1,2) );`

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- Remember that GAP is case-sensitive!
- Do not panic if you see `Error, Variable: 'FuncName' must have a value`.
- Care about names of variables and functions.
- Use command line editing.
- Use autocompletion instead of typing names of functions and variables in full.
- Use `?` and `??` to view help pages.
- Set the default help format to HTML using `SetHelpViewer`.
- Use the `LogTo` function to save all GAP input and output into a text file.
- If calculation takes too long, press <Control>\-C to interrupt it.
- Read 'A First Session with GAP' from the GAP Tutorial.

::::::::::::::::::::::::::::::::::::::::::::::::::


