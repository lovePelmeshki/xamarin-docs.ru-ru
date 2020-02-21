---
title: Горячий перезапуск Xamarin
description: Этот документ описывает настройку и использование горячего перезапуска Xamarin для отладки приложения iOS.
ms.prod: xamarin
ms.assetid: 6BC62A88-9368-41BB-8494-760F2A4805DB
ms.technology: xamarin-forms
author: jimmgarrido
ms.author: jigarrid
ms.date: 01/14/2020
ms.openlocfilehash: 2cf925a96e952e6b760da9ca5416e124a3e3716b
ms.sourcegitcommit: ccbf914615c0ce6b3f308d930f7a77418aeb4dbc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/07/2020
ms.locfileid: "77071157"
---
# <a name="xamarin-hot-restart-preview"></a>Горячий перезапуск Xamarin (предварительная версия)

![Предварительная версия](~/media/shared/preview.png)

Горячий перезапуск Xamarin позволяет быстро проверить изменения в приложении во время разработки, включая изменение кода, ресурсов и ссылок в нескольких файлах. Он передает новые изменения в существующий пакет приложений в целевом объекте отладки, что значительно ускоряет цикл сборки и развертывания.

> [!NOTE]
> Сейчас горячий перезапуск Xamarin доступен в Visual Studio 2019 версии 16.5 (предварительная версия) и поддерживает приложения iOS с помощью Xamarin.Forms. Поддержка Visual Studio для Mac и приложений, отличных от Xamarin.Forms, включена в дорожную карту.

## <a name="requirements"></a>Требования

- Visual Studio 2019 версии 16.5 (предварительная версия 2) или более поздней версии
- iTunes (64-разрядная версия)
- Учетная запись разработчика Apple


## <a name="initial-setup"></a>Начальная настройка

1. Убедитесь, что проект iOS задан в качестве запускаемого проекта, а для конфигурации сборки задано значение **Отладка|iPhone**.

   1. Если это существующий проект, перейдите в раздел **Сборка > Диспетчер конфигурации...** и убедитесь, что для проекта iOS включен параметр **Развертывание**.

2. Выберите и щелкните элемент **Локальное устройство** на панели инструментов, чтобы запустить мастер установки:

    [![](hot-restart-images/toolbar.png "Screenshot of the Visual Studio toolbar with local device set as the debug target.")](hot-restart-images/toolbar.png)

3. Если компонент iTunes не установлен, щелкните **Скачать iTunes**, чтобы скачать установщик. После завершения установки iTunes нажмите кнопку **Далее**.

4. Подключите устройство iOS к компьютеру. После обнаружения устройства его имя появится в мастере. Нажмите кнопку **Далее**.

5. Введите учетные данные разработчика Apple и нажмите кнопку **Далее**.

6. Выберите команду разработчик в раскрывающемся меню, чтобы включить [автоматическую подготовку](~/ios/get-started/installation/device-provisioning/automatic-provisioning.md) в проекте. Нажмите кнопку **Готово**.

> [!NOTE]
> Рекомендуется использовать автоматическую подготовку, чтобы можно было легко настроить дополнительные устройства iOS для развертывания. Однако вы можете отключить ее и продолжить использование ручной подготовки при наличии подходящих профилей подготовки.

## <a name="use-xamarin-hot-restart"></a>Использование горячего перезапуска Xamarin
После первоначальной настройки подключенное устройство появится в раскрывающемся меню целевого объекта отладки. Чтобы выполнить отладку приложения, выберите устройство в раскрывающемся списке и нажмите кнопку **Выполнить**. Visual Studio может отобразить сообщение о необходимости запустить приложение на устройстве вручную, чтобы начать сеанс отладки.

Вы можете внести изменения в файлы кода во время отладки, а затем нажмите кнопку **Перезапустить** на панели инструментов отладки или используйте сочетание клавиш **CTRL+ SHIFT+F5** для перезапуска сеанса отладки с применением новых изменений:

[![](hot-restart-images/restart.png "Screenshot of the debug toolbar with the restart button highlighted.")](hot-restart-images/toolbar.png)

## <a name="limitations"></a>Ограничения
- Сейчас поддерживаются только приложения iOS, созданные с использованием Xamarin.Forms и устройств iOS.
- Файлы раскадровки и XIB не поддерживаются, и приложение может аварийно завершить работу, если попытается загрузить их во время выполнения. Если вы используете их в своем приложении, сообщите нам, так как мы заинтересованы в поддержке такого сценария в будущем.
- Статические библиотеки и платформы iOS не поддерживаются. Если приложение попытается загрузить их, могут возникнуть ошибки времени выполнения или сбои. Однако поддерживаются динамические библиотеки iOS.
- Горячий перезапуск Xamarin запрещено использовать в целях создания пакетов приложений для публикации. Вам по-прежнему потребуется компьютер Mac для полной компиляции, подписывания и развертывания приложения в рабочей среде.

## <a name="troubleshoot"></a>Устранение неполадок
- Мастер установки не будет обнаруживать компонент iTunes, если он был установлен с помощью Microsoft Store. Сначала потребуется удалить эту версию, а затем скачать [установщик с сайта Apple](https://go.microsoft.com/fwlink/?linkid=2101014).
- Существует известная проблема, когда при включении сборок для конкретных устройств сеанс отладки приложения не запускается. В качестве обходного решения отключите эту возможность в разделе **Свойства > Сборка iOS** и повторите запуск отладки. Эта проблема будет исправлена в будущем выпуске.
- Если это приложение уже установлено на устройстве, попытка развертывания с использованием горячего перезапуска может завершиться ошибкой `AMDeviceStartHouseArrestService`. Чтобы обойти эту проблему, удалите приложение на устройстве и снова разверните его.

Чтобы сообщить о дополнительных проблемах, используйте средство обратной связи в разделе [Справка > Обратная связь > Сообщить о проблеме](/visualstudio/ide/feedback-options?view=vs-2019#report-a-problem).