---
title: Введение в Android Wear
description: С появлением износа Android Google вы больше не ограничены только телефонами и планшетами, когда дело доходит до разработки отличных приложений Android. Поддержка Xamarin. Android для износа Android позволяет выполнять C# код на вашем компьютере. В этом вводе представлен базовый обзор износа Android, описываются его основные возможности и предоставляются общие сведения о функциях, доступных в Android износ 2,0. В нем перечислены некоторые из наиболее популярных устройств, предоставляющих Android, а также ссылки на дополнительные материалы по документации по износу Google Android для дальнейшего ознакомления.
ms.prod: xamarin
ms.assetid: EAEF99F0-8FBE-47E4-8644-E7244CFAF464
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 3b1d27b1489cb71d4bd1922c2de993567ddf36bd
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028612"
---
# <a name="introduction-to-android-wear"></a>Введение в Android Wear

_С появлением износа Android Google вы больше не ограничены только телефонами и планшетами, когда дело доходит до разработки отличных приложений Android. Поддержка Xamarin. Android для износа Android позволяет выполнять C# код на вашем компьютере. В этом вводе представлен базовый обзор износа Android, описываются его основные возможности и предоставляются общие сведения о функциях, доступных в Android износ 2,0. В нем перечислены некоторые из наиболее популярных устройств, предоставляющих Android, а также ссылки на дополнительные материалы по документации по износу Google Android для дальнейшего ознакомления._

## <a name="overview"></a>Обзор

Износ Android работает на различных устройствах, в том числе на основе первого поколения в Motorola 360, LG и на платформе Samsung. Второе поколение, включая Sony Смартватч 3, также было выпущено с дополнительными возможностями, включая встроенные средства GPS и воспроизведение музыки в автономном режиме. Для Android износ 2,0 Google был объединен с LG для двух новых контрольных значений: LG Watch Спорт и LG Watch.

![Устройства Android износа 2,0](intro-to-wear-images/hero-image.png "Пример устройств Android износа 2,0")

Xamarin. Android 5,0 и более поздних версий поддерживает износ Android через поддержку Android 4.4 W (API 20) и пакет NuGet, который добавляет дополнительные элементы управления, связанные с износом. Xamarin. Android 5,0 и более поздней версии также включает функциональные возможности для упаковки приложений для износа. Пакеты NuGet также доступны для Android износа 2,0, как описано далее в этом разделе.

## <a name="android-wear-basics"></a>Основы износа Android

У Android износа есть парадигма пользовательского интерфейса, отличающаяся от того, что наладонные приложения Android. Первая волна приложений-поэтапного развертывания была разработана для того, чтобы расширить сопутствующее мобильное приложение каким-либо образом, но начиная с Android износа 2,0, приложения с износом можно использовать автономно. При развертывании приложения "износ" оно упаковывается в сопутствующее приложение для портативных компьютеров. Так как большинство получающих приложений зависят от портативного приложения, им требуется какой-то способ взаимодействия с карманными приложениями. В следующих разделах описываются эти сценарии использования и приводятся основные функции износа Android. 

### <a name="usage-scenarios"></a>Сценарии использования

Первая версия средства износа Android в первую очередь посвящена расширению текущих приложений для карманных компьютеров с улучшенными уведомлениями и синхронизацией данных между карманным приложением и приложением носимого пользователем. Таким образом, эти сценарии относительно просты в реализации.

#### <a name="wearable-notifications"></a>Уведомления носимого пользователем

Самым простым способом поддержки износа Android является использование преимуществ общего характера уведомлений между КПК и устройством носимого пользователем. Используя API уведомления поддержки v4 и класс `WearableExtender` (доступный в [библиотеке поддержки Xamarin Android](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)), можно коснуться встроенных функций платформы, таких как входящие и речевые входные данные. Образец [реЦипеассистант](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-recipeassistant) содержит пример кода, демонстрирующий, как отправить список уведомлений на устройство "износ Android". 

#### <a name="companion-applications"></a>Сопутствующие приложения

Другая стратегия состоит в том, чтобы создать полноценное приложение, которое работает на носимого пользователем устройстве и парах с сопутствующим карманным приложением. Хорошим примером такого подхода является пример приложения " [Викторина](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-quiz) ", в котором показано, как создать контрольный опрос, выполняемый на портативном устройстве, и запросите контрольные вопросы на устройстве носимого пользователем. 

### <a name="user-interface"></a>Пользовательский интерфейс

Основной шаблон навигации для износа — это серия карт, расположенных вертикально. Каждая из этих карточек может иметь связанные действия, которые размещаются в одной и той же строке. Класс `GridViewPager` предоставляет эти функции. Он соответствует той же концепции адаптера, что и `ListView`. Как правило, `GridViewPager` связывается с `FragmentGridPagerAdaptor` (или `GridPagerAdaptor`), который позволяет представить ячейки строк и столбцов как `Fragment`: 

[![Переход по износу](intro-to-wear-images/2d-picker-sml.png "Переход по износу")](intro-to-wear-images/2d-picker.png#lightbox)

Износ также использует кнопки действий, состоящие из большого цветного круга с небольшим текстом описания под ним (как показано выше).  В примере [гридвиевпажер](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-gridviewpager) показано, как использовать `GridViewPager` и `GridPagerAdapter` в приложении "износ".

В Android износ 2,0 добавляется контейнер навигации, ящик действий и кнопки встроенных действий в пользовательский интерфейс износа. Дополнительные сведения об элементах пользовательского интерфейса Android износа 2,0 см. в разделе, посвященном Android [анатомии](https://www.google.com/design/spec-wear/system-overview/anatomy.html) . 

### <a name="communications"></a>Взаимодействие

Для упрощения обмена данными между приложениями носимого пользователем и сопровождающими карманными приложениями в Android предусмотрено два разных интерфейса API взаимодействия: 

**API данных** &ndash; этот API аналогичен синхронизированному хранилищу данных между устройством носимого пользователем и карманным устройством. Android берет на себя распространение изменений между носимого пользователем и КПК, когда это оптимально. Если носимого пользователем выходит за пределы диапазона, он помещает синхронизацию в очередь на более позднее время. Главная точка входа для этого API — `WearableClass.DataApi`. Дополнительные сведения об этом API см. в статье [Синхронизация элементов данных](https://developer.android.com/training/wearables/data-layer/data-items.html) Android. 

**API сообщений** &ndash; этот API позволяет использовать путь обмена данными на более низком уровне: небольшие полезные данные отправляются односторонне без синхронизации между карманными и носимого пользователем приложениями.
Главная точка входа для этого API — `WearableClass.MessageApi`.
Дополнительные сведения об этом API см. в разделе [Отправка и получение сообщений](https://developer.android.com/training/wearables/data-layer/messages.html) Android.

Вы можете зарегистрировать обратные вызовы для получения этих сообщений через каждый интерфейс прослушивателя API или, Кроме того, реализовать в приложении службу, производную от `WearableListenerService`.
Эта служба будет автоматически создаваться при износе Android.
В примере [финдмифоне](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-findmyphonesample) показано, как реализовать `WearableListenerService`.

### <a name="deployment"></a>Развертывание

Каждое приложение носимого пользователем развертывается с собственным APK-файлом, внедренным в главное приложение APK. Эта упаковка обрабатывается автоматически в Xamarin. Android 5,0 и более поздних версиях, но ее необходимо выполнять вручную для версий Xamarin. Android, предшествующих версии 5,0. 
[Работа с упаковкой](~/android/wear/deploy-test/packaging.md) объясняет развертывание более подробно. 

## <a name="going-further"></a>Дальнейшие переходы 

Лучший способ познакомиться с износом Android — это создать и протестировать свое первое приложение. В следующем списке приведен рекомендуемый порядок чтения, позволяющий быстро приступить к ускорению.

1. [Программа установки & установки](~/android/wear/get-started/installation.md) предоставляет подробные инструкции по установке и настройке среды разработки для создания приложений Xamarin. Android износа. 

2. После установки необходимых пакетов и настройки эмулятора или устройства ознакомьтесь с разработкой " [Привет",](~/android/wear/get-started/hello-wear.md) чтобы получить пошаговые инструкции, в которых объясняется, как создать небольшой проект "износ Android", который обрабатывает нажатия кнопки и отображает счетчик щелчков по износу. модем. 

3. [Тестирование & развертывания](~/android/wear/deploy-test/index.md) предоставляет более подробные сведения о настройке и развертывании для эмуляторов и устройств, включая инструкции по развертыванию приложения на устройстве с помощью Bluetooth.

4. [При работе с размером экрана](~/android/wear/screen-sizes.md) объясняется, как просматривать и оптимизировать пользовательский интерфейс для различных доступных размеров экрана на износе устройств. 

5. [Работа с упаковкой](~/android/wear/deploy-test/packaging.md) описывает шаги для ручного упаковки приложений для распространения на Google Play.

После создания первого приложения для износа можно попробовать создать пользовательский человек для наблюдения за настройкой Android. 
[Создание циферблата с контрольными](~/android/wear/platform/creating-a-watchface.md) данными содержит пошаговые инструкции и примеры кода для разработки отключенной службы Digital Watch, а также кода, который расширяет его до последующего просмотра с помощью дополнительных функций. 

## <a name="android-wear-20"></a>Неизнос Android 2,0

В Android износ 2,0 введено множество новых функций и возможностей, таких как *сложности*, кривые макеты, средства отображения навигации и пометки действий, а также расширенные уведомления. Кроме того, износ 2,0 дает возможность создавать автономные приложения, работающие независимо от карманных приложений. Новые *жесты* наличных жестов позволяют выполнять Однопользовательское взаимодействие с приложением. В следующих разделах описаны эти функции и приведены ссылки, которые помогут приступить к работе с ними в приложении.

### <a name="install-wear-20-packages"></a>Установка пакетов износа 2,0

Чтобы создать приложение для износа 2,0 с помощью Xamarin. Android, необходимо добавить в проект пакет **Xamarin. Android. износа версии 2.0** (перейдите на **вкладку Обзор**):

[![Xamarin. Android. износ v 2.0](intro-to-wear-images/wear-nuget-2.0-sml.png "Установка Xamarin. Android. износа версии 2.0 NuGet")](intro-to-wear-images/wear-nuget-2.0.png#lightbox)

Этот пакет NuGet содержит привязки для носимого пользователем и несовместимости библиотек поддержки Android.

Помимо **Xamarin. Android. износа**, мы рекомендуем установить NuGet **Xamarin. гуглеплайсервицес. носимого пользователем** : 

[![Xamarin. Гуглеплайсервицес. носимого пользователем](intro-to-wear-images/gpsw-nuget-sml.png "Установка Xamarin. Гуглеплайсервицес. носимого пользователем NuGet")](intro-to-wear-images/gpsw-nuget.png#lightbox)

### <a name="key-features-of-wear-20"></a>Основные возможности износа 2,0

Android износ 2,0 — это самое главное обновление для износа Android с момента его первоначального запуска в 2014. В следующих разделах рассматриваются основные возможности Android износа 2,0, а также приведены ссылки, которые помогут приступить к использованию этих новых функций в приложении. 

#### <a name="complications"></a>Усложнения

*Сложности* — это небольшие мини-приложения для просмотра, которые можно легко увидеть без необходимости прокрутки циферблата. Сложности похожи на мини-приложения панели мониторинга в стиле рабочего стола; в них отображаются такие сведения, как погода, время работы батареи, события календаря и статистика приложений по подфитнесу: 

![Пример сложности](intro-to-wear-images/complications.png "Пример сложности")

Дополнительные сведения о сложностях см. в статье о возвыстях Android [Watch](https://developer.android.com/wear/preview/features/complications.html) . 

#### <a name="navigation-and-action-drawers"></a>Навигацияы и прорисовки действий 

В износ 2,0 включены два новых рисования. *Область навигации*, отображаемая в верхней части экрана, позволяет пользователям перемещаться между представлениями приложений (как показано слева ниже). *Ящик действий*, который отображается в нижней части экрана (как показано справа), позволяет пользователям выбирать из списка действий. 

![Навигацияы и прорисовки действий](intro-to-wear-images/drawers.png "Навигацияы и прорисовки действий")

Дополнительные сведения об этих двух новых интерактивных отрисовок см. в разделе [переходы и действия по износу](https://developer.android.com/wear/preview/features/ui-nav-actions.html) Android. 

#### <a name="curved-layouts"></a>Изогнутые макеты 

В ходе износа 2,0 появились новые функции для отображения искривленных макетов на устройствах с закругленными износами. В частности, новый класс `WearableRecyclerView` оптимизирован для отображения списка вертикальных элементов на круговых экранах: 

![Пример криволинейного макета](intro-to-wear-images/curved-layout.png "Пример криволинейного макета")

`WearableRecyclerView` расширяет класс `RecyclerView` для поддержки искривленных макетов и циклических жестов прокрутки. Дополнительные сведения см. в документации по API [веараблерециклервиев](https://developer.android.com/reference/android/support/wearable/view/WearableRecyclerView.html) для Android. 

#### <a name="standalone-apps"></a>Автономные приложения 

Приложения Android износа 2,0 могут работать независимо от карманных приложений. Это означает, что, например, с помощью интеллектуальных контрольных значений можно продолжать предоставлять полную функциональность даже в том случае, если сопутствующее карманное устройство выключено или находится далеко от устройства носимого пользователем. Дополнительные сведения об этой функции см. в разделе [автономные приложения](https://developer.android.com/wear/preview/features/standalone-apps.html) Android.

#### <a name="wrist-gestures"></a>Жесты надвижений 

Жесты ручного ввода позволяют пользователям взаимодействовать с приложением без использования сенсорного экрана, &ndash; пользователи могут реагировать на приложение с одной рукой. Поддерживаются два жеста: 

- Жест на выходе
- Жест жестов в

Дополнительные сведения см. в разделе [жесты](https://developer.android.com/wear/preview/features/gestures.html) для Android. 

Существует множество более нестандартных функций 2,0, таких как встроенные действия, интеллектуальный ответ, удаленный вход, расширенные уведомления и новый режим моста для уведомлений. Дополнительные сведения о новых функциях износа 2,0 см. в статье [Общие сведения об API](https://developer.android.com/wear/preview/api-overview.html)Android. 

## <a name="devices"></a>Устройства

Ниже приведены некоторые примеры устройств, которые могут выполнять износ Android.

- [Motorola 360](https://moto360.motorola.com/)
- [LG G, часы](https://www.lg.com/us/smart-watches/lg-W100-g-watch)
- [LG G Watch R](https://www.lg.com/us/smartwatch/g-watch-r)
- [Живая шестеренка Samsung](https://www.samsung.com/global/microsite/gear/gearlive_design.html)
- [Sony Смартватч 3](https://www.sonymobile.com/global-en/products/smartwear/smartwatch-3-swr50/)
- [Плата ASUS Зенватч](https://www.asus.com/us/Phones/ASUS_ZenWatch_WI500Q/)

## <a name="further-reading"></a>Дополнительные сведения

Ознакомьтесь с документацией по износу Android Google:

- [Об износе Android](https://www.android.com/wear/)
- [Разработка приложения "износ Android"](https://developer.android.com/design/wear/index.html)
- [Библиотека Android. support. носимого пользователем](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)
- [Неизнос Android 2,0](https://developer.android.com/wear/preview/index.html)

## <a name="summary"></a>Сводка

В этом вводе предоставлен обзор износа Android. В нем описаны основные функции износа Android и приведены общие сведения о функциях, появившихся в Android износа 2,0. В нем предоставлены ссылки на наиболее подходящие материалы для разработчиков, которые помогут разработчикам начать работу с использованием Xamarin. Android, и в нем были приведены примеры некоторых из устройств Android, находящихся на рынке.

## <a name="related-links"></a>Связанные ссылки

- [Установка и настройка](~/android/wear/get-started/installation.md)
- [Начало работы](~/android/wear/get-started/index.md)
