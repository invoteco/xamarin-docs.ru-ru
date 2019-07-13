---
title: Не обновляется файл Resource.designer.cs Android
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3F7376E3-59CC-4722-AEED-BB50E4D952AA
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/19/2017
ms.openlocfilehash: 65f7c6d8a573501ffec4aa1b8eaf28ff1e6479e8
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/13/2019
ms.locfileid: "67864224"
---
# <a name="my-android-resourcedesignercs-file-will-not-update"></a>Не обновляется файл Resource.designer.cs Android

> [!NOTE]
> Эта проблема устранена в Xamarin Studio 5.1.4 и более поздних версий. Тем не менее, если эта проблема возникает в Visual Studio для Mac, отправьте [новую ошибку](~/cross-platform/troubleshooting/questions/howto-file-bug.md) благодаря полное управление версиями сведения и полный создавать выходные данные журнала.

Ошибка в Xamarin.Studio 5.1 ранее повреждены CSPROJ-файлов, частично или полностью удалите XML-код в CSPROJ-файл. Это приведет к важной части Android построить систему (например, обновление Android Resource.designer.cs) переход на другой. По состоянию на 5.1.4 стабильный выпуск 15 июля, эта ошибка будет исправлена; но во многих случаях файл проекта требуется восстановить вручную, как описано ниже.


## <a name="two-possible-approaches-to-fixing-up-the-project-file"></a>Два возможных подхода к исправлению файла проекта

**Либо:**

1. Создайте новый проект приложения Xamarin.Android, задать все свойства проекта в соответствии с старый проект и добавьте все ресурсы, исходные файлы, т. д. обратно в проект.

   **ИЛИ**

2. Создайте резервную копию исходного проекта CSPROJ-файл, откройте его в текстовом редакторе и добавьте обратно в отсутствующие элементы из аккуратно созданные csproj-файла.

### <a name="if-this-does-not-solve-the-problem"></a>Если это не устранит проблему

Поэкспериментировав с этими элементами, вы могли заметить, что после обратно добавления элементов и перестроение проекта, обновит файл Resource.designer.cs, но затем может по-прежнему необходимо закрыть и снова откройте решение для автозавершения кода для распознавания новые типы, содержащиеся в Resource.designer.cs. 
