---
title: "Интерактивные книги"
description: "Используйте книги, для создания динамической документов с кодом C# для экспериментов, обучение, обучение и просмотр."
ms.topic: article
ms.prod: xamarin
ms.assetid: B79E5DE9-5389-4691-9AA3-FF4336CE294E
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: c111d2f873270eab78eee92edc3d884d1e92fdd8
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="interactive-workbooks"></a>Интерактивные книги

_Используйте книги, для создания динамической документов с кодом C# для экспериментов, обучение, обучение и просмотр._

Книги можно использовать как отдельное приложение, отдельно от интерфейс IDE.

Чтобы начать создание новой книги, запустите приложение книг. Если это уже не установлен, посетите [установки](~/tools/workbooks/install.md#install) страницы. Вам будет предложено создать книгу в вашей платформе, которой будет автоматически подключаться к приложению агента, что позволяет визуализировать документа в режиме реального времени.

Если книг приложение уже запущено, можно создать новый документ, перейдя к **файл > создать**.

Книги можно сохранить и позже открыть в приложении. Вы можете затем использовать их с другими пользователями для демонстрации идеи, изучить новые интерфейсы API или обучение новых понятий.

## <a name="code-editing"></a>Редактирование кода

Окно предоставляет дополнение кода, Цветовая подсветка синтаксиса, встроенные быстрой диагностики и поддержка многострочного оператора.

[ ![](workbook-images/inspector-0.6.0-repl-small.png "Предоставляет окно дополнение кода, Цветовая подсветка синтаксиса, встроенные быстрой диагностики и поддержка многострочного оператора")](workbook-images/inspector-0.6.0-repl.png#lightbox)

Xamarin книги будут сохраняться в `.workbook` файл, который представляет собой файл CommonMark с определенными метаданными в верхней (см. [типы файлов книг](#Workbooks_Files_Types) для получения дополнительных сведений о Сохранение книг).

### <a name="nuget-package-support"></a>Поддержка пакета NuGet

Многие популярные пакеты NuGet поддерживаются непосредственно в книгах Xamarin. Можно выполнить поиск пакетов, перейдя к **файл > добавить пакет**. Добавление пакета автоматически добавит `#r` инструкций, ссылки на сборки пакета, позволяя сразу их использовать.

При сохранении книги с ссылками на пакет, также сохраняются эти ссылки. Если книга совместно с другими пользователями, он автоматически загружать пакеты, на которую указывает ссылка.

Существуют некоторые ограничения с поддержкой пакета NuGet в книгах:

  * Собственные библиотеки поддерживается только в iOS и только в том случае, когда он связан с управляемой библиотеки.
  * Пакеты, которые зависят от `.targets` файлы или сценарии PowerShell скорее всего не будут работать должным образом.
  * Чтобы удалить или изменить зависимость пакета, измените манифест книги с помощью текстового редактора. Правильный пакет управления находится в разработке.

### <a name="xamarinforms-support"></a>Поддержка Xamarin.Forms

При ссылке пакет Xamarin.Forms NuGet в книге, приложение книги изменит его основного представления на основе Xamarin.Forms. Доступен через `Xamarin.Forms.Application.Current.MainPage`.

На вкладке представление инспектор также имеется специальную поддержку для отображения иерархии представления Xamarin.Forms поможет вам понять макеты.

## <a name="rich-text-editing"></a>Редактирования форматированного текста

Можно изменить текст вокруг кода в редакторе форматированного текста включен, как показано ниже:

![](workbook-images/inspector-0.6.2-editing.gif "Изменение текста, вокруг кода, с помощью редактора встроенной форматированного текста")

### <a name="markdown-authoring"></a>Создание разметки

Авторы книг может иногда проще для непосредственного редактирования CommonMark «источник» книги с помощью любимого редактора.

Имейте в виду, если изменение и сохранение книги в клиентском книг, CommonMark текст может быть переформатированы.

Обратите внимание, что из-за расширения CommonMark используется для включения YAML метаданных в файлах книги `---` зарезервирован для этой цели. Если вы хотите создать [тематический разрывов](http://spec.commonmark.org/0.27/#thematic-break) в тексте, следует использовать `***` или `___` вместо него. Следует избегать таких разрывы в книгах 1.2 и более ранних версий из-за ошибки во время сохранения.

### <a name="improvements-in-workbooks-13"></a>Усовершенствования в книгах 1.3

Мы также расширил синтаксис кавычки блок разметки немного для улучшения презентации. Путем добавления в качестве первого символа в вашу квоту блок emoji, могут повлиять на цвет фона квоты:

- `> [!NOTE]
>"будут отображаться как примечание с синим фоном
- `> [!IMPORTANT]
>"будут отображаться как предупреждение с желтым фоном
- `> [!WARNING]
>"будут отображаться как проблемы с красным фоном

Можно также связать верхние колонтитулы документа книги. Мы создаем привязки для каждого заголовка с Идентификатором привязки, который текст заголовка, обрабатываются следующим образом:

1. Заголовок в нижнем регистре.
1. Удалены все символы, за исключением буквы и цифры и дефисы.
1. Все пробелы заменяются на тире.

Это означает, что заголовок как «Важные заголовок» возвращает идентификатор `important-header` и могут быть связаны, вставив ссылку `#important-header` в книге.

## <a name="document-structure"></a>Структура документа

### <a name="cell"></a>Ячейки

Дискретный элемент содержимого, представляющий исполняемого кода или разметки. Код ячейки состоит из до четырех подкомпоненты:

- Редактор
  - Буфер
- Диагностики компилятора
- Вывод на консоль
- Результаты выполнения

### <a name="editor"></a>Редактор

Компонент интерактивного текст ячейки. Для ячейки кода это редактор фактический код с выделение синтаксиса и т. д. Для разметки ячеек это редактор содержимого форматированного текста, зависящие от контекста форматирования и создания панели инструментов.

### <a name="buffer"></a>Буфер
Фактический текстовое содержимое редактора.

### <a name="compiler-diagnostics"></a>Диагностики компилятора

Любой диагностических создаются при компиляции кода, отображается только в том случае, когда запрашивается явное выполнение. Отображаются сразу под областью редактора ячейки.

### <a name="console-output"></a>Вывод на консоль

Выходные данные записываются в стандартный выход или стандартные ошибки во время выполнения ячейки. Черный текст будет отображен стандартный выход и Стандартная ошибка отображается красным шрифтом.

### <a name="execution-results"></a>Результаты выполнения

Потенциально интерактивные представления результатов для ячейки будут отображаться после успешной компиляции, предоставляемых фактически создается результат выполнения. Исключения считаться результаты в этом контексте, так как они создаются в результате фактического выполнения компиляции.

## <a name="workbooks-files-types"></a>Типы файлов книг

### <a name="plain-files"></a>Обычные файлы

По умолчанию сохраняет книгу в виде простого текста `.workbook` файл, содержащий текст в формате CommonMark.

### <a name="packages"></a>Пакеты

Пакет книги — это каталог, с именем `.workbook` расширение.
На Mac поиска, а также в диалоговое окно открытия книги Xamarin и последние файлы меню будут распознаваться этот каталог, как если бы это был файл.

Каталог должен содержать `index.workbook` файла, являющегося фактическое обычный текст книги, которая будет загружаться в Xamarin книги. Каталог также может содержать ресурсы, необходимые для `index.workbook`, включая изображения и другие файлы.

Если обычный текст `.workbook` открывается файл, который ссылается на ресурсы из одного каталога в книгах 0.99.3 или более поздней версии, при ее сохранении, он будет преобразован в `.workbook` пакета. Это справедливо на Mac и Windows.

> [!NOTE]
> **Примечание:** Windows пользователи будут открывать `package.workbook\index.workbook` непосредственно, но в противном случае пакет будет функционировать так же, как и на компьютере Mac.

### <a name="archives"></a>Архивы

Пакеты книги, выполняется в каталогах, могут быть трудными для распространения легко через Интернет. Решением является архивы книги. Архив книги — это пакет сжатых zip книги, с именем `.workbook` расширение.

Диалоговое окно сохранения, начиная с версии 1.1 книги при сохранении пакета книги, позволяет выбирать вместо сохранения как архив. Книги 1.0 не было встроенного средства для создания и сохранения архивов.

В 1.0 книг при открытии в архиве книги, он прозрачно был преобразован в пакет книги и ZIP-файле было потеряно. В версии 1.1 книг остается ZIP-файл. Когда пользователь сохраняет архив, заменяется новый ZIP-файл.

Можно создать архив книгу вручную, щелкнув правой кнопкой мыши пакет книгу и выбрав **сжимать** на Mac, или **Отправить > Сжатая ZIP-папке** в Windows. Переименуйте ZIP-файл, чтобы `.workbook` расширение имени файла. Это работает только с пакетами книги, файлы не простой книги.

## <a name="related-links"></a>Связанные ссылки

- [Добро пожаловать в книгах](https://developer.xamarin.com/workbooks/workbooks/getting-started/welcome.workbook)