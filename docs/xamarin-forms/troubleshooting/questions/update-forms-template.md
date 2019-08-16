---
title: Можно ли обновить шаблон по умолчанию Xamarin.Forms до более поздней версии пакета NuGet?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 160FBE13-26EB-4B4F-9248-A5CBE58FDD7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 0c0b5e04bafc748b48ea007162cfd7277cc23752
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2019
ms.locfileid: "69528340"
---
# <a name="can-i-update-the-xamarinforms-default-template-to-a-newer-nuget-package"></a>Можно ли обновить шаблон по умолчанию Xamarin.Forms до более поздней версии пакета NuGet?

В этом руководством в качестве примера используется шаблон библиотеки Xamarin. Forms .NET Standard, но один и тот же общий метод также будет работать для шаблона общего проекта Xamarin. Forms. Это руководство написано с примером обновления с Xamarin. Forms 1.5.1.6471 до 2.1.0.6529, но для этого можно задать другие версии по умолчанию.

1. Скопируйте исходный шаблон `.zip` из:

    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\T\PT\Cross-Platform\Xamarin.Forms.PCL.zip`

2. `.zip` Распакуйте во временное расположение.

3. Измените все вхождения старой версии пакета Xamarin. Forms на новую версию, которую вы хотите использовать.
    * `FormsTemplate\FormsTemplate.vstemplate`
    * `FormsTemplate.Android\FormsTemplate.Android.vstemplate`
    * `FormsTemplate.iOS\FormsTemplate.iOS.vstemplate`

    Например`<package id="Xamarin.Forms" version="1.5.1.6471" />` -> `<package id="Xamarin.Forms" version="2.1.0.6529" />`

4. Измените элемент Name основного [файла многопроектного шаблона](https://msdn.microsoft.com/library/ms185308.aspx) (`Xamarin.Forms.PCL.vstemplate`), чтобы сделать его уникальным. Например:

    > `<Name>Blank App (Xamarin.Forms Portable) - 2.1.0.6529</Name>`

5. Повторно заархивировать всю папку шаблона. Обязательно сопоставьте исходную файловую структуру `.zip` файла. Файл должен находиться в верхней `.zip` части файла, а не в папках. `Xamarin.Forms.PCL.vstemplate`

6. Создайте подкаталог "Мобильные приложения" в папке шаблонов Visual Studio для каждого пользователя:
    > `%USERPROFILE%\Documents\Visual Studio 2013\Templates\ProjectTemplates\Visual C#\Mobile Apps`

7. Скопируйте новую ZIP-папку шаблона в новый каталог "Мобильные приложения".

8. Скачайте пакет NuGet, соответствующий версии, из шага 3. Например, [http://nuget.org/api/v2/package/Xamarin.Forms/2.1.0.6529](http://nuget.org/api/v2/package/Xamarin.Forms/2.1.0.6529) (см. также [https://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file](https://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file)) и скопируйте его в соответствующую вложенную папку в папке расширения Xamarin Visual Studio:
    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\Packages`
