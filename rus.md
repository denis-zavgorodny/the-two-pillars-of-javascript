# Два столпа JavaScript
## Часть 1: наследование через прототипы

Перед тем как мы начнем — позвольте представиться. Во время чтения вы, вероятно, будете задаваться вопросом «кто он такой и что о себе воображает».

Меня зовут **Эрик Элиот**, я автор книги **«Programming JavaScript Applications» (O’Reilly)**, ведущий *документального фильма* **«Programming Literacy»** и создатель серии платных онлайн-курсов **«Learn JavaScript with Eric Elliott»**.

Внес свой вклад в создание ПО для **Adobe Systems, Zumba Fitness, The Wall Street Journal, ESPN, BBC**, топ-артистов таких как **Usher, Frank Ocean, Metallica**, и многих других.

#### Однажды…
Я был заперт в темноте. Был слеп — топчась на месте, натыкаясь на предметы и ломая их, приводя в беспорядок все к чему прикасался.


В 90-х я программировал на C++, Delphi и Java, создавал 3D-плагины для ПО  которое позже стало называться Maya (используется большинством крупных киностудий для создания блокбастеров).


Тогда-то оно и случилось: *пришествие* Интернета. Все начали создавать веб-сайты, и, после практики создания и поддержки пары интернет-журналов, друг убедил меня что будущее Интернета будет за SaaS (тогда этого термина не было и в помине). В то время я об этом не догадывался, однако этот общий настрой стал потихоньку влиять на мое общее представление о программировании, потому что *если вы хотите делать хороший SaaS продукт — вам приходится использовать JavaScript*.

И как только я начал его использовать — я не смог с него спрыгнуть. Внезапно, все стало проще. ПО которое я создавал стало более гибким. Код стал жить дольше без рефакторинга. Вначале я думал о JavaScript как скриптовом языке для взаимодействия элементов страницы — однако, когда я узнал о cookies и AJAX, изначальное представление о нем изменилось.

JavaScript предлагает то чего не хватает в других языках:

> Свободу! (прим. пер.: а мне больше нравится определение «анархия» :)

JavaScript является одним из наиболее важных ЯП всех времен не просто из-за своей популярности, а потому что он популяризует две черезвычайно важные для развития всей науки программирования парадигмы:
* Наследование через прототипы (объекты не содержащие классов, делегирование прототипов более известное как OLOO — Objects Linking to Other Objects)
* Функциональное программирование (с помощью лямбда-выражений и замыканий)

Вместе я называю эти парадигмы *двумя столпами JavaScript* и, к моему стыду, они меня полностью совратили. Теперь мне не хочется программировать на языках в которых нет их реализации.

*JavaScript навсегда запомнят как один из самых значимых языков когда-либо созданых.* Множество других языков уже скопировали один, либо другой, либо оба этих столпа — и эти столпы уже изменили способы написания приложений *в других языках*.

Брендан Айк (создатель JavaScript) специально не задумывал ни один из этих столпов, но JavaScript всей своей сущностью полагается на их использование. Оба столпа одинаково важны, но я обеспокоен тем, что большинство JavaScript-программистов полностью игнорируют одну или обе эти инновации, поскольку JavaScript черезвычайно хорош в создании плохого кода если вы не затрудняете себя изучением хороших практик.

Низкий порог вхождения — это важная фича языка, позволяет быстро начать делать нужные вещи… но, строя карьеру разработчика, имейте ввиду что фаза игнорирования лучших практик языка *не должна длиться больше года*.

И если вы все еще находитесь в ней — **пришло время перейти на новый уровень**.

Если вы создаете конструкторы и наследуете их — вы так и не изучили JavaScript. И не важно, что вы занимаетесь этим с 1995 года. Вы не в состоянии воспользоваться преимуществами самых крутых возможностей языка.

**Вы работаете с липовым JavaScript'ом, который существует только как надстройка над Java.**

Вы кодите на этом удивительном, меняющем правила игры, плодотворном языке и **полностью упускаете то что делает процесс таким клевым.**

### Мы строим бардак
> «Не понимающие что идут в темноте никогда не выйдут на свет.» ~ Брюс Ли

**Конструкторы нарушают [принцип открытости/закрытости][opencloseprinciple]**. Создаете игру на HTML5? Хотите изменить стандартное поведение и использовать [пул объектов][objectpool] вместо инстанцирования новых копий, чтобы *сборщик мусора не влиял на FPS*? Вы либо поломаете логику приложения, либо запутаетесь с костылями при создании фабрики методов.

Если вы вернете произвольный объект из конструктора — это сломает ссылки на прототип, и ключевое слово `this` в конструкторе больше не будет ссылаться на только что созданный экземпляр объекта. Особенно это будет заметно при использовании шаблона [«фабрика»][fabricmethod], потому что в этом случае мы просто не сможем использовать `this`: придется его игнорировать.

Конструкторы без [строгого режима][usestrict] могут быть также опасны. Если вызывающий забывает использовать `new`, и ваш код *не использует строгий режим* или *не является ES6-классом* [вздох], все что вы присваиваете через `this` будет загрязнять глобальное пространство имен. Не очень красиво, я считаю.

До появления строгого режима эта особенность языка вызывала труднонаходимые неявные ошибки (так было в паре стартапов где я работал, в критические периоды роста, когда у нас было не так много времени чтобы гоняться за подобными ошибками).

В JavaScript **фабричные методы**  — всего лишь конструктор функции **минус** обязательный `new`, опасность загрязнения глобальной области видимости и неудобные ограниченя (имею ввиду раздражающую большую букву в начале названия класса).

**На самом деле, JavaScript вообще не нуждается в конструкторах**, поскольку *любая функция может вернуть новый объект*. С помощью динамического расширения объектов, синтаксиса объектов и `Object.create()` у нас есть все что нужно. И наконец-то `this` ведет себя везде одинаково. Ура-ура!

### Добро пожаловать в седьмой круг ада
> “Чаще всего я не настолько печален насколько должен.” ~ Т. Х. Уайт

Каждый слышал аналогию про вареную лягушку: если вы поместите лягушку в кипящую воду — она выскочит. Если вы поместите лягушку в холодную воду и постепенно начнете подогревать  — лягушка сварится, потому что не почувствует опасности. В нашей истории лягушки это мы.

Если поведение конструкторов это кастрюля с водой, то **классическое наследование** это не огонь, а **пламя из седьмого круга ада Данте**.

#### Проблема гориллы и банана
> «Проблема объектно-ориентированных языков в том что они тащут за собой всю неявную среду. Вы хотите получить банан, но кроме банана в нагрузку получаете гориллу, и все чертовы джунгли» ~ Джо Армстронг

Классическое наследование, как правило, позволяет наследовать только от одного предка, и это очень неудобный паттерн. Неудобный, потому что *любой объектно-ориентированый паттерн наследования, который я видел в приложениях, в итоге был неправильным*.

Допустим, вы начинаете с двумя классами: *Инструменты* и *Оружие*. Вы уже облажались — у вас не получится создать игру [«Cluedo»][cluedo] *(прим. пер.: если имеется ввиду что от класса Оружие без гемороев не получить инстансы Нож и Револьвер, необходимые для игры — то так себе метафора :)*.

#### Проблема [сильного зацепления][coupling]
*Связь между классом и его родителем является самой крепкой формой зависимости в объектно-ориентированном дизайне (ООД)*. Переиспользуемый модульный код, наоборот, имеет слабые связи.

Внесение даже небольших изменений в класс создает *наследуемые побочные эффекты*, ломающие, казалось бы, совсем не связанные вещи.

#### Проблема необходимого дублирования
Очевидное решение вышеописаных проблем — вернуться назад, создать новые классы с нужными изменениями и правильным наследованием… но, это связано с опасностью глобального рефакторинга. Обычно все заканчивается дублированием кода вместо переиспользования. Нарушается основополагающий принцип [**DRY**][DRY]

Как следствие, ваши сложно различимые джунгли классов продолжают расти и, по мере добавления уровней наследования, становятся все более запутанными. Когда вы находите баг — вы не правите его в одном месте. *Вы правите его везде.*

> «Ой. Еще один.» ~ Каждый классический ООП-программист рано или поздно

Это известная в ООП-кругах [**проблема необходимого дублирования.**][code-duplicity]


**Классы ES6 не исправляют ни одну из вышеописанных проблем.** ES6 все усугубляет, потому что **плохие идеи** *попадают в спецификации*, и тиражируются в тысячах книг, статей в блогах.

В этом плане ключевое слово `class`, вероятно, самое вредное нововведение в JavaScript. Я безмерно уважаю блестящих и трудолюбивых людей, которые прилагали свои усилиях по стандартизации, но даже блестящие люди иногда делают неправильные вещи. К примеру, попробуйте в консоли браузера выполнить `.1 + .2`. *(прим. пер.: тут автор явно [троллит][floating-math] — вряд ли это архитектурная ошибка языка)*. Однако, это не мешает мне считать, что Брендан Айк внес большой вклад в веб, языки программирования и развитие ИТ в целом.

P.S.: Не используете `super` если не получаете удовольствие от пошаговой отладки каждого слоя из множества абстракций наследований.

#### Падение
Эти проблемы умножаются по мере роста вашего приложения и, в конце концов, остается только переписать все с нуля или полностью доломать — иногда это единственный способ сократить финансовые потери.

Я натыкался на это **снова** и **снова**, **работа** за **работой**, **проект** за **проектом**. *Научимся ли мы когда-нибудь на своих ошибках?*

В одной компании, где я работал, *из-за подобных проблем пришлось даже перенести дату релиза на целый год*. Я верю в **доработки вместо переделок**. Другая компания, которую я консультировал, *чуть не обанкротилась* по тем же самым причинам.

> Эти проблемы — не вопрос вкуса или стиля. В зависимости от выбора, это может как помочь, так и убить ваш продукт.

Крупные компании, как правило, обычно делают вид что все под контролем. Но стартапы не могут позволить себе тормоза из-за подобных проблем во время попыток найти свой продукт/рынок, потому что время всегда ограничено.

***В современном коде я не видел ни одного успешного решения, которое помогало бы избежать всех вместе взятых проблем.***

### Выход в свет
> «Совершенство достигается не тогда, когда нечего больше добавить, а тогда когда больше нечего убавить». ~ Антуан де Сент-Экзюпери

Недавно, работая над библиотекой для демонстрации прототипного наследование для своей книги «Programming JavaScript Applications», я набрел на интересную идею: функция-фабрика, которая помогает создавать функции-фабрики, которые, в свою очередь, успешно наследуются и объединяются. Я назвал подобные фабрики «штампами», и библиотеку, соответвенно, [«Stampit»][stampit]. Она простая и очень маленькая. Я рассказывал о библиотеке на конференции O'Reilly Fluent конференции в 2013 году, и написал о ней в блоге.

Существует небольшое но постепенно растущее сообщество разработчиков, на чей стиль разработки повлиял Stampit. На текущий момент библиотека используется в продакшне множеством приложений миллионами пользователей.

Конечно, Stampit не является единственной альтернативой. Например, Дуглас Крокфорд совсем не использует `new` или `this`, предлагая вместо этого полностью функциональный подход к повторному использованию кода.

Все его объекты являются набором функций, не зависимых от глобальных переменных, или содержат только данные (например, ассоциативные массивы). Это достаточно хорошо работает до тех пор пока вы не создадите сотни тысяч объектов и, вдруг, ваше приложение должно будет работать с минимальной задержкой (я говорю о игровых движках, обработчиках событий реального времени и т.п.). В этом случае передача управления вызывающим методам поможет оптимизировать управление ресурсами. *(прим. пер.: есть подозрение что автор не до конца понял фишку ФП)*

Еще одна хорошая альтернатива наследованию — использование модулей (я рекомендую npm + ES6 модули через Browserify или WebPack), или просто клонирование объектов через копирование их свойств (`Object.assign()`, `lodash.extend()` и т.п.).

Механизм копирования это еще одна форма прототипного наследования под названием «наследование через объединение».

Даже если вы последуете совету Дугласа Крокфорда и перестанете использовать `this`, вы все еще можете делать вещи с использованием прототипов. Наследование через объединение возможно благодаря такому свойству JavaScript как динамическое расширение объекта: способности модифицировать экземпляр объекта после его создания.

**Вам никогда не потребуются классы в JavaScript**, и я ни разу не встречал ситуации в которой класс был бы лучше чем вышеописанные альтернативы. Если вы сможете придумать такую ситуацию — опишите её в комментариях (я предлагаю эту задачу на протяжении нескольких лет, и никто пока не придумал хороший пример использования кроме надуманных аргументов о микро-оптимизациях или стилевых предпочтениях).

Когда я говорю людям, что конструкторы и классическое наследование это зло — они начинают защищаться. *Я не нападаю на вас. Я пытаюсь вам помочь.*

Люди привязываются к одному стилю программирования, как будто способ разработки является частью их личности. *Полная чушь.*

Не имеет значения как это сделано, если это сделано плохо.

> Единственная важная вещь в разработке ПО — это то что пользователи любят ваше ПО.

Многие люди не прислушиваются к советам, и предпочитают наступать на грабли самостоятельно. Не делайте эту ошибку, *её цена может быть непомерной*. У вас есть возможность учиться не наступать на грабли, на которые наступали многие снова, и снова в течении десятилетий. Целые книги посвящены этим граблям.

Одна из таких книг (вероятно, самая известная) [«Паттерны проектирования»][design-patterns] от GoF строится вокруг двух основополагающих принципов:

*«Создавайте интерфейсы вместо реализации»* (фокусируйтесь на том **что** делает ваш код вместо _как он это делает_) и *«композиция предпочтительнее наследования»*.


Поскольку дочерние классы реализуют интерфейсы родительского — второй принцип вытекает из первого, но будет полезно обсудить его подробно.

Книга содержит целый раздел паттернов по созданию объектов, которые решают единственную цель — обойти ограничения конструкторов и наследования через классы.

Погуглите *«new considered harmful»*, *«inheritance considered harmful»* и *«super is a code smell»*. Вы найдете дюжину статей в авторитетных блогах наподобии Dr. Dobb's Journal, написаных еще до изобретения JavaScript. Все они которых говорят о тех же самых вещах: `new`, сломанное классическое наследование и связывание потомок-родитель (в нашем случае через `super`) это дорога в один конец.

*Даже Джеймс Гослинг, создатель Java, признает что Java неправильно реализует объекты.*

Хотите ближе к JavaScript? Дуглас Крокфорд [сообщил][douglas-speech] что `Object.create()` был добавлен в синтаксис для того чтобы ему не пришлось использовать `new`.

Кайл Симпсон (автор, [«You don't know JS»][dont-know-js]) написал захватывающую серию из трех постов под названием [«JS Objects: Inherited a Mess.»][js-inherited-mess]


Кайл противопоставляет прототипное наследование классическому через классы, утверждая что первое проще и удобнее. Он даже ввел термин **OLOO** (Objects Linked to Other Objects), чтобы прояснить различия между делегированием прототипа и наследованием через класс.

### Хороший код — простой код.
> «Упрощение это удаление очевидного и добавление смысла» ~ Джон Маэда

#### Итак, если вы выбросите конструкторы с классическим наследовании из своего JavaScript кода, все станет:
* **Проще** (легче читать и писать, исчезнут неправильные паттерны проектирования)
* **Гибче** (переключиться с создания новых экземпляров на легко утилизируемые пулы объектов? Без проблем)
* **Мощнее и выразительнее** (наследовать от нескольких предков? наследовать приватные методы? Как нефиг делать)  

#### Вариант получше
> «Если вещь теоретически может быть опасной, и есть вариант получше — всегда используйте этот вариант» ~ Дуглас Крокфорд *(прим. пер.: или я не так понял смысл фразы, или это Капитан Очевидность)*

Я не стараюсь отобрать у вас полезный инструмент. Моя задача — предупредить вас о правильном использовании нужных инструментов и при этом не выстрелить себе в ногу. В случае конструкторов и классов, есть лучшие варианты.

Другой распространенный аргумент, который программисты часто используют, звучит так: «код это способ самовыражения, а стиль программирования это искусство». Я считаю данный аргумент слишком эмоциональным и в целом нерациональным:

*Ваш код не является продуктом вашего самовыражения, как и кисти художника не являются для него результатом работы.* **Код это инструмент. А программа это продукт.**

Да, *некоторые исходные коды сами по себе настолько красивы что кажутся искусством*, однако если они не распечатан на бумаге и не выставлены в картинной галлерее — **вряд ли их оценят искуствоведы**. В большинстве же случаев, *пользователи наслаждаются не вашим кодом, а результатом его работы — программой.*

Хороший стиль программирования требует, чтобы, когда вы предстанете перед выбором: элегантный, простой гибкий код, или сложный, неудобный, с ограничениями — вы выберете первый вариант. Сейчас модно быть открытым новым особенностям языка, но есть два пути: *правильный и не правильный*.

**Выберите правильный путь**

~ Эрик Эллиот


[opencloseprinciple]: https://ru.wikipedia.org/wiki/Принцип_открытости/закрытости
[objectpool]: https://ru.wikipedia.org/wiki/Объектный_пул
[fabricmethod]: https://ru.wikipedia.org/wiki/Фабричный_метод_(шаблон_проектирования)
[usestrict]: http://ru.stackoverflow.com/questions/435546/%D0%A7%D1%82%D0%BE-%D0%B7%D0%BD%D0%B0%D1%87%D0%B8%D1%82-use-strict
[cluedo]: https://ru.wikipedia.org/wiki/Cluedo
[coupling]: https://ru.wikipedia.org/wiki/Зацепление_(программирование)
[DRY]: https://ru.wikipedia.org/wiki/Don%E2%80%99t_repeat_yourself
[code-duplicity]: https://ru.wikipedia.org/wiki/Дублирование_кода
[floating-math]: http://0.30000000000000004.com
[stampit]: https://www.npmjs.com/package/stampit
[design-patterns]: https://ru.wikipedia.org/wiki/Design_Patterns
[douglas-speech]: https://www.youtube.com/watch?v=bo36MrBfTk4
[dont-know-js]: https://github.com/getify/You-Dont-Know-JS
[js-inherited-mess]: https://davidwalsh.name/javascript-objects
[pillars-source]: https://medium.com/javascript-scene/the-two-pillars-of-javascript-ee6f3281e7f3
[comment-about-inheritance]: https://medium.com/@_ericelliott/what-you-were-taught-was-not-prototypal-inheritance-f853ce3db00e