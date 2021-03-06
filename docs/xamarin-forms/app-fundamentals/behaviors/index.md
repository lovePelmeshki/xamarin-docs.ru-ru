---
title: Реакции на события Xamarin.Forms
description: Реакции на события позволяют добавлять функциональные возможности в элементы управления пользовательского интерфейса без разделения их на подклассы. Реакции на события пишутся в коде и добавляются в элементы управления в XAML или коде.
ms.prod: xamarin
ms.assetid: 42E32AD7-8E3B-48B3-B402-E75B758DA913
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d917d7d6421cfae7fc877c81023a835573fa99b1
ms.sourcegitcommit: a003b036f6fb83818e2ecc9c72a641e3aeb373bd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2020
ms.locfileid: "88964627"
---
# <a name="no-locxamarinforms-behaviors"></a>Реакции на события Xamarin.Forms

_Реакции на события позволяют добавлять функциональные возможности в элементы управления пользовательского интерфейса без разделения их на подклассы. Реакции на события пишутся в коде и добавляются в элементы управления в XAML или коде._

## <a name="introduction-to-behaviors"></a>[Введение в реакции на события](introduction.md)

Реакции на события позволяют реализовать код, который обычно пришлось бы писать как код программной части, так как он напрямую взаимодействует с API элемента управления таким образом, что его можно быстро подключить к элементу управления. В этой статье приводятся общие сведения о реакциях на события.

## <a name="attached-behaviors"></a>[Вложенные реакции на событие](attached.md)

Присоединенные реакции на события — это классы `static` с одним или несколькими присоединенными свойствами. В этой статье содержатся сведения о создании и использовании присоединенных реакций на события.

## <a name="no-locxamarinforms-behaviors"></a>[Реакции на события Xamarin.Forms](creating.md)

Реакции на событие Xamarin.Forms создаются путем наследования от классов [`Behavior`](xref:Xamarin.Forms.Behavior) или [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1). В этой статье содержатся сведения о создании и использовании реакций на события Xamarin.Forms.

## <a name="reusable-effectbehavior"></a>[Повторно используемая EffectBehavior](effect-behavior.md)

Реакции на события удобно использовать для добавления эффекта в элемент управления, удаления стереотипного эффекта, обработки кода из файлов кода программной части. В этой статье демонстрируется создание и использование реакции на событие Xamarin.Forms для добавления эффекта в элемент управления.
