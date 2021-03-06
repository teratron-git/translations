# React Native в Airbnb: Технология
### Технические детали.

*Перевод статьи [Gabriel Peal](https://medium.com/@gpeal): [React Native at Airbnb: The Technology](https://medium.com/airbnb-engineering/react-native-at-airbnb-the-technology-dafd0b43838).*

![](https://cdn-images-1.medium.com/max/2000/1*iaYan0f1NeQlzGnwzjXEvg.jpeg)

*Это вторая статья [в серии](../gabriel-peal-react-native-at-airbnb), в которой мы поделимся нашим опытом с React Native и расскажем, что ждёт в дальнейшем мобильную разработку в Airbnb.*

React Native является относительно новой и быстроразвивающейся платформой в среде Android, iOS, веб и кроссплатформенных фреймворков. Спустя два года можно смело сказать, что платформа React Native во многом революционна. Это изменение парадигмы для мобильных устройств, и мы смогли воспользоваться многими её преимуществами. Однако, эти преимущества нельзя упоминать в отрыве от весьма болезненных аспектов технологии.

## Что работает хорошо

### Кроссплатформа.
Основное преимущество React Native заключается в том, что код, который вы пишете, работает на Android и iOS. Новая функциональность нашего приложения, созданная с использованием React Native, смогла достичь 95-100% переиспользования кода, и только 0,2% файлов были специфичными для платформы (\*.android.js/\*.ios.js).

### Система единого дизайн-языка (Unified Design Language System или DLS)
[Мы разработали язык кроссплатформенного дизайна под названием DLS](http://sketchapp.me/vizualnyj-yazyk-ot-kompanii-airbnb/). У нас есть Android, iOS, React Native и веб-версии каждого компонента. Наличие унифицированного языка проектирования упростило написание кроссплатформенных функций, поскольку это означало, что проекты, имена компонентов и экраны будут согласованы между платформами. Тем не менее, мы все еще могли принимать решения, соответствующие платформе, там где это применимо. Например, мы используем родной [Toolbar](https://developer.android.com/reference/android/support/v7/widget/Toolbar) на Android и [UINavigationBar](https://developer.apple.com/documentation/uikit/uinavigationbar) на iOS, и мы решили скрыть [индикаторы раскрытия информации](https://developer.apple.com/ios/human-interface-guidelines/views/tables/) на Android, потому что они не соответствуют гайдлайнам платформы Android.

Мы решили переписать компоненты вместо того, чтобы оборачивать уже имеющиеся у нас нативные, потому что было более надежно сделать подходящие для платформы API индивидуально для каждой платформы и уменьшить затраты на обслуживание для инженеров Android и iOS, которые могут не знать, как правильно тестировать изменения в React Native. Тем не менее, это вызвало фрагментацию между платформами, в которых нативные и React Native версии одного и того же компонента могли разойтись в реализации.

### React
Есть причины, по которым React считается [наиболее любимым разработчиками](https://insights.stackoverflow.com/survey/2018/#technology-most-loved-dreaded-and-wanted-frameworks-libraries-and-tools) веб-фреймворком. Он простой, но мощный и хорошо масштабируется до больших кодовых баз. Кое-что из того, что мы особо любим:

* **Компоненты**: Компоненты React принуждают к разделению ответственности. Эта идея приносит главный вклад в масштабируемость React;
* **Упрощенные жизненные циклы**: Жизненные циклы компонент в Android и, в несколько меньшей степени, в iOS, как известно, [сложны](https://i.stack.imgur.com/fRxIQ.png). Функционально-реактивные компоненты React фундаментально решают эту проблему и делают обучение React Native значительно проще, чем обучение Android или iOS;
* **Декларативность**: декларативный характер React помог сохранить наш пользовательский интерфейс в синхронизации с лежащим в основе состоянием.

### Скорость итераций
При разработке на React Native мы смогли надежно использовать горячую перезагрузку (hot reloading), для того чтобы видеть наши изменения на Android и iOS уже через секунду или две. Несмотря на то, что производительность сборки является главным приоритетом для наших собственных приложений, она никогда не приближалась к скорости итерации, которую мы достигли с React Native. В лучшем случае время компиляции нативного кода у нас составляет 15 секунд, но может достигать 20 минут для полных сборок.

### Инвестиции в инфраструктуру
Мы плотно интегрировали React Native приложение с нативной инфраструктурой. Все основные элементы, такие как: сеть, i18n, эксперименты, непрерывные переходы между элементами (shared element transitions), информация об устройстве, информация об учетной записи и многие другие, были обёрнуты в единый API React Native . Эти мосты были одной из наиболее сложных частей разработки, потому что мы хотели обернуть существующие API Android и iOS во что-то, что было бы последовательным и каноническим для React. Поскольку сохранение этих мостов актуальными при быстрых итерациях и, одновременно разработка новой инфраструктуры — это постоянная игра в догонялки, инвестиции в инфраструктурную команду помогли несколько упростить разработку.

Без этих крупных инвестиции в инфраструктуру, использование React Native привело бы к плохому опыту разработчиков и пользователей. В результате мы не считаем, что React Native можно просто прикрепить к существующему приложению без значительных и непрерывных инвестиций.

### Производительность
Одной из самых больших ожидаемых проблем React Native была его производительность. Однако на практике это редко было настоящей проблемой. Большинство наших React Native  экранов ведут себя так же отзывчиво, как и родные. Производительность часто рассматривается в одном измерении. Мы часто видели, что мобильные инженеры смотрят на JS и думают «медленнее, чем Java». Однако изъятие бизнес-логики и [раскладки](https://github.com/facebook/yoga) из основного потока во многих случаях повышает производительность отрисовки.

Когда мы видели проблемы с производительностью, они обычно были вызваны чрезмерной визуализацией и были смягчены эффективным использованием [shouldComponentUpdate](https://reactjs.org/docs/react-component.html#shouldcomponentupdate), [removeClippedSubviews](https://facebook.github.io/react-native/docs/view.html#removeclippedsubviews) и лучшим использованием Redux.

Однако проблемы со временем инициализации и первого рендеринга (описанные ниже) привели к плохой работе React Native со стартовыми экранами и [deep linking](https://habr.com/company/redmadrobot/blog/267587/), и увеличили время TTI при навигации между экранами. Кроме того, экраны, которые теряли кадры, было трудно отлаживать, потому что между React Native компонентами и нативными view стоит Yoga.

### Redux
Для управления состоянием мы использовали [Redux](https://redux.js.org/). Мы нашли его эффективным и защищающим UI от возможности когда-либо выйти из синхронизации с состоянием. Так же с помощью него возможен легкий обмен данными между экранами. Тем не менее, Redux славится количеством кода, который необходимо написать для достижения результата, и имеет относительно сложную кривую обучения. Мы предоставили генераторы для некоторых общих шаблонов, но это все еще была одна из самых сложных частей и источник путаницы при работе с React Native. Стоит отметить, что эти проблемы не являются спецификой React Native.

### Опора на нативное
Поскольку все в React Native может быть соединено с помощью нативного кода, мы в конечном итоге смогли реализовать множество вещей, в возможности реализации которых изначально не были уверены, такие как:

1. _Shared element transitions_: мы написали компонент _<Shared Element>_, который поддерживается нативным кодом общих элементов на Android и iOS. Это даже работает между нативными экранами и React Native.

1. _Lottie_: мы смогли заставить Lottie работать в React Native, обернув существующие библиотеки на Android и iOS.

1. _Нативный сетевой стек_: React Native использует существующий нативный сетевой стек и кэш на обеих платформах.

1. _Другая инфраструктура_: так же, как и с сетевой подсистемой, мы обернули и остальные части нашей существующей нативной инфраструктуры, такие как i18n, эксперименты и так далее, так что они работали бесшовно в React Native.

### Статический анализ
У нас есть [сильная история использования eslint](https://github.com/airbnb/javascript) в веб-разработке. Тем не менее, мы были первой платформой в Airbnb, попробовавшей [Prettier](https://github.com/prettier/prettier). Мы обнаружили, что он эффективен для уменьшения траты времени на пустяки и отвлечение внимания от основной проблемы во время кодревью. Prettier в настоящее время активно исследуется нашей командой веб-инфраструктуры.

Мы также использовали аналитику для измерения времени рендеринга и производительности, чтобы выяснить, какие экраны были главным приоритетом для изучения проблем производительности.

Поскольку React Native был меньше и новее, чем наша веб-инфраструктура, он оказался хорошим полигоном для испытания новых идей. Многие из инструментов и идей, которые мы создали для React Native, сейчас внедряются в веб-разработку.

### Анимации
Благодаря библиотеке React Native [Animated](https://facebook.github.io/react-native/docs/animated.html) мы смогли достичь анимации без лагов и даже анимаций, управляемых взаимодействием, таких как прокрутка с параллакс-эффектом.

### JS/React Open Source
Поскольку React Native действительно работает с React и JavaScript, мы смогли использовать чрезвычайно широкий спектр JavaScript-проектов, таких как redux, reselect, jest и так далее.

### Flexbox
React Native обрабатывает раскладку с помощью [Yoga](https://github.com/facebook/yoga), кросс-платформенной библиотеки, написанной на C, которая производит вычисления раскладки через API [flexbox](https://www.w3schools.com/css/css3_flexbox.asp). На раннем этапе нас поразили ограничения Yoga, такие как отсутствие пропорций, но они были добавлены в последующих обновлениях. Плюс, веселые уроки, такие как [flexbox froggy](https://flexboxfroggy.com/) сделали адаптацию более приятной.

### Сотрудничество с веб-разработкой
На позднем этапе исследования React Native мы начали разрабатывать сразу для браузера, iOS и Android. Учитывая, что браузеный JavaScript также использует Redux, мы нашли большие участки кода, которые могут быть использованы совместно веб и нативными платформами без каких-либо изменений.

## Что работает не очень хорошо

### Незрелость React Native
React Native менее зрелый, чем Android или iOS. Он молодой, очень амбициозный, и очень быстро развивается. Хотя он хорошо работает в большинстве ситуаций, есть случаи, когда его незрелость оказывается узким местом и приводит к тому, что что-то тривиальное для нативной разработки становится очень сложным для реализации в React Native. К сожалению, эти случаи трудно предсказать и решение проблемы может занять от нескольких часов до нескольких дней.

### Поддержка форка React Native
Из-за незрелости React Native были времена, когда нам нужно было исправлять исходники React Native. В дополнение к контрибьютингу в основную ветку, мы должны были [поддерживать собственный форк](https://github.com/airbnb/react-native/commits/0.46-canary), в которой мы могли бы быстро объединить изменения и начать их использовать. За два года нам пришлось добавить около 50 коммитов поверх React Native. Это делает процесс апгрейда React Native крайне болезненным.

### JavaScript и инструменты разработчика
JavaScript является нетипизированным языком. Отсутствие типобезопасности было трудно масштабировать и это стало источником споров с командой инженеров мобильной разработки, использующих типизированные языки, которые в противном случае могли быть заинтересованы в изучении React Native. Мы исследовали внедрение [Flow](https://flow.org/), но загадочные сообщения об ошибках привели к разочарованию разработчика. Мы также изучили [TypeScript](http://www.typescriptlang.org/), но интеграция его в нашу существующую инфраструктуру, такую как [babel](https://babeljs.io/) и [metro bundler](https://github.com/facebook/metro), оказалась проблематичной. Однако мы продолжаем активно исследовать TypeScript в веб-разработке.

### Рефакторинг
Побочный эффект того, что в JavaScript отсутствует статическая типизация заключается в том, что рефакторинг был чрезвычайно сложным и подверженным ошибкам. Переименование пропсов, особенно пропсов с общим именем, таким как `onClick` или `props`, которые проходят через несколько компонентов, было кошмаром для рефакторинга. Что еще хуже, изменённый в процессе рефакторинга код ломался во время исполнения, а не во время компиляции, и было трудно добавить правильный статический анализ для избежания таких случаев.

### Неконсистентность JavaScriptCore
Один тонкий и сложный аспект React Native связан с тем, что он выполняется в [среде JavaScriptCore](https://facebook.github.io/react-native/docs/javascript-environment.html). Следующие последствия этого факта, с которыми мы столкнулись:
* iOS поставляется с собственным [JavaScriptCore из коробки](https://developer.apple.com/documentation/javascriptcore). Это означало, что iOS была в основном последовательной и не проблематичной для нас.
* Android не имеет собственного JavaScriptCore так что React Native вынужден поставлять его в составе бандла. Тем не менее, тот JavaScriptCore, что вы получаете по умолчанию, является [очень устаревшим](https://github.com/facebook/react-native/issues/10245). В результате, мы должны были пойти собственным путём, чтобы поставлять приложение с [новым JavaScriptCore](https://github.com/react-community/jsc-android-buildscripts).
* При отладке React Native подключается к экземпляру Chrome Developer Tools. Это здорово, потому что это мощный отладчик. Однако, когда отладчик подключен, весь JavaScript исполняется в движке V8, который входит в состав Chrome. Это нормально в 99,9% случаев, но иногда может преподносить сюрпризы. Например, у нас был случай, когда `toLocaleString` работал на iOS, а на Android работал только во время отладки. Оказывается, что Android JSC [не включает его](https://github.com/facebook/react-native/issues/15717), и молча падет, но если вы находитесь в режиме отладки, в этом случае React Native использует V8, в котором всё хорошо. Незнание технических деталей, таких как эта, может привести к дням болезненной отладки для инженеров.

### React Native и Open Source
Изучение платформы является сложным и трудоемким. Большинство людей знают только одну или две платформы. Разработка React Native библиотек, в которых есть взаимодействие с нативными частями, такими как карты, видео и так далее, требует равного знания всех трех платформ. Мы обнаружили, что большинство проектов на React Native с открытым исходным кодом были написаны людьми, которые имели опыт работы только с одним или двумя платформами. Это привело к несоответствиям или неожиданным ошибкам на Android или iOS.

В Android многие React Native библиотеки также требуют использования относительного пути к node_modules вместо публикации артефактов maven, что не соответствует ожиданиям коммьюнити.

### Параллельная работа над инфраструктурой и задачами.
Мы накопили много лет разработки нативной инфраструктуры на Android и iOS. Однако в React Native мы начали с чистого листа и должны были написать или создать мосты со всей существующей инфраструктуры. Это означало, что были времена, когда инженеру по продукту требовалась некоторая функциональность, которая еще не существовала. В этот момент он либо должен был работать с платформой, с которой не был знаком, и вне рамок своего проекта, чтобы создать необходимую инфраструктуру, либо работа была заблокирована ожиданием создания необходимой инфраструктуры со стороны других команд.

### Крэш-мониторинг
Мы используем [Bugsnag](https://www.bugsnag.com/) для отчетов о сбоях на Android и iOS. Как мы выяснили, Bugsnag на React Native менее надёжен и требует больше работы, чем на других наших платформах. Поскольку React Native относительно новый и редкий в отрасли, нам пришлось самим создать значительное количество инфраструктуры, такой как загрузка карт кода (source maps), и работать вместе с Bugsnag, чтобы иметь возможность делать такие вещи, как, например, фильтрацию только тех ошибок, которые произошли в React Native.

Из-за количества пользовательской инфраструктуры вокруг React Native у нас иногда возникали серьезные проблемы, в которых не создавался отчёты об ошибке или карты кода не были должным образом загружены.

Наконец, исследование сбоев в React Native может быть очень сложным, если проблема охватывает React Native и нативный код, так как трассировки стека (stack traces) не переходят между React Native и нативной частью.

### Мост с нативной частью

React Native имеет [API Brige](https://facebook.github.io/react-native/docs/communication-ios.html) для связи между нативной частью и React Native. Хотя это работает, как и ожидалось, код оказывается чрезвычайно громоздким. Во-первых, требуется правильная настройка всех трёх сред разработки. Мы также испытали много проблем, когда типы, которые приходили из JavaScript, оказывались неожиданными. Например, целые числа часто были обернуты в строки. Что еще хуже, иногда iOS будет молча проглатывать ошибки, в то время как Android рухнет. Мы начали исследовать автоматическую кодогенерацию из TypeScript кода к концу 2017 года, но было слишком поздно.

### Время инициализации
Прежде чем React Native сможет отрисовываться в первый раз, необходимо инициализировать его среду выполнения. К сожалению, даже на устройстве высокого класса это занимает несколько секунд для приложения нашего размера. Это сделало почти невозможным использование React Native для экранов запуска. Мы минимизировали время первого рендеринга для React Native, инициализируя его при запуске приложения.

### Начальное время рендеринга
В отличие от нативных экранов, рендеринг React Native требует по крайней мере одного полного цикла: основной поток -> JS -> поток Yoga -> основной поток, прежде чем будет достаточно информации для рендеринга экрана в первый раз. Мы наблюдали средний начальный рендер 280 мс для 90-го перцентиля на iOS и 440 мс на Android. На Android мы использовали [postponeEnterTransition API](https://developer.android.com/reference/android/app/Activity.html#postponeEnterTransition%28%29), которое обычно используется для общих переходов элементов, чтобы задержать отображение экрана до его визуализации. На iOS у нас были проблемы со скоростью настройки конфигурации navbar от React Native. В результате мы добавили искусственную задержку 50 мс для всех переходов экранов React Native, чтобы предотвратить мерцание navbar после загрузки конфигурации.

### Размер приложения
React Native также оказывает незначительное влияние на размер приложения. На Android общий размер React Native приложения (Java + JS + нативные библиотеки, такие как Yoga + JavaScript Runtime) составлял 8 Мб на ABI. С x86 и arm (только 32 бит) в одном APK, это было бы ближе к 12 Мб.

### 64-бита
Мы все еще не можем поставлять 64-битный APK на Android из-за [этой](https://github.com/facebook/react-native/issues/2814) проблемы.

### Жесты
Мы избегали использовать React Native для экранов, которые включали сложные жесты, потому что сенсорная подсистема для Android и iOS достаточно отличается, из-за чего создание единого API было вызовом для всего сообщества React Native. Однако работа прогрессирует и [react-native-gesture-handler](https://github.com/kmagiera/react-native-gesture-handler) достиг версии 1.0.

### Длинные списки
React Native добился некоторого прогресса в этой области с такими библиотеками, как [FlatList](https://facebook.github.io/react-native/docs/flatlist.html). Однако, эти библиотеки далеки от зрелости и гибкости [RecyclerView](https://developer.android.com/guide/topics/ui/layout/recyclerview) на Android или [UICollectionView](https://developer.apple.com/documentation/uikit/uicollectionview) на iOS. Многие проблемы трудно преодолеть из-за работы в разных потоках. Данные адаптера не могут быть доступны синхронно, поэтому можно видеть, что отображения мигают, поскольку они асинхронно отображаются при быстрой прокрутке. Текст также не может быть измерен синхронно, поэтому нельзя выполнить определенные оптимизации с предварительно вычисленными высотами ячеек.

### Обновление React Native
Хотя большинство обновлений React Native были тривиальными, было также несколько, которые были болезненными. В частности, было почти невозможно использовать React Native с 0.43 (апрель 2017) до 0.49 (октябрь 2017), потому что он использовал React 16 alpha и beta. Это было чрезвычайно проблематично, поскольку большинство React-библиотек, предназначенных для использования в веб, не поддерживают предварительные версии React. Процесс подбора правильных зависимостей для этого обновления был серьезным ущербом для другой работы над инфраструктурой для React Native в середине 2017 года.

### Доступность
В 2017 году мы провели большую работу над вопросами доступности, в процессе который мы приложили значительные усилия для обеспечения того, чтобы люди с ограниченными возможностями могли использовать Airbnb для бронирования. Однако в API специальных возможностей (accessibility API) React Native было много дыр. Для того, чтобы обеспечить даже минимально приемлемый уровень доступности, мы должны были поддерживать наш собственный форк React Native. В это случае однострочное исправление на Android или iOS занимало несколько дней, время уходило на выяснение того, как добавить его в React Native, копирование решения в ядро React Native, заведение issue и отслеживание его течение нескольких недель.

### Хлопотные крэши
Нам пришлось иметь дело с несколькими очень странными случаями сбоев у пользователей, которые трудно исправить. Например, мы в настоящее время испытываем [этот сбой](https://issuetracker.google.com/issues/37045084) в аннотации _@ReactProp_ и не смогли воспроизвести его на любом устройстве, даже с идентичным оборудованием и программным обеспечением.

### SavedInstanceState между процессами на Android
Android часто очищает фоновые процессы, но дает им возможность синхронно сохранять свое состояние. Однако на React Native все состояния доступны только в потоке JavaScript, поэтому это не может быть выполнено синхронно. Даже если это не так, Redux как хранилище состояний не совместим с этим подходом, поскольку он содержит сочетание сериализуемых и несериализуемых данных и может содержать больше данных, чем может поместиться в пакет savedInstanceState, что приведет к [сбоям в работе](https://medium.com/@mdmasudparvez/android-os-transactiontoolargeexception-on-nougat-solved-3b6e30597345).

---

Это вторая часть в серии записей, освещающих наш опыт работы с React Native в Airbnb.

Часть 1: [React Native в Airbnb](../gabriel-peal-react-native-at-airbnb)

Часть 2: [Технология](../gabriel-peal-react-native-at-airbnb-the-technology)

Часть 3: [Создание кроссплатформенной мобильной команды](../gabriel-peal-building-a-cross-platform-mobile-team)

Часть 4: [Принятие решения по React Native](../gabriel-peal-sunsetting-react-native)

Часть 5: [Что дальше с мобильной разработкой](../gabriel-peal-whats-next-for-mobile-at-airbnb)

- - - -

*Слушайте наш подкаст в [iTunes](https://itunes.apple.com/ru/podcast/девшахта/id1226773343) и [SoundCloud](https://soundcloud.com/devschacht), читайте нас на [Medium](https://medium.com/devschacht), контрибьютьте на [GitHub](https://github.com/devSchacht), общайтесь в [группе Telegram](https://t.me/devSchacht), следите в [Twitter](https://twitter.com/DevSchacht) и [канале Telegram](https://t.me/devSchachtChannel), рекомендуйте в [VK](https://vk.com/devschacht) и [Facebook](https://www.facebook.com/devSchacht).*
