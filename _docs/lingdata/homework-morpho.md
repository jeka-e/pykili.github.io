## Домашнее задание "Разработка фрагмента учебного корпуса", часть 5

### Работа с грамматической разметкой 

Для работы вы получите текстовый файл ваших расшифровок с лексико-грамматической разметкой (мы используем UDpipe, модель Russian-SynTagRus-2.3, с некоторыми модификациями). Оставьте в файле только те реплики, которые были вами в ELAN. 
У каждого слова указан ID, пожалуйста, никак не модифицируйте его, чтобы можно было конвертировать исправления обратно в корпус. То же касается табуляций между полями, они важны для последующей автоматической обработки. 
[Папка](https://yadi.sk/d/9rw_m86315Xfrg) с файлами.

```
1   Значит   значить   VERB   Aspect=Imp|Mood=Ind|Number=Sing|Person=3|Tense=Pres|VerbForm=Fin|Voice=Act
2   кошку    кошка     NOUN   Animacy=Anim|Case=Acc|Gender=Fem|Number=Sing
3   кото...  который   PRON   _
4   /        /         PUNCT  _ 
5   ээ       ээ        INTJ   _
6   /        /         PUNCT  _
7   кошку   кошка      NOUN   Animacy=Anim|Case=Acc|Gender=Fem|Number=Sing
8   зовут   звать      VERB   Aspect=Imp|Mood=Ind|Number=Plur|Person=3|Tense=Pres|VerbForm=Fin|Voice=Act
9   Машка   машка      PROPN  Animacy=Anim|Case=Gen|Gender=Masc|Number=Sing
10  //      //         PUNCT  _
```

Мы советуем работать с каждым слоем отдельно, чтобы снизить количество ошибок, пропущенных по невнимательности. [Список помет](https://github.com/olesar/ruUD/blob/master/conversion/RNCtoUD.md) UDpip - c их соответствиями в НКРЯ. В случае сомнений, лучше обращаться к НКРЯ, поиск в корпусе со снятой лексико-грамматической омонимией. 

#### Леммы

Проверьте правильность лемм. Леммы глаголов (включая причастия и деепричастия) -- инфинитивы, для каждого вида (СВ
-НСВ) свой. Для оборванных слов нужно восстановить лемму полного слова.  
 
#### Части речи

Обратите внимание, что имена собственные имеют отдельный тег PROPN. Маркеры хезитации _а-а_, _ээ_ помечаются как междометия (INTJ). Добавьте теги для предикатива (PRAEDIC) и порядковых числительных (ANUM) там, где нужно. 
Субстантивированные, адъективированные и т.п. употребления лучше размечать по исходной части речи, ср. _Я вспомнил страшное_ (лемма страшный, ADJ). 

Часто смешиваемые случаи: 
* `PRON` vs `DET` (_все (знают)_, лемма все, PRON  и _все (дети)_, лемма весь, DET, _(увидел) его_, лемма он, PRON, и _его портрет_, лемма его, DET)  
* `PRON` vs `CONJ` (_что_, _то_)   
* `ADP` vs `ADV` (_(шла) мимо_ и _(шла) мимо (забора_)  
* `VERB` vs `NOUN` (_деть_ и _дело_, ср. форму _дела_)    

#### Морфологические категории

Проверять разметку грамматических категорий особенно трудно, поэтому мы советуем просматривать отдельно ключевые категории: 

* падеж для существительных, местоимений, прилагательных, числительных и причастий, прежде всего, Nom vs Acc. Восстановите "вторые" падежи, если нужно: Gen2, Loc2, Acc2. 
* вид для глагола
* одушевленность для существительных
* род для существительных, прилагательных и местоимений  
* сравнительные степени типа _поскорей_ должны получить помету Degree=Cmp2 (ср. _скорей Degree=Cmp).


Если какой-то тег вызывает у вас вопросы и вы сомневаетесь в разметке, поставьте знак `!` непосредственно перед ним, например, `!Case=Gen`.


#### Неоднословные союзы, предлоги, наречия и т. п. 

Они размечаются пословно, и их проверять не надо. Разметка таких единиц будет проверена автоматически по словарю.


#### Ошибки в токенизации

Если словоформу типа _маш-ка_ программа ошибочно разбила на три токена, поставьте ! в начале строки, перед ID. Аналогично, если несколько токенов размечены как один.  


#### Итог

Исправленный файл (с тем же именем, что и выданный вам исходный) положите в ваш репозиторий LiveCorpus. 


#### Полезное

[Инструкция по разметке НКРЯ](http://ruscorpora.ru/sbornik2005/09sitch.pdf). В ней указана нотация НКРЯ, но принципы разметки те же. 

Предикатив (категория состояния) -- это часть речи, для которой характерны следующие контексты:  
* дативный (_Мне холодно_, _Вам не пора?_)   
* инфинитивный (_Холодно стоять-то_, _Пора уходить_)  
* локативный (_В комнате холодно_)  
* позиция предиката предложения (клаузы)   
У предикативов бывает сравнительная степень и прошедшее и будущее время (ср. _было холодно_, _пора будет уходить_). Чаще всего предикативами бывают формы на -о/-е, а также _пора_, _жаль_, _охота_, _лень_ и др. 

Основные различия в категоризации части речи между UDpipe и НКРЯ  
* _который_ PRON vs APRO
* _один_ NUM vs ANUM
* _бы_, _б_ AUX vs PART  
* _быть_ (в функции грамматического показателя прошедшего и будущего времени) AUX vs V
