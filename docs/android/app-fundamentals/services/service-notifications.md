---
title: Уведомления службы
description: В этом руководство объясняется, как служба Android может использовать локальные уведомления для отправки информации пользователю.
ms.prod: xamarin
ms.assetid: 6C06FDE7-6385-40EF-AC7C-8EFB54E29F45
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: b02785863f89ef6a273c52c09f45a99c17cb6242
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73024537"
---
# <a name="service-notifications"></a>Уведомления службы

_В этом руководство объясняется, как служба Android может использовать локальные уведомления для отправки информации пользователю._

## <a name="service-notifications-overview"></a>Общие сведения об уведомлениях службы

Уведомления службы позволяют приложению отображать сведения пользователю, даже если приложение Android не находится на переднем плане. Уведомление может предоставить пользователю действия, например отображение действия из приложения. В следующем примере кода показано, как служба может отправлять уведомления пользователю.

```csharp
[Service]
public class MyService: Service 
{
    // A notification requires an id that is unique to the application.
    const int NOTIFICATION_ID = 9000;
    
    public override StartCommandResult OnStartCommand(Intent intent, StartCommandFlags flags, int startId)
    {
        // Code omitted for clarity - here is where the service would do something.
    
        // Work has finished, now dispatch anotification to let the user know.
        Notification.Builder notificationBuilder = new Notification.Builder(this)
            .SetSmallIcon(Resource.Drawable.ic_notification_small_icon)
            .SetContentTitle(Resources.GetString(Resource.String.notification_content_title))
            .SetContentText(Resources.GetString(Resource.String.notification_content_text));
        
        var notificationManager = (NotificationManager)GetSystemService(NotificationService);
        notificationManager.Notify(NOTIFICATION_ID, notificationBuilder.Build());
    }
}
```

На этом снимке экрана показан пример отображаемого уведомления:

[значок уведомления![, отображаемый в строке состояния](service-notifications-images/01-notification-sml.png)](service-notifications-images/01-notification.png#lightbox)

Когда пользователь выводит на экран уведомление в верхней части экрана, отображается полное уведомление:

![Оповещение, отображаемое в области уведомлений](service-notifications-images/02-fullnotification.png)

## <a name="updating-a-notification"></a>Обновление уведомления

Чтобы обновить уведомление, служба повторно опубликует уведомление, используя тот же идентификатор уведомления. При необходимости Android отобразит или обновит уведомление в строке состояния.

```csharp 
void UpdateNotification(string content)
{
   var notification = GetNotification(content, pendingIntent);

   NotificationManager notificationManager = (NotificationManager)GetSystemService(Context.NotificationService);
   notificationManager.Notify(NOTIFICATION_ID, notification);
}

Notification GetNotification(string content, PendingIntent intent)
{
   return new Notification.Builder(this)
           .SetContentTitle(tag)
           .SetContentText(content)
           .SetSmallIcon(Resource.Drawable.NotifyLg)
           .SetLargeIcon(BitmapFactory.DecodeResource(Resources, Resource.Drawable.Icon))
           .SetContentIntent(intent).Build();
}
```

Дополнительные сведения об уведомлениях см. в разделе [локальные уведомления](~/android/app-fundamentals/notifications/local-notifications.md) в статье [об уведомлениях Android](~/android/app-fundamentals/notifications/index.md) .

## <a name="related-links"></a>Связанные ссылки

- [Локальные уведомления в Android](~/android/app-fundamentals/notifications/local-notifications.md)
