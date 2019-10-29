---
title: Обновление существующих приложений iOS
description: В этом документе описаны действия, которые необходимо выполнить для обновления приложения Xamarin. iOS с Classic API на Unified API.
ms.prod: xamarin
ms.assetid: 303C36A8-CBF4-48C0-9412-387E95024CAB
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: 9b531bd095781c80c5f3418725d57f8f6bbb06fd
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73015031"
---
# <a name="updating-existing-ios-apps"></a>Обновление существующих приложений iOS

_Выполните следующие действия, чтобы обновить существующее приложение Xamarin. iOS для использования Unified API._

Обновление существующего приложения для использования Unified API требует внесения изменений в сам файл проекта, а также пространств имен и интерфейсов API, используемых в коде приложения.

## <a name="the-road-to-64-bits"></a>Путь к 64 бит

Новые унифицированные API-интерфейсы необходимы для поддержки архитектуры 64-разрядного устройства из мобильного приложения Xamarin. iOS. По состоянию на 1 февраля 2015 Apple требуется, чтобы все новые отправки приложений в магазин приложений iTunes поддерживали 64 разрядов.

Xamarin предоставляет инструментарий для Visual Studio для Mac и Visual Studio, чтобы автоматизировать процесс миграции с Classic API на Unified API или преобразовать файлы проекта вручную. Хотя использование автоматического инструментария очень рекомендовано, в этой статье рассматриваются оба метода.

### <a name="before-you-start"></a>Перед началом работы...

Перед обновлением существующего кода до Unified API настоятельно рекомендуется устранить все *предупреждения компиляции*. Многие *предупреждения* в Classic API станут такими ошибками после миграции в единую систему. Устранение ошибок перед началом работы упрощается, поскольку сообщения компилятора из Classic API часто содержат указания по обновлению.

## <a name="automated-updating"></a>Автоматическое обновление

После исправления предупреждений выберите существующий проект iOS в Visual Studio для Mac или Visual Studio и выберите **перенести в Xamarin. iOS Unified API** из меню **проект** . Пример:

![](updating-ios-apps-images/beta-tool1.png "Choose Migrate to Xamarin.iOS Unified API from the Project menu")

Перед выполнением автоматизированной миграции необходимо согласиться с этим предупреждением (очевидно, что перед начинать в этом Adventure необходимо убедиться в наличии резервных копий и системы управления версиями):

![](updating-ios-apps-images/beta-tool2.png "Agree to this warning before the automated migration will run")

Это средство, по сути, автоматизирует все шаги, описанные в разделе **обновление вручную** , и является предлагаемым методом преобразования существующего проекта Xamarin. iOS в Unified API.

## <a name="steps-to-update-manually"></a>Действия по обновлению вручную

После исправления предупреждений выполните следующие действия, чтобы вручную обновить приложения Xamarin. iOS для использования нового Unified API:

### <a name="1-update-project-type--build-target"></a>1. Обновление типа проекта & целевой объект сборки

Измените версию проекта в файлах **CSPROJ** с `6BC8ED88-2882-458C-8E55-DFD12B67127B` на `FEACFBD2-3405-455C-9665-78FE426C6842`. Измените файл **CSPROJ** в текстовом редакторе, заменив первый элемент в элементе `<ProjectTypeGuids>`, как показано ниже:

![](updating-ios-apps-images/csproj.png "Edit the csproj file in a text editor, replacing the first item in the ProjectTypeGuids element as shown")

Измените элемент **Import** , содержащий `Xamarin.MonoTouch.CSharp.targets`, на `Xamarin.iOS.CSharp.targets`, как показано ниже.

![](updating-ios-apps-images/csproj2.png "Change the Import element that contains Xamarin.MonoTouch.CSharp.targets to Xamarin.iOS.CSharp.targets as shown")

### <a name="2-update-project-references"></a>2. Обновление ссылок проекта

Разверните узел **ссылки** проекта приложения iOS. Вначале будет показана * неработающая **ссылка, как на этом** снимке экрана (так как мы только что изменили тип проекта):

![](updating-ios-apps-images/references.png "It will initially show a broken- monotouch reference similar to this screenshot because the project type changed")

Щелкните правой кнопкой мыши проект приложения iOS, чтобы **изменить ссылки**, а затем щелкните ссылку на **однокасание** и удалите ее с помощью красной кнопки "X".

![](updating-ios-apps-images/references-delete-monotouch-sml.png "Right-click on the iOS application project to Edit References, then click on the monotouch reference and delete it using the red X button")

Теперь прокрутите список ссылок до конца и тикайте сборку **Xamarin. iOS** .

![](updating-ios-apps-images/references-add-xamarinios-sml.png "Now scroll to the end of the references list and tick the Xamarin.iOS assembly")

Нажмите кнопку **ОК** , чтобы сохранить изменения ссылок на проект.

### <a name="3-remove-monotouch-from-namespaces"></a>3. Удаление из пространств имен с одним касанием

Удалите префикс **нечувствительного касания** из пространств имен в операторах `using` или в любом месте полного имени класса ClassName (например, `MonoTouch.UIKit` просто `UIKit`).

### <a name="4-remap-types"></a>4. Сопоставление типов

Были введены [собственные типы](~/cross-platform/macios/nativetypes.md) , которые заменяют некоторые ранее использовавшиеся типы, например экземпляры `System.Drawing.RectangleF` с `CoreGraphics.CGRect` (например,). Полный список типов можно найти на странице " [собственные типы](~/cross-platform/macios/nativetypes.md) ".

### <a name="5-fix-method-overrides"></a>5. Исправление переопределений методов

Для некоторых `UIKit` методов была изменена сигнатура для использования новых [собственных типов](~/cross-platform/macios/nativetypes.md) (например, `nint`). Если пользовательские подклассы переопределяют эти методы, сигнатуры больше не будут совпадать и будут приводить к ошибкам. Исправьте эти переопределения метода, изменив подкласс в соответствии с новой сигнатурой, используя собственные типы.

Примеры включают изменение `public override int NumberOfSections (UITableView tableView)` возвращают `nint` и изменение типа возвращаемого значения и типов параметров в `public override int RowsInSection (UITableView tableView, int section)` на `nint`.

## <a name="considerations"></a>Особенности

При преобразовании существующего проекта Xamarin. iOS из Classic API в новый Unified API необходимо учитывать следующие моменты, если это приложение полагается на один или несколько компонентов или пакетов NuGet.

### <a name="components"></a>Компоненты

Любой компонент, который вы включили в приложение, также потребуется обновить до Unified API или при попытке компиляции возникнет конфликт. Для любого включаемого компонента замените текущую версию новой версией из хранилища компонентов Xamarin, которое поддерживает Unified API и выполните чистую сборку. Все компоненты, которые еще не были преобразованы автором, будут отображать предупреждение только 32 бит в хранилище компонентов.

### <a name="nuget-support"></a>Поддержка NuGet

Хотя мы внесены изменения в NuGet для работы со службой поддержки Unified API, новый выпуск NuGet не был новым, поэтому мы рассказывающие, как получить NuGet для распознавания новых интерфейсов API.

До этого времени, как и для компонентов, необходимо переключить любой пакет NuGet, который вы включили в проект, в версию, поддерживающую унифицированные интерфейсы API, и затем выполнить чистую сборку.

> [!IMPORTANT]
> Если у вас есть ошибка в форме _"Ошибка 3 не может одновременно включать" и ". dll" и "Xamarin. iOS. dll" в одном проекте Xamarin. iOS, на "Xamarin. iOS. dll" имеется явная ссылка, а "0.0.000" — "XXX, Version =, Culture = нейтральный, PublicKeyToken = null ""_ после преобразования приложения в унифицированные API, обычно это происходит из-за наличия в проекте компонента или пакета NuGet, который не был обновлен до Unified API. Необходимо удалить существующий компонент или NuGet, обновить версию, поддерживающую унифицированные API-интерфейсы, и выполнить чистую сборку.

## <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Включение 64 разрядных сборок приложений Xamarin. iOS

Для мобильного приложения Xamarin. iOS, которое было преобразовано в Unified API, разработчику по-прежнему необходимо включить сборку приложения на 64 разрядных компьютеров из параметров приложения. Подробные инструкции по 64 включению поразрядных сборок см. в статье **включение 64 разрядов для приложений Xamarin. iOS** в документе [рекомендации по платформе 32/64 bit](~/cross-platform/macios/32-and-64/index.md#enable-64) .

## <a name="finishing-up"></a>Завершение

Если вы решили использовать автоматический или ручной метод преобразования приложения Xamarin. iOS из классической среды в унифицированные API, существует несколько экземпляров, требующих дальнейших действий, которые потребуется выполнить вручную. Ознакомьтесь с нашими [советами по обновлению кода в Unified API](~/cross-platform/macios/unified/updating-tips.md) документе для получения известных проблем и устранения неполадок.

## <a name="related-links"></a>Связанные ссылки

- [Советы по обновлению кода в Unified API](~/cross-platform/macios/unified/updating-tips.md)
- [Работа с собственными типами в кроссплатформенных приложениях](~/cross-platform/macios/native-types-cross-platform.md)
- [Различия между классическими и Unified APIми](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
