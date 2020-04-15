---
title: Атрибут Debuggable
ms.prod: xamarin
ms.assetid: 1ABF90F1-6A70-45AE-9271-D90DC42807D0
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 12/13/2019
ms.openlocfilehash: 6e54dba0c832596818c5163dc4e14ad1e659e040
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2020
ms.locfileid: "75487975"
---
# <a name="debuggable-attribute"></a>Атрибут Debuggable

Для отладки Android поддерживает протокол JDWP (Java Debug Wire Protocol). Эта технология позволяет некоторым средствам, например ADB, взаимодействовать с виртуальной машиной Java. Протокол JDWP очень важен на этапе разработки, но не забывайте отключить его перед публикацией приложения.

JDWP можно настроить с помощью значения атрибута `android:debuggable` в приложении Android. Выберите _один_ из трех приведенных ниже способов установки этого атрибута в Xamarin.Android.

## <a name="androidmanifestxml"></a>AndroidManifest.XML

Создайте или откройте файл `AndroidManifext.xml` и задайте в нем атрибут `android:debuggable`. Будьте осторожны: не отправляйте сборку выпуска с включенной отладкой.

## <a name="add-an-application-class-attribute"></a>Добавление атрибута класса Application

Если в приложении Xamarin.Android есть класс с атрибутом `[Application]`, обновите атрибут до `[Application(Debuggable = true)]`. Задайте для него значение `false`, чтобы отключить его.

## <a name="add-an-assembly-attribute"></a>Добавление атрибута assembly

Если в приложении Xamarin.Android еще НЕТ атрибута класса `[Application]`, добавьте атрибут уровня сборки `[assembly: Application(Debuggable=true)]` в файл C#. Задайте для него значение `false`, чтобы отключить его.

## <a name="summary"></a>Сводка

Если присутствуют одновременно `AndroidManifest.xml` и `ApplicationAttribute`, содержимое `AndroidManifest.xml` имеет более высокий приоритет, чем `ApplicationAttribute`.

Если вы добавите атрибут класса _и_ атрибут сборки, возникнет ошибка компилятора:

```error
"Error The "GenerateJavaStubs" task failed unexpectedly.
System.InvalidOperationException: Application cannot have both a type with an [Application] attribute and an [assembly:Application] attribute."
```

По умолчанию если не указано ни `AndroidManifest.xml`, ни `ApplicationAttribute`, то значение для атрибута `android:debuggable` зависит от того, создаются ли отладочные символы. Если отладочные символы присутствуют, Xamarin.Android устанавливает для атрибута `android:debuggable` значение `true`.

> [!WARNING]
> Значение атрибута `android:debuggable` НЕ всегда зависит от конфигурации сборки. Может случиться так, что для сборки выпуска атрибут `android:debuggable` имеет значение true. Если вы используете атрибут для установки этого значения, вы можете создать оболочку для атрибута в директиве компилятора:
> 
> ```csharp
> #if DEBUG
> [Application(Debuggable = true)]
> #else
> [Application(Debuggable = false)]
> #endif
> ```

## <a name="related-links"></a>Связанные ссылки

- [Приложения с атрибутом Debuggable на Android Market](https://labs.f-secure.com/archive/debuggable-apps-in-android-market/)
