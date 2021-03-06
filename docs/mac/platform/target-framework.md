---
title: Целевая платформа для Xamarin. Mac
description: В этой статье рассматриваются целевые платформы (библиотеки базовых классов), доступные для Xamarin. Mac, и последствия их использования в проекте Xamarin. Mac.
ms.prod: xamarin
ms.assetid: AF21BE16-3F92-4121-AB4C-D51AC863D92D
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 11/10/2017
ms.openlocfilehash: e07ec4fd4436d951ea4033dbceab2cef47e96218
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73025934"
---
# <a name="target-framework-for-xamarinmac"></a>Целевая платформа для Xamarin. Mac

_В этой статье рассматриваются целевые платформы (библиотеки базовых классов), доступные для Xamarin. Mac, и последствия их использования в проекте Xamarin. Mac._

![Параметры целевой платформы для Xamarin. Mac](target-framework-images/select-target.png "Параметры целевой платформы для Xamarin. Mac")

## <a name="background"></a>Фон

Каждая программа или библиотека .NET зависит от функциональных возможностей, предоставляемых библиотекой базовых классов (BCL). В эту библиотеку BCL входят такие сборки, как mscorlib, System, System .NET. http и System. XML, которые предоставляют общие функциональные возможности, встроенные во все языки .NET.

В течение многих лет было разработано несколько различных версий этой библиотеки BCL, оптимизированных для различных вариантов использования. Класс BCL "Desktop" включает более широкий набор библиотек, которые могут быть слишком высокой плотностью для других вариантов использования, в то время как мобильные устройства сосредоточены на обеспечении безопасного связывания интерфейсов API, что удаляет неиспользуемый код для уменьшения объема приложений.

Одним из наиболее важных последствий этих различных целевых платформ является то, что все сборки в данной программе *должны быть* ориентированы на совместимые сборки BCL. Если это не так, можно иметь две сборки, связанные с разными версиями **System. dll** , которые будут рассогласованы с сигнатурой данного типа. Общая библиотека может быть целевой [.NET Standard 2](https://blog.xamarin.com/share-code-net-standard-2-0/), которая является общим подмножеством целевых платформ или конкретной целевой платформой.

Существует три параметра целевой платформы для Xamarin. Mac, с разными преимуществами и компромиссами.

- **Современный** («мобильный» в более старой документации) — очень похожее подмножество возможностей Xamarin. iOS с высокой настройкой производительности и размера. Эта Целевая платформа является защищенной компоновщиком, поэтому эти проекты могут значительно уменьшиться, удалив неиспользуемый код.

- **Full** (именуемый XM 4,5 в более старой документации) — очень похожее подмножество для классической среды BCL с небольшим удалением. Так как Целевая платформа почти идентична net45 (и более поздних версий), она может легко использовать множество NuGet, не предоставляющих ни netstandard2, ни конкретных сборок Xamarin. Mac. Однако из-за использования System. Configuration она несовместима с компоновкой.

- Не **поддерживается** (называется системой в более старой документации) — вместо СВЯЗЫВАНИЯ с BCL, предоставляемой Xamarin. Mac, используйте текущую установленную систему Mono. Это обеспечивает полный набор сборок, в том числе некоторые из них, которые являются проблематичными (например, System. Drawing). Этот параметр существует только в крайнем случае, и его настоятельно рекомендуется использовать для исчерпания других параметров перед его использованием. Как предполагает название, использование не поддерживается официальными каналами поддержки.

## <a name="setting-the-target-framework"></a>Настройка целевой платформы

Чтобы изменить тип целевой платформы для проекта Xamarin. Mac, выполните следующие действия.

1. Откройте проект Xamarin.iOS в Visual Studio для Mac.
2. В **обозревателе решений** дважды щелкните файл проекта, чтобы открыть диалоговое окно **Параметры проекта**.
3. На вкладке **Общие** выберите тип **целевой платформы** , соответствующий потребностям приложения.

    [![Использование окна "Параметры проекта" для выбора целевой платформы](target-framework-images/select-target-full.png "Использование окна "Параметры проекта" для выбора целевой платформы")](target-framework-images/select-target-full-large.png#lightbox)

4. Чтобы сохранить внесенные изменения, нажмите кнопку **OK**.

После переключения типа целевой платформы необходимо выполнить **очистку** и **перестроить** проект Xamarin. Mac.

## <a name="summary"></a>Сводка

В этой статье кратко описаны различные типы целевых платформ (библиотеки базовых классов), доступные для приложения Xamarin. Mac, и когда следует использовать каждый тип платформы.

## <a name="related-links"></a>Связанные ссылки

- [Совместное использование кода iOS и Mac](~/cross-platform/macios/index.md)
- [Unified API](~/cross-platform/macios/unified/index.md)
- [Переносимые библиотеки классов](~/cross-platform/app-fundamentals/pcl.md)
- [Сборки](~/cross-platform/internals/available-assemblies.md)
- [Обновление существующих приложений Mac](~/cross-platform/macios/unified/updating-mac-apps.md)
