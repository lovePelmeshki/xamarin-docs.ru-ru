---
title: SceneKit в Xamarin. iOS
description: В этом документе описывается SceneKit, API-интерфейс графа трехмерной сцены, который упрощает работу с трехмерной графикой за счет абстракции сложностей OpenGL.
ms.prod: xamarin
ms.assetid: 19049ED5-B68E-4A0E-9D57-B7FAE3BB8987
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/14/2017
ms.openlocfilehash: d5aa7eb239b74437699aedb9699fefc862a3d345
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86932396"
---
# <a name="scenekit-in-xamarinios"></a>SceneKit в Xamarin. iOS

SceneKit — это API графа трехмерной сцены, который упрощает работу с трехмерной графикой. Он впервые появился в OS X 10,8 и теперь поставляется с iOS 8. SceneKit создание впечатляющих трехмерных визуализаций и необычных трехмерных игр не требует опыта в OpenGL. Основываясь на общих концепциях графов сцены, SceneKit абстрагирует сложности OpenGL и OpenGL ES, что упрощает добавление трехмерного содержимого в приложение. Однако, если вы являетесь экспертом по OpenGL, SceneKit обладает отличной поддержкой привязки непосредственно с OpenGL. Он также включает множество функций, дополняющих трехмерную графику, например "физика", и хорошо интегрируется с несколькими другими платформами Apple, такими как основная анимация, основной образ и набор спрайтов.

SceneKit очень прост в работе с. Это декларативный API, который отвечает за отрисовку. Вы просто настраиваете сцену, добавляете в нее свойства, а SceneKit обрабатывает отрисовку сцены.

Для работы с SceneKit можно создать граф сцены с помощью `SCNScene` класса. Сцена содержит иерархию узлов, представленную экземплярами `SCNNode` , определяющую расположения в трехмерном пространстве. Каждый узел имеет такие свойства, как геометрия, освещение и материалы, влияющие на его внешний вид, как показано на следующем рисунке:

![Иерархия SceneKit](scenekit-images/image7.png)

## <a name="create-a-scene"></a>Создание сцены

Чтобы создать сцену на экране, добавьте ее в объект, `SCNView` назначив его свойству сцены представления. Кроме того, если вы вносите изменения в сцену, `SCNView` обновит саму себя, чтобы отобразить изменения.

```csharp
scene = SCNScene.Create ();
sceneView = new SCNView (View.Frame);
sceneView.Scene = scene;
```

Сцены можно заполнять из файлов, экспортированных с помощью инструмента трехмерного моделирования, или программными средствами из геометрических примитивов. Например, вот как можно создать сферу и добавить ее в сцену:

```csharp
sphere = SCNSphere.Create (10.0f);
sphereNode = SCNNode.FromGeometry (sphere);
sphereNode.Position = new SCNVector3 (0, 0, 0);
scene.RootNode.AddChildNode (sphereNode);
```

## <a name="adding-light"></a>Добавление освещения

На этом этапе сфера не будет отображать ничего, так как в сцене нет освещения. Присоединение `SCNLight` экземпляров к узлам создает индикаторы в SceneKit. Существует несколько типов источников света от различных форм направленного освещения до окружающего освещения. Например, следующий код создает всенаправленныеый источник на стороне сферы.

```csharp
// omnidirectional light
var light = SCNLight.Create ();
var lightNode = SCNNode.Create ();
light.LightType = SCNLightType.Omni;
light.Color = UIColor.Blue;
lightNode.Light = light;
lightNode.Position = new SCNVector3 (-40, 40, 60);
scene.RootNode.AddChildNode (lightNode);
```

Всенаправленные освещение создает Рассеянное отражение, что приводит к равномерному освещению, например к освещению фонарик. Создание внешнего освещения похоже на то, что не имеет направления, так как оно намерено намерено в любом направлении. Подумайте о том, как освещение:)

```csharp
// ambient light
ambientLight = SCNLight.Create ();
ambientLightNode = SCNNode.Create ();
ambientLight.LightType = SCNLightType.Ambient;
ambientLight.Color = UIColor.Purple;
ambientLightNode.Light = ambientLight;
scene.RootNode.AddChildNode (ambientLightNode);
```

После того, как индикаторы на месте, сфера видна в сцене.

![Сфера видна в сцене при освещении](scenekit-images/image8.png)

## <a name="adding-a-camera"></a>Добавление камеры

Добавление камеры (Скнкамера) в сцену изменяет точку зрения. Шаблон для добавления камеры аналогичен. Создайте камеру, прикрепите ее к узлу и добавьте узел в сцену.

```csharp
// camera
camera = new SCNCamera {
    XFov = 80,
    YFov = 80
};
cameraNode = new SCNNode {
    Camera = camera,
    Position = new SCNVector3 (0, 0, 40)
};
scene.RootNode.AddChildNode (cameraNode);
```

Как видно из приведенного выше кода, объекты SceneKit можно создавать с помощью конструкторов или метода Create Factory. Первый из них допускает использование синтаксиса инициализатора C#, но его использование в основном зависит от предпочтений.

При наличии камеры вся сфера видна пользователю:

![Вся сфера видна пользователю.](scenekit-images/image9.png)

К сцене можно также добавить дополнительные индикаторы. Вот как это выглядит с помощью нескольких источников света всенаправленные:

![Сфера с еще несколькими всенаправленные светами](scenekit-images/image10.png)

Кроме того, с помощью параметра `sceneView.AllowsCameraControl = true` пользователь может изменить точку зрения с помощью сенсорного жеста.

### <a name="materials"></a>Материалы

Материалы создаются с помощью класса Скнматериал. Например, чтобы добавить изображение на поверхность сферы, присвойте образу *рассеянное* содержимое материала.

```csharp
material = SCNMaterial.Create ();
material.Diffuse.Contents = UIImage.FromFile ("monkey.png");
sphere.Materials = new SCNMaterial[] { material };
```

На этом рисунке изображение размещается на узле, как показано ниже:

![Размещение изображения на поверхности](scenekit-images/image11.png)

Материал может быть настроен на реагирование на другие типы освещения. Например, объект можно сделать блестящим, чтобы его отражающее содержимое было настроено для отображения отраженного отражения, что приводит к яркому расположению на поверхности, как показано ниже:

![Объект, посвященный отраженному отражению, что приводит к яркому расположению на поверхности](scenekit-images/image12.png)

Материалы являются очень гибкими, что позволяет достичь большого объема кода. Например, вместо задания изображения на рассеянное содержимое задайте вместо него изображение.

```csharp
material.Reflective.Contents = UIImage.FromFile ("monkey.png");
```

Теперь обезьяна выглядит визуально в пределах сферы, независимо от точки зрения.

### <a name="animation"></a>Анимация

SceneKit предназначен для корректной работы с анимацией. Можно создать как явную, так и неявную анимацию, а также даже визуализировать сцену из основного дерева слоя анимации. При создании неявной анимации SceneKit предоставляет собственный класс перехода, `SCNTransaction` .

Ниже приведен пример, поворачивающий сферу:

```csharp
SCNTransaction.Begin ();
SCNTransaction.AnimationDuration = 2.0;
sphereNode.Rotation = new SCNVector4 (0, 1, 0, (float)Math.PI * 4);
SCNTransaction.Commit ();
```

Однако можно анимировать гораздо больше, чем поворот. Многие свойства SceneKit являются анимированными. Например, следующий код анимирует материал `Shininess` для увеличения отраженного отражения.

```csharp
SCNTransaction.Begin ();
SCNTransaction.AnimationDuration = 2.0;
material.Shininess = 0.1f;
SCNTransaction.Commit ();
```

SceneKit очень прост в использовании. Она предлагает множество дополнительных функций, включая ограничения, физику, декларативные действия, трехмерный текст, глубину поддержки полей, интеграцию комплекта спрайтов и интеграцию образов с основными компонентами.
