---
title: Можно ли обновить шаблон по умолчанию Xamarin.Forms до более поздней версии пакета NuGet?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 160FBE13-26EB-4B4F-9248-A5CBE58FDD7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: e439d39dd8591cad14485e64aabab2d6016a8e27
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61345919"
---
# <a name="can-i-update-the-xamarinforms-default-template-to-a-newer-nuget-package"></a>Можно ли обновить шаблон по умолчанию Xamarin.Forms до более поздней версии пакета NuGet?

В этом руководстве в качестве примера используется Xamarin.Forms .NET Standard шаблон библиотеки, но один и тот же общий метод будет работать для шаблона общий проект Xamarin.Forms. Это руководство предназначено в примере обновления из Xamarin.Forms 1.5.1.6471 для 2.1.0.6529, но эти действия также можно задать другие версии по умолчанию, вместо этого.

1.  Скопируйте исходный шаблон `.zip` из:

    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\T\PT\Cross-Platform\Xamarin.Forms.PCL.zip`

2.  Распакуйте `.zip` во временную папку.

3.  Измените все вхождения старую версию пакета форм в новую версию, которую вы хотите использовать.
    *   `FormsTemplate\FormsTemplate.vstemplate`
    *   `FormsTemplate.Android\FormsTemplate.Android.vstemplate`
    *   `FormsTemplate.iOS\FormsTemplate.iOS.vstemplate`

    Пример: `<package id="Xamarin.Forms" version="1.5.1.6471" />` -> `<package id="Xamarin.Forms" version="2.1.0.6529" />`

4.  Изменить элемент «name» основной [файл многопроектного шаблона](https://msdn.microsoft.com/library/ms185308.aspx) (`Xamarin.Forms.PCL.vstemplate`) для обеспечения его уникальности. Пример:
    > <Name>Пустое приложение (Xamarin.Forms Portable) - 2.1.0.6529</Name>

5.  Повторно заархивировать папку весь шаблон. Убедитесь, что соответствует структуре исходного файла `.zip` файл. `Xamarin.Forms.PCL.vstemplate` Файл должен быть в верхней части `.zip` файле не внутри папки.

6.  Создайте подкаталог «Мобильные приложения» в папке шаблонов пользователя Visual Studio:
    > `%USERPROFILE%\Documents\Visual Studio 2013\Templates\ProjectTemplates\Visual C#\Mobile Apps`

7.  Скопируйте новой папки шаблона ZIP-архив вверх в новый каталог «Мобильные приложения».

8.  Скачайте пакет NuGet, который совпадает с версией из шага 3. Например [ http://nuget.org/api/v2/package/Xamarin.Forms/2.1.0.6529 ](http://nuget.org/api/v2/package/Xamarin.Forms/2.1.0.6529) (см. также [ https://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file ](https://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file)) и скопируйте его в соответствующую вложенную папку в папке extensions с Xamarin для Visual Studio:
    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\Packages`
