---
title: Общие сведения о Cognitive Services Xamarin.Forms и Azure
description: В этой статье введение в пример приложения, которое демонстрирует способ вызова некоторые интерфейсы API Microsoft Cognitive Service.
ms.prod: xamarin
ms.assetid: 74121ADB-1322-4C1E-A103-F37257BC7CB0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 36aa53a6d257d8f5311cab84485e608bef3e97f8
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2019
ms.locfileid: "67659271"
---
# <a name="xamarinforms-and-azure-cognitive-services-introduction"></a>Общие сведения о Cognitive Services Xamarin.Forms и Azure

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)

_Microsoft Cognitive Services — это набор API-интерфейсов, пакетов SDK и службы, доступные разработчикам сделать свои приложения более интеллектуальных, добавив функции, такие как распознавание лиц, речи и понимание языка. В этой статье введение в пример приложения, в котором показано, как вызывать некоторые интерфейсы API Microsoft Cognitive Service._

## <a name="overview"></a>Обзор

Сопутствующий пример является приложением списка заданий, предоставляет функциональные возможности для:

- Просмотр списка задач.
- Добавление и изменение задач через Экранная клавиатура или выполняет распознавание речи с помощью API распознавания речи Microsoft. Дополнительные сведения о выполнении распознавания речи см. в разделе [распознавание речи, используя API распознавания речи Microsoft](speech-recognition.md).
- Орфографии проверки задач, с помощью API проверки орфографии Bing. Дополнительные сведения см. в разделе [проверка орфографии с помощью API проверки орфографии Bing](spell-check.md).
- Перевод задач из английский, немецкий язык, с помощью API перевода. Дополнительные сведения см. в разделе [перевод текста с помощью API перевода](text-translation.md).
- Удаление задач.
- Задайте состояние задания на «Готово».
- Оцените приложение с помощью распознавания эмоций, с помощью API распознавания лиц. Дополнительные сведения см. в разделе [распознавания эмоций, с помощью API распознавания лиц](emotion-recognition.md).

Задачи будут храниться в локальной базе данных SQLite. Дополнительные сведения об использовании локальной базы данных SQLite, см. в разделе [работа с локальной базой данных](~/xamarin-forms/data-cloud/data/databases.md).

`TodoListPage` Отображается при запуске приложения. Эта страница отображается список всех задач, хранящихся в локальной базе данных и позволяет пользователю создать новую задачу или Оцените приложение:

![](introduction-images/sample-application-1.png "TodoListPage")

Можно создать новые элементы, щелкнув *+* кнопку, которая переходит к `TodoItemPage`. Эту страницу можно также перейти, выбрав задачу:

![](introduction-images/sample-application-2.png "TodoItemPage")

`TodoItemPage` Позволяет задачам создаваться, редактирование, проверка которого преобразуется, сохраняются и удалены. Система распознавания речи можно использовать для создания или изменения задачи. Это достигается, нажав кнопку "микрофон", чтобы начать запись и, нажав клавишу ту же кнопку еще раз, чтобы остановить запись, которая отправляет записи в API распознавания речи Bing.

Щелкнув кнопку smilies `TodoListPage` переходит к `RateAppPage`, который используется для выполнения распознавание эмоций лица на изображении лица выражения:

![](introduction-images/sample-application-3.png "RateAppPage")

`RateAppPage` Пользователь может сделать фотографию их сторону, которой были отправлены в API распознавания лиц с отображением возвращаемый распознавания эмоций.

## <a name="understand-the-application-anatomy"></a>Понимать структуру приложений

Проект общего кода для примера приложения состоит из пяти основных папок:

|Папка|Цель|
|--- |--- |
|Модели|Классы модели данных для приложения. Сюда входят `TodoItem` класс, который моделирует один элемент данных, используемых приложением. Папка также содержит классы, используемые для модели JSON ответы, возвращаемые из разных Microsoft Cognitive Service интерфейсов API.|
|Репозитории|Содержит `ITodoItemRepository` интерфейс и `TodoItemRepository` класса, которые используются для выполнения операций базы данных.|
|Службы|Содержит интерфейсы и классы, используемые для доступа к другой Microsoft Cognitive Service API, вместе с интерфейсами, которые используются `DependencyService` для нахождения классы, реализующие интерфейсы в проектах платформы.|
|Utils|Содержит `Timer` класс, используемый методом `AuthenticationService` класса, чтобы обновить маркер доступа JWT каждые 9 минут.|
|Представления|Содержит страницы для приложения.|

Проект общего кода также содержит некоторые важные файлы:

|Файл|Цель|
|--- |--- |
|Constants.cs|`Constants` Класс, который указывает ключи API и конечных точек для Microsoft Cognitive Service интерфейсы API, которые вызываются. Константы ключа API требуется обновление для доступа к другой API Cognitive Services.|
|App.xaml.cs|`App` Класс отвечает за создание экземпляра оба первой страницы, будет отображен приложением на каждой платформе и `TodoManager` класс, который используется для вызова операций базы данных.|

### <a name="nuget-packages"></a>Пакеты NuGet

В примере приложения используются следующие пакеты NuGet:

- `Newtonsoft.Json` — Платформа JSON для .NET.
- `PCLStorage` — предоставляет набор кросс платформенных локальный файл API ввода-ВЫВОДА.
- `sqlite-net-pcl` — место хранения базы данных SQLite.
- `Xam.Plugin.Media` — предоставляет кросс платформенных photo ведения и комплектации API-интерфейсы.

Кроме того эти пакеты NuGet также устанавливают свои собственные зависимости.

### <a name="model-the-data"></a>Моделирования данных

В примере приложения используется `TodoItem` класс для моделирования данных, отображаются и хранятся в локальной базе данных SQLite. Следующий пример кода демонстрирует класс `TodoItem`:

```csharp
public class TodoItem
{
  [PrimaryKey, AutoIncrement]
  public int ID { get; set; }
  public string Name { get; set; }
  public bool Done { get; set; }
}
```

`ID` Свойство используется для уникальной идентификации каждого `TodoItem` экземпляр и снабжен SQLite атрибутами, которые делают свойства заданы с автоматическим приращением первичного ключа в базе данных.

### <a name="invoke-database-operations"></a>Вызова операций базы данных

`TodoItemRepository` Класс реализует операции с базой данных, а экземпляр класса может осуществляться через `App.TodoManager` свойство. `TodoItemRepository` Класс предоставляет следующие методы для вызова операций базы данных:

- **GetAllItemsAsync** — получает все элементы из локальной базы данных SQLite.
- **GetItemAsync** — Извлекает указанный элемент из локальной базы данных SQLite.
- **SaveItemAsync** — создает или обновляет элемент в локальной базе данных SQLite.
- **DeleteItemAsync** — Удаляет указанный элемент из локальной базы данных SQLite.

### <a name="platform-project-implementations"></a>Реализации платформы проекта

`Services` Папка в проекте с общим кодом содержит `IFileHelper` и `IAudioRecorderService` интерфейсы, используемые `DependencyService` класса классы, реализующие интерфейсы в проектах платформы.

`IFileHelper` Интерфейс реализуется `FileHelper` класс в каждом проекте платформы. Этот класс состоит из единственного метода `GetLocalFilePath`, который возвращает путь к локальному файлу для хранения базы данных SQLite.

`IAudioRecorderService` Интерфейс реализуется `AudioRecorderService` класс в каждом проекте платформы. Этот класс состоит из `StartRecording`, `StopRecording`и вспомогательные методы, которые используют интерфейсы API платформы записывать звук из микрофон устройства и сохраните его в формате WAV. В iOS `AudioRecorderService` использует `AVFoundation` API записывать звук. В Android `AudioRecordService` использует `AudioRecord` API записывать звук. В универсальной платформы Windows (UWP), `AudioRecorderService` использует `AudioGraph` API записывать звук.

### <a name="invoke-cognitive-services"></a>Когнитивные службы вызова неуправляемого кода

Пример приложения вызывает следующие Microsoft Cognitive Services:

- Microsoft Speech API. Дополнительные сведения см. в разделе [распознавание речи, используя API распознавания речи Microsoft](speech-recognition.md).
- API проверки орфографии Bing. Дополнительные сведения см. в разделе [проверка орфографии с помощью API проверки орфографии Bing](spell-check.md).
- Перевод API. Дополнительные сведения см. в разделе [перевод текста с помощью API перевода](text-translation.md).
- API распознавания лиц. Дополнительные сведения см. в разделе [распознавания эмоций, с помощью API распознавания лиц](emotion-recognition.md).

## <a name="related-links"></a>Связанные ссылки

- [Документация по Microsoft Cognitive Services](https://www.microsoft.com/cognitive-services/documentation)
- [Cognitive Services TODO (пример)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)