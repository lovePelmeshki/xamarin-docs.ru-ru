---
title: Создание кода XIB в Xamarin. iOS
description: В этом документе описывается, как Xamarin. iOS создает код для отображения файлов. XIB в C#, делая визуальные элементы управления доступными программным способом.
ms.prod: xamarin
ms.assetid: 365991A8-E07A-0420-D28E-BC4D32065E1A
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 21085e534cee4010e79b76e39b11e03e6fb2580b
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84565035"
---
# <a name="xib-code-generation-in-xamarinios"></a>Создание кода XIB в Xamarin. iOS

> [!IMPORTANT]
> В этом документе объясняется, Visual Studio для Macа интеграция только с Xcode Interface Builder, так как действия и розетки не используются в Xamarin Designer для iOS. Дополнительные сведения о конструкторе iOS см. в документе, посвященном [конструктору iOS](~/ios/user-interface/designer/index.md) .

Средство Apple Interface Builder Tool («геодоступное») можно использовать для визуального проектирования пользовательских интерфейсов. Определения интерфейса, созданные функцией геохранения, сохраняются в файлах **XIB** . Мини-приложениям и другим объектам в **XIB** -файлах может быть присвоен идентификатор класса, который может быть пользовательским типом, определяемым пользователем. Это позволяет настраивать поведение мини-приложений и создавать пользовательские мини-приложения.

Эти классы пользователей обычно являются подклассами классов контроллеров пользовательского интерфейса. Они имеют *возможность* (аналогично свойствам) и *действия* (аналогичные события), которые могут быть подключены к объектам интерфейса. Во время выполнения, когда загружается файл с расширением, создаются объекты, а розетки и действия подключаются к различным объектам пользовательского интерфейса динамически. При определении этих управляемых классов необходимо определить все действия и функции, чтобы они совпадали с теми, которые предположительно заменяются. Для упрощения этого Visual Studio для Mac использует модель, подобную фоновому коду. Это похоже на то, что Xcode для цели-C, но модель создания кода и соглашения были оптимизированы, чтобы быть более привычными для разработчиков .NET.

Работа с файлами **. XIB** сейчас не поддерживается в Xamarin. iOS для Visual Studio.

## <a name="xib-files-and-custom-classes"></a>Файлы XIB и пользовательские классы

А также с помощью существующих типов из Cocoa Touch можно определить пользовательские типы в **XIB** -файлах. Также можно использовать типы, определенные в других **XIB** -файлах, или полностью в коде C#. В настоящее время Interface Builder не имеет сведений о типах, определенных за пределами текущего **XIB** -файла, поэтому они не будут перечисляться или отображать свои пользовательские розетки и действия. Удаление этого ограничения планируется в будущем.

Пользовательские классы можно определить в **XIB** -файле с помощью команды "добавить подкласс" на вкладке "классы" в Interface Builder. Мы будем называть их классами CodeBehind. Если **XIB** -файл содержит аналог файла ". XIB.Designer.cs" в проекте, Visual Studio для Mac автоматически заполнит его определениями разделяемых классов для всех пользовательских классов в **XIB**. Эти разделяемые классы называются "классами конструктора".

## <a name="generating-code"></a>Генерирование кода

Для любого ** {0} XIB** -файла с действием сборки *Page*, если в проекте также существует ** {0} XIB.Designer.CS** -файл, Visual Studio для Mac создаст разделяемые классы в файле конструктора для всех классов пользователей, которые он может найти в **XIB** -файле, со свойствами для розеток и разделяемых методов для всех действий. Создание кода включается просто при наличии этого файла.

Файл конструктора автоматически обновляется при изменении файла **. XIB** и Visual Studio для Mac восстанавливает фокус. Файл конструктора не должен изменяться вручную, поскольку изменения будут перезаписаны в следующий раз, Visual Studio для Mac обновляет файл.

## <a name="registration-and-namespaces"></a>Регистрация и пространства имен

Visual Studio для Mac создает классы конструктора, используя пространство имен по умолчанию проекта для расположения файла конструктора, чтобы обеспечить его соответствие обычным намеспаЦинг проекта .NET. Пространство имен файлов конструктора определяется по пространству имен по умолчанию проекта и его параметрам "политики именования .NET". Учтите, что при изменении пространства имен по умолчанию для проекта MD будет повторно создавать классы в новом пространстве имен, поэтому можно обнаружить, что разделяемые классы больше не соответствуют друг другу.

Чтобы сделать класс обнаруживаемым исполняющей средой цели-C, Visual Studio для Mac применяет `[Register (name)]` атрибут к классу. Хотя Xamarin. iOS автоматически регистрирует `NSObject` производные классы, он использует полные имена .NET. Атрибут, применяемый Visual Studio для Mac, переопределяет это, чтобы каждый класс был зарегистрирован с именем, используемым в **XIB** -файле. Если вы используете пользовательские классы в области без использования Visual Studio для Mac для создания файлов конструктора, может потребоваться применить это вручную, чтобы управляемые классы соответствовали ожидаемым именам классов цели-C.

Классы не могут быть определены более чем в одном **. XIB**, или они будут конфликтовать.

## <a name="non-designer-class-parts"></a>Части классов, не являющихся конструкторами

Разделяемые классы конструктора не предназначены для использования "как есть". Выходы являются частными, и базовый класс не указан. Предполагается, что каждый класс будет иметь соответствующую часть класса "не-Designer" в другом файле, который задает базовый класс, использует или предоставляет эти розетки, а также определяет конструкторы, необходимые для создания экземпляра класса из машинного кода при загрузке **. XIB**. Эти шаблоны **по** умолчанию делают это, но для любых дополнительных пользовательских классов, определенных в **XIB**, необходимо добавить элемент, не являющийся конструктором, вручную.

Причиной этого является потребность в гибкости. Например, несколько классов фонового кода могут создать подкласс общего управляемого абстрактного класса, который подклассировать класс, подклассом которого является.

Обычно они помещаются в ** {0} XIB.CS** -файл рядом с файлом конструктора ** {0} XIB.Designer.CS** .

<a name="generated"></a>

## <a name="generated-actions-and-outlets"></a>Созданные действия и розетки

В классах разделяемого конструктора Visual Studio для Mac создает свойства, соответствующие любым подключенным розеткам, определенным в геоотношении, и разделяемые методы, соответствующие любым связанным действиям.

### <a name="outlet-properties"></a>Свойства розетки

Классы конструктора содержат свойства, соответствующие всем розеткам, определенным в пользовательском классе. Тот факт, что эти свойства являются подробными сведениями о реализации в Bridge. iOS для целевого моста C, чтобы включить отложенную привязку. Их следует рассматривать как эквивалентные закрытым полям, которые предназначены для использования только из класса CodeBehind. Если вы хотите сделать их открытыми, добавьте свойства метода доступа в часть класса, не являющейся конструктором, как и для любого другого закрытого поля.

Если свойства розетки определены как тип `id` (эквивалентный `NSObject` ), генератор кода конструктора в настоящее время определяет наиболее надежный тип, основанный на объектах, подключенных к этой розетке, для удобства.
Однако это может не поддерживаться в будущих версиях, поэтому рекомендуется явно вводить их при определении пользовательского класса.

### <a name="action-properties"></a>Свойства действия

Классы конструктора содержат разделяемые методы, соответствующие всем действиям, определенным в пользовательском классе. Это методы без реализации. Разделяемые методы являются двойная:

1. Если вы вводите `partial` в тело класса части класса, не являющегося конструктором, Visual Studio для Mac предоставит Автозаполнение сигнатур всех нереализованных разделяемых методов.
2. Сигнатуры разделяемого метода имеют примененный атрибут, который предоставляет их для мира цели-C, поэтому они могут обрабатываться как соответствующее действие.

При желании можно проигнорировать разделяемый метод и реализовать действие, применив атрибут к другому методу или добавив его к базовому классу.

Если для действий определен тип отправителя `id` (эквивалентный `NSObject` ), генератор кода конструктора в настоящее время определяет наиболее надежный тип на основе объектов, подключенных к этому действию. Однако это может не поддерживаться в будущих версиях, поэтому рекомендуется явно вводить действия при определении пользовательского класса.

Обратите внимание, что эти разделяемые методы создаются только для C#, поскольку CodeDOM не поддерживает разделяемые методы, поэтому они не формируются для других языков.

## <a name="cross-xib-class-usage"></a>Использование классов с перекрестными XIB

Иногда пользователям нужно ссылаться на один и тот же класс из нескольких **XIB** файлов, например с помощью контроллеров вкладок. Это можно сделать, явно ссылаясь на определение класса из другого файла **XIB** или определив еще одно имя класса во втором **. XIB**.

Последний случай может быть проблематичным из-за Visual Studio для Mac обработки файлов **. XIB** по отдельности. Он не может автоматически обнаруживать и объединять дубликаты определений, поэтому могут возникнуть конфликты многократного применения атрибута Register, если один и тот же частичный класс определен в нескольких файлах конструктора. Последние версии Visual Studio для Mac попытаются разрешить эту проблему, но она может не всегда работать должным образом. В будущем это, скорее всего, станет неподдерживаемым, а Visual Studio для Mac сделает все типы, определенные во всех **XIB** -файлах, и управляемый код в проекте напрямую видимыми из всех **XIB** -файлов.

## <a name="type-resolution"></a>Разрешение типов

Типы, используемые в геоуровне, — это имена типов цели-C. Они сопоставляются с типами CLR с помощью атрибутов Register. При создании розетки и кода действия Visual Studio для Mac будет разрешать соответствующие типы CLR для всех типов цели-C, упакованных с помощью Xamarin. iOS Core, и указывать полные имена их типов.

Однако генератор кода не может в настоящее время разрешать типы CLR из имен типов цели-C в пользовательском коде или библиотеках, поэтому в таких случаях он выводит имя типа буквально. Это означает, что соответствующий тип CLR должен иметь то же имя, что и тип цели-C, и находиться в том же пространстве имен, что и код, который его использует. Это запланировано к исправлению в будущем, учитывая все типы цели-C в проекте во время создания кода.
