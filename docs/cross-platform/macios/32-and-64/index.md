---
title: "Замечания о 32/64-разрядной платформе"
description: "Рекомендации при разработке для 32-разрядных и 64-разрядной архитектуры для приложения"
ms.topic: article
ms.prod: xamarin
ms.assetid: F7126340-04B2-4A10-B14D-394E23527C1A
ms.technology: xamarin-cross-platform
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: af266bf9484f3a0da45173f4cfba22e0378ace22
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2018
---
# <a name="3264-bit-platform-considerations"></a>Замечания о 32/64-разрядной платформе

Во время операций ввода-вывода и macOS Исторически установлена поддерживаемая 32 и 64-разрядных приложений, Apple постепенно устаревшим поддержка 32-разрядных.

Начиная с версии iOS 11, больше не будет запускать 32-разрядных приложений, и [передачу магазина приложений должен поддерживать 64-разрядных](https://developer.apple.com/news/?id=06282017b).

Начиная с января 2018, [передан в магазине приложений Mac новые приложения должны поддерживать 64-разрядных](https://developer.apple.com/news/?id=06282017a), и существующие приложения должны быть обновлены до июня 2018.

Классический API Xamarin (`XamMac.dll` и `monotouch.dll`) поддерживается только 32-разрядных приложений. Однако использовать новые приложения Xamarin.iOS и Xamarin.Mac [единой API](~/cross-platform/macios/unified/index.md) (`Xamarin.iOS` и `Xamarin.Mac`) по умолчанию, и таким образом целевой 32 и 64-разрядная версия, при необходимости.

## <a name="ios"></a>iOS

<a name="enable-64" />

### <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Включение 64-разрядных сборок приложения Xamarin.iOS

> [!WARNING]
> Этот раздел включен для исторические причины, а также переместиться API единой старых проектов Xamarin.iOS и поддержку 64-разрядной. По умолчанию все новые проекты Xamarin.iOS будут использовать единый интерфейс API и целевой 64-разрядной.

Разработчики Xamarin.iOS мобильных приложений, которые были преобразованы в единой API, необходимо вручную обновить параметры сборки для 64-разрядных целевых:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. В **Pad решения**, дважды щелкните файл проекта приложения, чтобы открыть **параметры проекта** окна.
2. Выберите **сборка iOS**.
3. Для iPhone симулятор в **Поддерживаемые архитектуры** раскрывающийся список, выберите либо **x86\_64** или **i386 + x86\_64**:

   [![Поддерживаемые архитектуры параметру x86\_64 или i386 + x86\_64](Images/Image01.png "Setting Supported architectures to x86\_64 or i386 + x86\_64")](Images/Image01-large.png#lightbox) 

4. Для физических устройств выберите один из доступных **ARM64** комбинации:

   [![Указывая архитектур поддерживаемые сочетания ARM64](Images/Image02.png "поддерживается параметр архитектур одно из сочетаний ARM64")](Images/Image02-large.png#lightbox)

5. Нажмите кнопку **ОК**.
6. Выполните чистую сборку.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. В **обозревателе решений**, щелкните правой кнопкой мыши проект приложения и выберите **свойства**.
2. Выберите **сборка iOS**.
3. IPhone симулятор, задайте **Поддерживаемые архитектуры** либо **x86\_64** или **i386 + x86\_64**: 

   [![Установка Поддерживаемые архитектуры x86_64 или i386 + x86\_64](Images/VS02.png "Setting Supported architectures to x86_64 or i386 + x86\_64")](Images/VS02-large.png#lightbox)

4. Для физических устройств выберите один из доступных **ARM64** комбинации:
    
   [![Указывая архитектур поддерживаемые сочетания ARM64](Images/VS01.png "поддерживается параметр архитектур одно из сочетаний ARM64")](Images/VS01-large.png#lightbox)

5. Сохраните изменения.
6. Выполните чистую сборку.

-----

ARMv7s поддерживается только обработчиком A6, включенных в iPhone 5 (или более поздней). ARMv7 код быстрее и меньше, чем ARMv6 только работает с iPhone 3GS и более поздней версии и предусмотренного Apple, ориентируясь на iPad или минимальное iOS версии 5.0. ARMv6 работает на всех устройствах, но больше не поддерживается компилятором, поставляемый с Xcode 4.5 и более поздних версий. 

ARM64 необходима для поддержки iOS 8 на iPhone 6 или другие 64-разрядных устройствах и будет требоваться Apple при отправке или обновления, выход из приложений в магазине приложений iTunes.

Подробный обзор возможностей различных устройств iOS, см. Apple [совместимости устройств](https://developer.apple.com/library/content/documentation/DeviceInformation/Reference/iOSDeviceCompatibility/DeviceCompatibilityMatrix/DeviceCompatibilityMatrix.html) документа.

### <a name="64-bit-and-binary-size-increases"></a>64-разрядные и двоичный размер увеличивается

Во время перехода Apple из 32-разрядной на 64-разрядную версию iOS приложения будет необходимо запустить на 32-разрядных и 64-разрядного оборудования. По этой причине API единой Xamarin разработчики могут ориентироваться и.

Отбор информации о 32-разрядных и 64-разрядных архитектур значительно увеличит размер приложения. Тем не менее это так позволит более новых устройств для выполнения оптимизированный код, продолжая поддерживать старые устройства.

> [!IMPORTANT]
> Если появляется следующее сообщение при отправке приложения iOS в магазине приложений iTunes _«предупреждение ITMS-9000: отсутствует поддержка 64-разрядных. Запуск нового iOS 1 февраля 2015 г., приложения, отправить в магазин приложений необходимо включить поддержку 64-разрядных и быть построены с iOS 8 SDK, включенный в Xcode 6 или более поздней версии. Для включения 64 бита в вашем проекте, рекомендуется использовать значение по умолчанию, параметр архитектуры «стандартный» сборки Xcode для построения одного двоичного файла с 32-разрядных и 64-разрядного кода.»_ Необходимо перейти к одному из доступных поддерживаемых архитектур **ARM64** комбинации (как показано выше), перекомпиляции и повторной отправки.

## <a name="mac"></a>Mac

> [!IMPORTANT]
> Начиная с января 2018, все новые приложения Mac передан в магазине приложений Mac должен поддерживать 64-разрядной. Существующих приложений Mac App Store и их обновления должны поддерживать 64-разрядной, начиная с июня 2018. В разделе [отказ Apple](https://developer.apple.com/news/?id=06282017a) и [руководство описывает, как обновить приложения Xamarin.Mac на 64-разрядную версию](~/cross-platform/macios/32-and-64/mac-64-bit.md).

Большинство современных компьютеры Mac поддерживают 32-разрядных и 64-разрядных приложений.   MacOS 10.6 (Snow Leopard) была последней операционной системы для запуска в 32-разрядных системах.   Большинство компьютеров Mac, вышедшие после выпуска 2010 поддерживает обе системы.

В отличие от операций ввода-вывода многие из новых платформ, реализованные в последних версиях macOS поддерживаются только в 64-разрядном режиме (CloudKit, EventKit, игровое, LocalAuthentication, MediaLibrary, MultipeerConnectivity, NotificationCenter, GLKit, SpriteKit, социальных, и MapKit среди прочих).

API единой разработчики могут выбрать, какие приложения для создания: 32-разрядная или 64-разрядной.

**32-разрядные приложения** будет использоваться на 32-разрядных и 64-разрядных компьютерах Mac, назначено адресное пространство ограничено 32 бита и требуют, чтобы все библиотеки 32 бита.

Этот режим обычно используется при наличии 32-разрядных зависимости, которые не выполняются в 64-разрядном режиме, если вы хотите иметь меньшее загрузки или в случае выигрыш в производительности при перемещении на 64-разрядную версию.

В этом режиме ограничивается, как вы не сможете использовать множество платформ, доступные в macOS Mavericks и macOS Yosemite.

**64-разрядных приложений** будет выполняться только на 64-разрядных устройствах Mac.

Для Mac Это предпочтительный режим работы как большинство компьютеров Mac, используется в настоящее время поддерживает 64-разрядном режиме, и у вас есть доступ ко всем платформ компании Apple.

### <a name="enabling-64-bit-builds-of-xamarinmac-apps"></a>Включение 64-разрядных сборок Xamarin.Mac приложений

Сведения о построении 64-разрядное приложение, с помощью Xamarin.Mac см. в разделе [обновление единой Xamarin.Mac приложений на 64-разрядную версию](~/cross-platform/macios/32-and-64/mac-64-bit.md) руководства.

## <a name="related-links"></a>Связанные ссылки

- [Классический vs отличия единой API](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)