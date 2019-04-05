---
title: Вопросы и ответы по Xamarin.Forms
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 89364175-53BA-4A09-B3E2-44AC67DD971C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 24e2ae456e478585f30aa704917f66bb0bf11da9
ms.sourcegitcommit: 650458de1d362cd7de174cacef7838f0e74426f3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/15/2019
ms.locfileid: "57981644"
---
# <a name="xamarinforms-frequently-asked-questions"></a>Вопросы и ответы по Xamarin.Forms

## <a name="can-i-update-the-xamarinforms-default-template-to-a-newer-nuget-packageupdate-forms-templatemd"></a>[Можно ли обновить шаблон по умолчанию Xamarin.Forms до более поздней версии пакета NuGet?](update-forms-template.md)
В этом руководстве в качестве примера используется Xamarin.Forms .NET Standard шаблон библиотеки, но один и тот же общий метод будет работать для шаблона общий проект Xamarin.Forms.

## <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-xamarinforms-xaml-filesforms-xaml-designermd"></a>[Почему конструктор XAML в Visual Studio работает с XAML-файлами Xamarin.Forms?](forms-xaml-designer.md)
Xamarin.Forms сейчас не поддерживает визуальные конструкторы для файлов XAML.

## <a name="android-build-error-the-linkassemblies-task-failed-unexpectedlyandroid-linkassemblies-errormd"></a>[Ошибка сборки Android: непредвиденный сбой задачи LinkAssemblies](android-linkassemblies-error.md)
Может появиться сообщение об ошибке `The "LinkAssemblies" task failed unexpectedly` при построении проекта Xamarin.Android, в котором используются форм. Это происходит, когда компоновщик активен (обычно при *выпуска* сборки, чтобы уменьшить размер пакета приложения); и она выполняется, так как Android целевые объекты не обновлен до последней версии framework. 

## <a name="why-does-my-xamarinformsmaps-android-project-fail-with-compiletodalvik--unexpected-top-level-errormaps-compiletodalvik-errormd"></a>[«Почему проект Android xamarin.Forms.Maps завершается ошибкой COMPILETODALVIK: НЕПРЕДВИДЕННАЯ ВЕРХНЕГО УРОВНЯ ERROR?»](maps-compiletodalvik-error.md)
Эта ошибка может возникать в панель ошибок из Visual Studio для Mac или в окне выходных данных сборки Visual Studio; в Android проектах, использующих Xamarin.Forms.Maps. Чаще всего, она будет устранена, увеличив размер кучи Java для проекта Xamarin.Android.
