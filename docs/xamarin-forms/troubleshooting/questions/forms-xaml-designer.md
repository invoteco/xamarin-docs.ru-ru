---
title: Почему конструктор XAML в Visual Studio не работает с XAML-файлами Xamarin.Forms?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: cab2eefb-c52f-4d81-866e-8f1feabbdd64
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: ce318faab1af07b95a7769d81a506979b37a34e4
ms.sourcegitcommit: efbc69acf4ea484d8815311b058114379c9db8a2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/08/2019
ms.locfileid: "73842999"
---
# <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-xamarinforms-xaml-files"></a>Почему конструктор XAML в Visual Studio не работает с XAML-файлами Xamarin.Forms?

В настоящее время Xamarin. Forms не поддерживает визуальные конструкторы для файлов XAML. Поэтому при попытке открыть файл XAML форм в *конструкторе пользовательского интерфейса XAML* Visual Studio или *конструкторе пользовательского интерфейса XAML с кодировкой*выдается следующее сообщение об ошибке:

> "Не удается открыть файл в выбранном редакторе. Выберите другой редактор ".

Это ограничение описывается в разделе Руководство по [основам XAML Xamarin. Forms](~/xamarin-forms/xaml/xaml-basics/index.md) :

> «Пока нет визуального конструктора для создания XAML в приложениях Xamarin. Forms, поэтому все XAML должны быть написаны вручную».

Тем не менее, предварительный просмотр XAML Xamarin. Forms можно отобразить, выбрав пункт View > другое меню "Просмотр" **Windows > Xamarin. Forms** .
