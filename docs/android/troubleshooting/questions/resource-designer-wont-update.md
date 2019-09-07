---
title: Не обновляется файл Resource.designer.cs Android
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3F7376E3-59CC-4722-AEED-BB50E4D952AA
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/19/2017
ms.openlocfilehash: c175c533e437736849eac6be1f4a90a783123812
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757147"
---
# <a name="my-android-resourcedesignercs-file-will-not-update"></a>Не обновляется файл Resource.designer.cs Android

> [!NOTE]
> Эта проблема решена в Xamarin Studio 5.1.4 и более поздних версиях. Однако если проблема возникает в Visual Studio для Mac, запишите [новую ошибку](~/cross-platform/troubleshooting/questions/howto-file-bug.md) с полными сведениями об управлении версиями и полным выходным файлом журнала сборки.

Ошибка в Xamarin. Studio 5,1 ранее повредила CSPROJ-файлы, частично или полностью удаляя код XML в файле. csproj. Это приведет к сбою важных частей системы сборки Android (например, при обновлении Android Resource.designer.cs). По состоянию на 5.1.4 стабильной версии 15 июля эта ошибка была исправлена. но во многих случаях файл проекта необходимо исправить вручную, как описано ниже.

## <a name="two-possible-approaches-to-fixing-up-the-project-file"></a>Два возможных подхода к исправлению файла проекта

**Иметь**

1. Создайте новый проект приложения Xamarin. Android, установите все свойства проекта в соответствии со старым проектом и добавьте в проект все ресурсы, исходные файлы и т. д.

   **ИЛИ**

2. Создайте резервную копию файла CSPROJ исходного проекта, затем откройте его в текстовом редакторе и добавьте обратно в отсутствующие элементы из файла с открытым расширением. csproj.

### <a name="if-this-does-not-solve-the-problem"></a>Если это не решает проблему

После эксперимента с этими элементами можно заметить, что после добавления элементов и повторного создания проекта файл Resource.designer.cs будет обновлен, но после этого может потребоваться закрыть и снова открыть решение, чтобы получить сведения о завершении кода. новые типы, содержащиеся в Resource.designer.cs. 
