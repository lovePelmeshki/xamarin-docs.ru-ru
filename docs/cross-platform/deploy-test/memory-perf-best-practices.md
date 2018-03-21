---
title: "Кроссплатформенная производительность"
description: "Существует множество методов повышения производительности приложений, созданных на платформе Xamarin. Вместе они могут значительно снизить загрузку ЦП и сократить объем памяти, используемой приложением. Эти методы описаны в данной статье."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9ce61f18-22ac-4b93-91be-5b499677d661
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/24/2017
ms.openlocfilehash: 287f564ba74050aa8a06e5a582ae8db6657e440e
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="cross-platform-performance"></a>Кроссплатформенная производительность

_Есть много способов повысить производительности приложений, созданных на платформе Xamarin. Вместе они могут значительно снизить загрузку ЦП и сократить объем памяти, используемой приложением. Все они описаны в этой статье._

Низкая производительность приложения проявляется по-разному. Из-за нее приложение может переставать отвечать на запросы, могут возникать задержки при прокрутке и сокращаться время работы батареи. Однако оптимизация производительности предусматривает не только правильную реализацию кода. Необходимо также учитывать эффективность работы пользователей. Например, чтобы повысить удобство работы, необходимо сделать так, чтобы выполнение операций не мешало пользователю выполнять другие действия.


<a name="profiler" />

## <a name="use-the-profiler"></a>Использование профилировщика

При разработке приложения пытаться оптимизировать код следует только после его профилирования. Профилирование — это способ определения того, какие именно оптимизации кода позволят сильнее всего сократить проблемы производительности. Профилировщик отслеживает использование памяти приложением и регистрирует время выполнения его методов. Эти данные помогают исследовать пути выполнения в приложении и затраты на выполнение кода, чтобы выявить наилучшие возможности для оптимизации.

Профилировщик Xamarin Profiler проводит оценку и помогает выявить проблемы с производительностью в приложении. С его помощью можно профилировать приложения Xamarin.iOS и Xamarin.Android из среды Visual Studio для Mac или Visual Studio. Дополнительные сведения о профилировщике Xamarin Profiler см. в статье [Введение в Xamarin Profiler](~/tools/profiler/index.md).

При профилировании приложения следуйте представленным ниже рекомендациям:

- Старайтесь не профилировать приложение в симуляторе, так как последний может давать искаженное представление о производительности приложения.
- В идеале профилирование следует проводить на различных устройствах, так как показатели производительности, полученные на одном устройстве, не всегда отражают аналогичные показатели на других устройствах. По крайней мере профилирование следует провести на устройстве с минимальными требуемыми характеристиками.
- Закройте все другие приложения, чтобы они не оказывали влияния на измеренные показатели профилируемого приложения.

<a name="idisposable" />

## <a name="release-idisposable-resources"></a>Освобождение ресурсов IDisposable

Интерфейс `IDisposable` предоставляет механизм для освобождения ресурсов. Он предоставляет метод `Dispose`, который следует реализовать для освобождения ресурсов явным образом. Интерфейс `IDisposable` не является деструктором, и его следует реализовывать только в указанных ниже ситуациях:

- Если класс является владельцем неуправляемых ресурсов. К типичным неуправляемым ресурсам, требующим освобождения, относятся файлы, потоки и сетевые подключения.
- Если класс является владельцем управляемых ресурсов `IDisposable`.

Потребители типа могут вызывать реализацию `IDisposable.Dispose` для освобождения ресурсов, если экземпляр больше не нужен. Для этого могут применяться два подхода:

- заключение объекта `IDisposable` в оператор `using`;
- заключение вызова метода `IDisposable.Dispose` в блок `try`/`finally`.

### <a name="wrapping-the-idisposable-object-in-a-using-statement"></a>Заключение объекта IDisposable в оператор using

В следующем примере кода показано заключение объекта `IDisposable` в оператор `using`:

```csharp
public void ReadText (string filename)
{
  ...
  string text;
  using (StreamReader reader = new StreamReader (filename)) {
    text = reader.ReadToEnd ();
  }
  ...
}
```

Класс `StreamReader` реализует интерфейс `IDisposable`, а оператор `using` предоставляет удобный синтаксис для вызова метода `StreamReader.Dispose` объекта `StreamReader`, прежде чем он окажется вне области действия. В блоке `using` объект `StreamReader` доступен только для чтения, и переназначить его нельзя. Кроме того, оператор `using` обеспечивает вызов метода `Dispose` даже в случае возникновения исключения, так как компилятор реализует промежуточный язык (IL) для блока `try`/`finally`.

### <a name="wrapping-the-call-to-idisposabledispose-in-a-tryfinally-block"></a>Заключение вызова метода IDisposable.Dispose в блок Try/Finally

В следующем примере кода показано заключение вызова метода `IDisposable.Dispose` в блок `try`/`finally`:

```csharp
public void ReadText (string filename)
{
  ...
  string text;
  StreamReader reader = null;
  try {
    reader = new StreamReader (filename);
    text = reader.ReadToEnd ();
  } finally {
    if (reader != null) {
      reader.Dispose ();
    }
  }
  ...
}
```

Класс `StreamReader` реализует интерфейс `IDisposable`, и блок `finally` вызывает метод `StreamReader.Dispose` для освобождения ресурса.

Дополнительные сведения см. в статье, посвященной [интерфейсу IDisposable](https://developer.xamarin.com/api/type/System.IDisposable/).

<a name="events" />

## <a name="unsubscribe-from-events"></a>Отмена подписки на события

Во избежание утечек памяти следует отменять подписку на события перед удалением объекта-подписчика. До тех пор пока подписка на событие не отменена, делегат события в публикующем объекте будет ссылаться на делегат, инкапсулирующий обработчик событий подписчика. Пока ссылка присутствует в публикующем объекте, память, занимаемая объектом-подписчиком, не будет освобождена при сборке мусора.

В следующем примере кода показано, как отменить подписку на событие:

```csharp
public class Publisher
{
  public event EventHandler MyEvent;

  public void OnMyEventFires ()
  {
    if (MyEvent != null) {
      MyEvent (this, EventArgs.Empty);
    }
  }
}

public class Subscriber : IDisposable
{
  readonly Publisher publisher;

  public Subscriber (Publisher publish)
  {
    publisher = publish;
    publisher.MyEvent += OnMyEventFires;
  }

  void OnMyEventFires (object sender, EventArgs e)
  {
    Debug.WriteLine ("The publisher notified the subscriber of an event");
  }

  public void Dispose ()
  {
    publisher.MyEvent -= OnMyEventFires;
  }
}
```

Класс `Subscriber` отменяет подписку на событие в методе `Dispose`.

При использовании обработчиков событий и синтаксиса лямбда-выражений могут также образовываться циклические ссылки, так как лямбда-выражения могут ссылаться на объекты и поддерживать их в активном состоянии. Поэтому ссылку на анонимный метод можно сохранить в поле и использовать для отмены подписки на событие, как показано в следующем примере кода:

```csharp
public class Subscriber : IDisposable
{
  readonly Publisher publisher;
  EventHandler handler;

  public Subscriber (Publisher publish)
  {
    publisher = publish;
    handler = (sender, e) => {
      Debug.WriteLine ("The publisher notified the subscriber of an event");
    };
    publisher.MyEvent += handler;
  }

  public void Dispose ()
  {
    publisher.MyEvent -= handler;
  }
}
```

Поле `handler` содержит ссылку на анонимный метод и служит для подписки на события и ее отмены.

<a name="weakreferences" />

## <a name="use-weak-references-to-prevent-immortal-objects"></a>Использование слабых ссылок для предотвращения возникновения неуничтожимых объектов

> [!NOTE]
> Чтобы обеспечить эффективное использование памяти приложениями для iOS, их разработчикам следует ознакомиться с документацией, в которой описывается, как [избежать циклических ссылок в iOS](~/ios/deploy-test/performance.md#avoidcircularreferences).

<a name="lazy" />

## <a name="delay-the-cost-of-creating-objects"></a>Отложенное создание объектов

С помощью отложенной инициализации можно отложить создание объекта до момента его первого использования. Этот прием в основном используется, чтобы повысить быстродействие, избежать лишних вычислений и уменьшить требования к памяти.


Применять отложенную инициализацию объектов, создание которых является ресурсоемким, рекомендуется в двух указанных ниже случаях:

- Объект может не использоваться в приложении.
- Перед созданием объекта должны быть выполнены другие ресурсоемкие операции.

Класс `Lazy<T>` используется для определения типа с отложенной инициализацией, как показано в следующем примере кода:

```csharp
void ProcessData(bool dataRequired = false)
{
  Lazy<double> data = new Lazy<double>(() =>
  {
    return ParallelEnumerable.Range(0, 1000)
                 .Select(d => Compute(d))
                 .Aggregate((x, y) => x + y);
  });

  if (dataRequired)
  {
    if (data.Value > 90)
    {
      ...
    }
  }
}

double Compute(double x)
{
  ...
}
```

Отложенная инициализация производится при первом обращении к свойству `Lazy<T>.Value`. При первом доступе заключенный в оболочку тип создается, возвращается и сохраняется для использования в будущем.

Дополнительные сведения об отложенной инициализации см. в статье [Отложенная инициализация](https://msdn.microsoft.com/en-us/library/dd997286(v=vs.110).aspx).

<a name="async" />

## <a name="implement-asynchronous-operations"></a>Реализация асинхронных операций

Платформа .NET предоставляет асинхронные версии многих интерфейсов API. В отличие от синхронных интерфейсов API, асинхронные интерфейсы API гарантируют, что активный поток выполнения никогда не блокирует вызывающий поток на длительный период времени. Поэтому при вызове интерфейса API из потока пользовательского интерфейса по возможности используйте асинхронный интерфейс API. Это позволит избежать блокирования потока пользовательского интерфейса, что повысит удобство работы пользователя с приложением.

Кроме того, чтобы избежать блокирования потока пользовательского интерфейса, длительные операции следует выполнять в фоновом потоке. Платформа .NET предоставляет ключевые слова `async` и `await`, которые позволяют создавать асинхронный код для выполнения длительных операций в фоновом потоке и доступа к результатам по завершении. Хотя длительные операции могут выполняться асинхронно с помощью ключевого слова `await`, оно не гарантирует выполнения операции в фоновом потоке. Чтобы обеспечить такое выполнение, можно передать длительную операцию в метод `Task.Run`, как показано в следующем примере кода:

```csharp
public class FaceDetection
{
  ...
  async void RecognizeFaceButtonClick(object sender, EventArgs e)
  {
    await Task.Run(() => RecognizeFace ());
    ...
  }

  async Task RecognizeFace()
  {
    ...
  }
}
```

Метод `RecognizeFace` выполняется в фоновом потоке, причем прежде чем продолжить выполнение, метод `RecognizeFaceButtonClick` ожидает завершения метода `RecognizeFace`.

Длительные операции также должны поддерживать возможность отмены. Например, возможна ситуация, когда продолжать выполнять длительную операцию больше не нужно, если пользователь перешел к другому элементу в приложении. Возможность отмены реализуется по указанной ниже схеме:

- Создайте экземпляр `CancellationTokenSource`. Он будет управлять отменой и отправлять уведомления о ней.
- Передайте значение свойства `CancellationTokenSource.Token` в каждую задачу, которая должна поддерживать отмену.
- Для каждой задачи предоставьте механизм реагирования на отмену.
- Вызовите метод `CancellationTokenSource.Cancel` для предоставления уведомления об отмене.

> [!IMPORTANT]
> Класс `CancellationTokenSource` реализует интерфейс `IDisposable`, поэтому после завершения выполнения экземпляра `CancellationTokenSource.Dispose` должен быть вызван метод `CancellationTokenSource`.



Дополнительные сведения см. в статье [Обзор поддержки асинхронного выполнения](~/cross-platform/platform/async.md).

<a name="sgen" />

## <a name="use-the-sgen-garbage-collector"></a>Использование сборщика мусора SGen

Управляемые языки, такие как C#, используют сборку мусора для освобождения памяти, выделенной объектам, которые больше не нужны. В платформе Xamarin применяются два сборщика мусора:

- [**SGen**](http://www.mono-project.com/docs/advanced/garbage-collector/sgen/) — это сборщик мусора, учитывающий поколения объектов; в платформе Xamarin он используется по умолчанию.
- [**Boehm**](http://www.hboehm.info/gc/) — это традиционный сборщик мусора, не учитывающий поколения объектов. Это сборщик мусора по умолчанию для приложений Xamarin.iOS на основе Classic API.

SGen использует одну из трех куч для выделения пространства для объектов:

-  **Инкубатор** — здесь выделяется пространство для новых небольших объектов. Если место в инкубаторе заканчивается, производится слабая сборка мусора. Все активные объекты перемещаются в основную кучу.
-  **Основная куча** — здесь находятся объекты с длительным сроком использования. Если памяти в основной куче недостаточно, производится интенсивная сборка мусора. Если в результате интенсивной сборки мусора не удалось освободить достаточно памяти, сборщик SGen запросит дополнительную память у системы.
-  **Пространство больших объектов** — здесь находятся объекты, для хранения которых требуется более 8000 байт. Большие объекты не попадают изначально в инкубатор, а сразу размещаются в этой куче.

Одно из преимуществ сборщика SGen заключается в том, что время, затрачиваемое на слабую сборку мусора, пропорционально количеству новых активных объектов, созданных с момента последней слабой сборки мусора. Благодаря этому снижается влияние сборки мусора на производительность приложения, так как слабые сборки мусора занимают меньше времени, чем интенсивная. Интенсивная сборка мусора по-прежнему производится, но не так часто.

### <a name="reducing-pressure-on-the-garbage-collector"></a>Снижение нагрузки на сборщик мусора

Когда сборщик SGen начинает сборку мусора, потоки приложения останавливаются на то время, пока освобождается память. Во время освобождения памяти в пользовательском интерфейсе могут возникать небольшие паузы или задержки. То, насколько они ощутимы, зависит от двух факторов:

1. **Частота** — как часто происходит сборка мусора. Частота сборки мусора тем выше, чем больше памяти выделяется в промежутке между операциями сборки.
1. **Длительность** — сколько времени занимает отдельная сборка мусора. Она примерно пропорциональна количеству собираемых активных объектов.

В целом это означает, что при выделении памяти для большого количества объектов, которые не остаются активными, сборка мусора осуществляется часто, но каждый раз проводится быстро. И наоборот, если память для новых объектов выделяется редко, но они остаются активными, сборка мусора производится реже, но длится дольше.

Чтобы снизить нагрузку на сборщик мусора, следуйте приведенным ниже правилам:

- Избегайте сборки мусора короткими циклами с помощью пулов объектов. Это особенно важно в случае с играми, в которых большинство объектов должно создаваться заранее.
- Явным образом освобождайте ресурсы, такие как потоки, сетевые подключения, большие блоки памяти и файлы, если они больше не нужны. Дополнительные сведения см. в разделе [Освобождение ресурсов IDisposable](#idisposable).
- Отменяйте регистрацию обработчиков событий, если они больше не нужны, чтобы объекты можно было собирать. Дополнительные сведения см. в разделе [Отмена подписки на события](#events).

<a name="linker" />

## <a name="reduce-the-size-of-the-application"></a>Уменьшение размера приложения

Чтобы понимать, как образуется размер исполняемого файла приложения, необходимо разбираться в процессе компиляции на каждой платформе:

- Приложения iOS компилируются в режиме Ahead Of Time (AOT) на языке сборок ARM. Платформа .NET включается, причем неиспользуемые классы исключаются только в том случае, если включен соответствующий параметр компоновщика.
- Приложения Android компилируются на промежуточном языке (IL) и упаковываются с помощью MonoVM и JIT-компиляции. Неиспользуемые классы платформы исключаются только в том случае, если включен соответствующий параметр компоновщика.
- Приложения Windows Phone компилируются на языке IL и выполняются встроенной средой выполнения.

Кроме того, если в приложении широко применяются универсальные шаблоны, итоговый размер исполняемого файла будет еще больше, так как этот файл будет содержать скомпилированные в машинный код версии универсальных возможностей.

Для сокращения размера приложений в состав средств сборки платформы Xamarin включен компоновщик. По умолчанию компоновщик отключен, и его необходимо включить в параметрах проекта для приложения. Во время сборки он производит статический анализ приложения, чтобы определить, какие типы и члены действительно используются приложением. После этого он удаляет ненужные типы и методы из приложения.

На приведенном ниже снимке экрана показаны параметры компоновщика для проекта Xamarin.iOS в Visual Studio для Mac:

![](memory-perf-best-practices-images/linker-options-ios.png)

На приведенном ниже снимке экрана показаны параметры компоновщика для проекта Xamarin.Android в Visual Studio для Mac:

![](memory-perf-best-practices-images/linker-options-droid.png)

Для управления работой компоновщика служат три параметра:

-  **Не компоновать**. Компоновщик не удаляет ненужные типы и методы. В целях повышения производительности этот параметр используется по умолчанию для отладочных сборок.
-  **Компоновать пакеты SDK платформы/Только сборки пакета SDK**. При выборе этого параметра будет уменьшаться размер только тех сборок, которые предоставляются Xamarin. Пользовательский код не затрагивается.
-  **Компоновать все сборки**. Это более интенсивный способ оптимизации, который охватывает как сборки SDK, так и пользовательский код. В случае со сборками удаляются неиспользуемые резервные поля, и каждый экземпляр (или привязанный объект) становится компактнее, занимая меньше памяти.

Параметр *Компоновать все сборки* следует использовать с осторожностью, так как он может нарушить работу приложения непредвиденным образом. При проведении компоновщиком статического анализа может неправильно определяться необходимый код, из-за чего из скомпилированного приложения удаляется слишком много кода. Эта проблема проявляется только во время выполнения и приводит к аварийному завершению работы приложения. Поэтому после изменения режима работы компоновщика важно тщательно протестировать приложение.

Если при тестировании выявляется, что компоновщик удалил нужный класс или метод, можно пометить типы или методы, на которые нет статических ссылок, но которые требуются приложению, с помощью одного из следующих атрибутов:

-  `Xamarin.iOS.Foundation.PreserveAttribute` — этот атрибут предназначен для проектов Xamarin.iOS;
-  `Android.Runtime.PreserveAttribute` — этот атрибут предназначен для проектов Xamarin.Android.

Например, может быть необходимо сохранить конструкторы по умолчанию для типов, экземпляры которых создаются динамически. Кроме того, может потребоваться сохранить свойства типов для сериализации XML.

Дополнительные сведения см. в статьях, посвященных [компоновщику для iOS](~/ios/deploy-test/linker.md) и [компоновщику для Android](~/android/deploy-test/linker.md).

### <a name="additional-size-reduction-techniques"></a>Дополнительные методы уменьшения размера

Мобильные устройства оснащаются ЦП различной архитектуры. Поэтому Xamarin.iOS и Xamarin.Android создают *толстые двоичные файлы*, которые содержат скомпилированную версию приложения для каждой архитектуры ЦП. Благодаря этому приложение может выполняться на любом устройстве независимо от его архитектуры ЦП.

Чтобы еще больше сократить размер исполняемого файла приложения, можно выполнить указанные ниже действия:

- Создайте сборку выпуска.
- Уменьшите число архитектур, для которых предназначено приложение, чтобы избежать создания толстого двоичного файла.
- Чтобы создать более оптимизированный исполняемый файл, используйте компилятор LLVM.
- Сократите размер управляемого кода в приложении. Этого можно достичь, включив компоновщик для каждой сборки (*Компоновать все* для проектов iOS или *Компоновать все сборки* для проектов Android).

Приложение Android можно также разделить на отдельные пакеты APK для каждого набора ABI ("архитектуры").
Дополнительные сведения см. в записи блога [How To Keep Your Android App Size Down](http://motzcod.es/post/112072508362/how-to-keep-your-android-app-size-down) (Как ограничить размер приложения Android).

<a name="optimizeimages" />

## <a name="optimize-image-resources"></a>Оптимизация графических ресурсов

Изображения — это один из самых ресурсоемких ресурсов в приложениях. Они часто хранятся в высоком разрешении. Хотя это позволяет сделать приложение более визуально насыщенным и детализированным, в связи с необходимостью декодировать изображения возрастает загрузка ЦП, а для хранения декодированных изображений требуется больше памяти. Декодировать изображение в высоком разрешении в памяти, если затем при отображении его размер будет уменьшен, бессмысленно. Вы можете сократить загрузку ЦП и занимаемую память, сохранив несколько версий изображений в разных разрешениях, приблизительно соответствующих ожидаемым размерам экрана. Например, изображение, отображаемое в представлении списка, скорее всего, будет иметь меньшее разрешение, чем изображение, отображаемое на весь экран. Кроме того, для эффективного отображения изображений с минимальными расходами памяти можно загружать версии изображений высокого разрешения в уменьшенном масштабе. Дополнительные сведения см. в разделе [Эффективная загрузка больших растровых изображений](https://developer.xamarin.com/recipes/android/resources/general/load_large_bitmaps_efficiently/).

Независимо от разрешения изображения отображение графических ресурсов может значительно увеличивать объем памяти, занимаемой приложением. Поэтому их следует создавать только при необходимости и сразу же освобождать, если они больше не нужны приложению.

<a name="activationperiod" />

## <a name="reduce-the-application-activation-period"></a>Сокращение периода активации приложения

У всех приложений есть *период активации*, то есть время от запуска приложения до момента готовности его к использованию. От периода активации зависит первое впечатление пользователей о приложении. Чтобы это впечатление было благоприятным, период активации следует свести к минимуму и сделать его приятным для пользователей.

До того как появится начальный пользовательский интерфейс приложения, должен выводиться экран-заставка, сообщающий пользователю о запуске приложения. Если на отображение начального пользовательского интерфейса требуется время, экран-заставка должен сообщать о ходе активации, чтобы пользователь не решил, что приложение перестало отвечать на запросы. Это можно реализовать в виде индикатора выполнения или аналогичного элемента управления.

В течение периода активации приложение выполняет логику активации, которая часто включает в себя загрузку и обработку ресурсов. Период активации можно сократить, упаковав необходимые ресурсы в приложение, а не извлекая их из удаленного источника. Например, в некоторых ситуациях во время периода активации может быть рациональнее загружать локально хранящиеся подстановочные данные. Затем, после того как отобразится начальный пользовательский интерфейс и пользователь сможет начать работать с приложением, подстановочные данные можно постепенно заменять данными из удаленного источника. Кроме того, логика активации приложения должна выполнять только те задачи, которые необходимы для того, чтобы пользователь мог приступить к работе с приложением. Это может помочь в случае удаленной загрузки дополнительных сборок, когда сборки загружаются при первом использовании.

<a name="webservicecommunication" />

## <a name="reduce-web-service-communication"></a>Сокращение взаимодействия с веб-службой

Подключение к веб-службе из приложения может влиять на его производительность. Например, более интенсивное использование пропускной способности сети приводит к сокращению срока работы устройства от аккумулятора. Кроме того, приложение может использоваться в среде с ограниченной пропускной способностью. Поэтому имеет смысл сократить использование пропускной способности между приложением и веб-службой.

Одним из подходов к сокращению используемой пропускной способности является сжатие данных перед их передачей по сети. Однако повышение загрузки ЦП в связи со сжатием может также приводить к ускоренному разряду батареи. Поэтому прежде чем решать, следует ли передавать по сети сжатые данные, необходимо тщательно оценить возможные выгоды и потери.

Еще один момент, который следует принять во внимание, — это формат данных, передаваемых между приложением и веб-службой. Два основных формата — это XML и JSON. XML — это формат обмена текстовыми данными, доля полезных данных в котором сравнительно мала, так как он включает в себя большое количество символов форматирования. JSON — это формат обмена текстовыми данными, доля полезных данных в котором высока, что сокращает требования к пропускной способности при отправке и получении данных. Поэтому для мобильных приложений формат JSON часто предпочтительнее.

При передаче данных между приложением и веб-службой рекомендуется использовать объекты передачи данных (DTO). Объект передачи данных содержит набор данных, подлежащих передаче по сети. С помощью объектов передачи данных можно передавать больше данных за один удаленный вызов, что позволяет уменьшить количество удаленных вызовов, совершаемых приложением. Как правило, на выполнение удаленного вызова с большим объемом полезных данных требуется столько же времени, сколько и на вызов с небольшим объемом полезных данных.

Данные, полученные из веб-службы, следует кэшировать локально, чтобы вместо повторного извлечения данных из веб-службы использовались данные из кэша. Однако при выборе такого подхода следует также реализовать подходящую стратегию кэширования для обновления данных в локальном кэше при их изменении в веб-службе.

## <a name="summary"></a>Сводка

В этой статье были рассмотрены методы повышения производительности приложений, созданных на платформе Xamarin. Вместе они могут значительно снизить загрузку ЦП и сократить объем памяти, используемой приложением.

## <a name="related-links"></a>Связанные ссылки

- [Производительность Xamarin.iOS](~/ios/deploy-test/performance.md)
- [Производительность Xamarin.Android](~/android/deploy-test/performance.md)
- [Общие сведения о Xamarin Profiler](~/tools/profiler/index.md)
- [Производительность Xamarin.Forms](~/xamarin-forms/deploy-test/performance.md)
- [Общие сведения о поддержке асинхронного выполнения](~/cross-platform/platform/async.md)
- [IDisposable](https://developer.xamarin.com/api/type/System.IDisposable/)
- [Предотвращение распространенных проблем в приложениях Xamarin (видео)](https://university.xamarin.com/guestlectures/avoiding-common-pitfalls-in-xamarin-apps)