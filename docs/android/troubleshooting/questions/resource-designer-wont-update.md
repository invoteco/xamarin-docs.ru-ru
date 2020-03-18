---
title: Не обновляется файл Resource.designer.cs Android
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3F7376E3-59CC-4722-AEED-BB50E4D952AA
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/19/2017
ms.openlocfilehash: 4683bbaa5aa48c7b5de5fb9a87a4cd3fbc0aeada
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/10/2020
ms.locfileid: "73019511"
---
# <a name="my-android-resourcedesignercs-file-will-not-update"></a>Не обновляется файл Resource.designer.cs Android

> [!NOTE]
> Эта проблема решена в Xamarin Studio 5.1.4 и более поздних версиях. Тем не менее, если проблема возникает в Visual Studio для Mac, создайте файл [новой ошибки](~/cross-platform/troubleshooting/questions/howto-file-bug.md) с полными сведениями о версиях и полным выводом журнала сборки.

Ошибка в Xamarin.Studio 5.1 ранее повреждала CSPROJ-файлы путем частичного или полного удаления XML-кода в CSPROJ-файле. Это приводило к тому, что важные части системы сборки Android (такие как обновление Android Resource.design.cs) выходили из строя. Начиная со стабильной версии 5.1.4, вышедшей 15 июля, эту ошибку исправлено; однако во многих случаях файл проекта приходится восстанавливать вручную, как описано ниже.

## <a name="two-possible-approaches-to-fixing-up-the-project-file"></a>Два возможных подхода к исправлению файла проекта

**Варианты**

1. Создайте совершенно новый проект приложения Xamarin.Android, установите все свойства проекта в соответствии с вашим старым проектом, и добавьте все ваши ресурсы, исходные файлы и т. д. обратно в проект.

   **ИЛИ**

2. Сделайте резервную копию оригинального CSPROJ-файла вашего проекта, затем откройте его в текстовом редакторе и добавьте обратно недостающие элементы из нового CSPROJ-файла.

### <a name="if-this-does-not-solve-the-problem"></a>Если это не решает проблему

После экспериментов с этими элементами, вы можете заметить, что после повторного добавления элементов и восстановления проекта, файл Resource.designer.cs будет обновляться, но тогда вам все равно может потребоваться закрыть и повторно открыть решение, чтобы завершить код, и распознать новые типы, содержащиеся в Resource.designer.cs. 
