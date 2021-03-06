---
title: Работа с пользовательскими по умолчанию в Xamarin. iOS
description: В этой статье рассматривается работа с Нсусердефаултс для сохранения параметров по умолчанию в приложении Xamarin iOS или расширении. Он описывает Нсусердефаултс на высоком уровне и описывает, как считывать и записывать значения.
ms.prod: xamarin
ms.assetid: DAE7FFC4-B8C9-4D9E-886A-9B2388452EEB
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/07/2016
ms.openlocfilehash: 4706b483f8f3a104f54ea2bf451cb4e2fb209486
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73009082"
---
# <a name="working-with-user-defaults-in-xamarinios"></a>Работа с пользовательскими по умолчанию в Xamarin. iOS

_В этой статье рассматривается работа с Нсусердефаулт для сохранения параметров по умолчанию в приложении или расширении Xamarin. iOS._

Класс `NSUserDefaults` предоставляет приложениям и расширениям iOS способ программного взаимодействия с системой по умолчанию на уровне системы. Используя систему по умолчанию, пользователь может настроить поведение или стиль приложения в соответствии с их предпочтениями (в зависимости от структуры приложения). Например, чтобы представлять данные в метриках и германской измерениях или выбрать определенную тему пользовательского интерфейса.

При использовании с группами приложений `NSUserDefaults` также предоставляет способ обмена данными между приложениями (или расширениями) в пределах определенной группы.

<a name="About-User-Defaults" />

## <a name="about-user-defaults"></a>О пользовательских значениях по умолчанию

Как указано выше, пользовательское по умолчанию (`NSUserDefaults`) можно добавить в приложение (или расширение) и использовать для предоставления настраиваемых параметров, которые конечный пользователь может изменить для настройки внешнего вида или работы приложения во время выполнения.

При первом выполнении приложения `NSUserDefaults` считывает ключи и значения из базы данных пользователя по умолчанию и кэширует их в память, чтобы не открывать и считывать базу данных каждый раз, когда требуется значение. 

> [!IMPORTANT]
> Apple больше не рекомендует разработчику вызывать метод `Synchronize` для синхронизации кэша в памяти с базой данных напрямую. Вместо этого он будет автоматически вызываться через периодические интервалы, чтобы обеспечить синхронизацию кэша в памяти с базой данных пользователя по умолчанию.

Класс `NSUserDefaults` содержит несколько удобных методов для чтения и записи значений предпочтений для общих типов данных, таких как String, Integer, float, Boolean и URL-адреса. Другие типы данных можно архивировать с помощью `NSData`, а затем считывать из базы данных пользователя по умолчанию или записывать в нее. Дополнительные сведения см. в статье [по программированию и настройке](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i)Apple.

<a name="Accessing-the-Shared-NSUserDefaults-Instance" />

## <a name="accessing-the-shared-nsuserdefaults-instance"></a>Доступ к общему экземпляру Нсусердефаултс 

Экземпляр общего пользователя по умолчанию предоставляет доступ к параметрам пользователя по умолчанию для текущего пользователя устройства. Если общий объект по умолчанию не существует, он создается при первом обращении к нему и инициализации со следующими сведениями:

- `NSArgumentDomain`, состоящий из значений по умолчанию, проанализированных из текущего приложения.
- Домен идентификатора пакета приложения.
- `NSGlobalDomain`, состоящий из параметров по умолчанию, общих для всех приложений.
- Отдельный домен для каждого из предпочитаемых языков пользователя.
- `NSRegistrationDomain` с набором временных значений по умолчанию, которые могут быть изменены приложением для обеспечения успешного поиска.

Чтобы получить доступ к экземпляру общих пользовательских значений по умолчанию, используйте следующий код:

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
```

<a name="Accessing-an-App-Group-NSUserDefaults-Instance" />

## <a name="accessing-an-app-group-nsuserdefaults-instance"></a>Доступ к экземпляру Нсусердефаултс группы приложений

Как упоминалось выше, с помощью групп приложений `NSUserDefaults` можно использовать для обмена данными между приложениями (или расширениями) в пределах данной группы. Сначала необходимо убедиться, что группа приложений и необходимые идентификаторы приложений были правильно настроены в разделе **сертификаты, идентификаторы & профилей** в [центре разработки iOS](https://developer.apple.com/devcenter/ios/) и установлены в среде разработки.

Далее необходимо создать один из допустимых идентификаторов приложений для приложения и/или расширения, а файл `Entitlements.plist` должен быть включен в набор приложений с включенными и указанными группами приложений.

После этого можно получить доступ к общим пользовательским группам приложений по умолчанию, используя следующий код:

```csharp
// Get App Group User Defaults
var plist = new NSUserDefaults ("group.com.xamarin.todaysharing", NSUserDefaultsType.SuiteName);
```

Где `group.com.xamarin.todaysharing` — это группа приложений, созданная в оснастке **сертификаты, идентификаторы & профилей** , к которым требуется получить доступ. Дополнительные сведения см. в документации по [возможностям группы приложений](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) .

<a name="Reading-Default-Values" />

## <a name="reading-default-values"></a>Чтение значений по умолчанию

После доступа к нужной пользовательской базе данных по умолчанию можно считывать значения из значений по умолчанию, используя пары "ключ-значение", и несколько удобных методов в зависимости от типа считываемых данных:

- `ArrayForKey` — возвращает массив `NSObjects` для заданного значения ключа.
- `BoolForKey` — возвращает логическое значение для заданного ключа.
- `DataForKey` — возвращает объект `NSData` для данного ключа.
- `DictionaryForKey` — возвращает `NSDictionary` для заданного ключа.
- `DoubleForKey` — возвращает значение типа Double для заданного ключа.
- `FloatForKey` — возвращает значение типа float для заданного ключа.
- `IntForKey` — возвращает целочисленное значение для заданного ключа.
- `StringArrayForKey` — возвращает массив объектов `String` из заданного значения ключа.
- `StringForKey` — возвращает строковое значение для заданного ключа.
- `URLForKey` — возвращает значение `NSUrl` для данного ключа.

Например, следующий код считывает логическое значение из значений по умолчанию для пользователя:

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
...

// Get value
var useHeader = plist.BoolForKey("UseHeader");

```

<a name="Writing-Default-Values" />

## <a name="writing-default-values"></a>Запись значений по умолчанию

Как и при чтении значений выше, после доступа к нужной пользовательской базе данных по умолчанию можно записать значения по умолчанию, используя пары "ключ-значение", и несколько удобных методов в зависимости от типа записываемых данных:

- `SetBool` — записывает заданное логическое значение в заданный ключ.
- `SetDouble` — записывает заданное значение типа Double в заданный ключ.
- `SetFloat` — записывает заданное значение с плавающей запятой в заданный ключ.
- `SetString` — записывает заданное строковое значение в заданный ключ.
- `SetURL` — записывает заданное значение URL (`NSUrl`) в указанный ключ.

Например, следующий код будет записывать логическое значение в значения по умолчанию для пользователя:

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
...

// Save value
plist.SetBool(useHeader, "UseHeader");
...

```

> [!IMPORTANT]
> При первом выполнении приложения `NSUserDefaults` считывает ключи и значения из базы данных пользователя по умолчанию и кэширует их в память, чтобы не открывать и считывать базу данных каждый раз, когда требуется значение.

<a name="Summary" />

## <a name="summary"></a>Сводка

В этой статье рассматривается класс `NSUserDefaults` и его использование для предоставления набора параметров, которые конечный пользователь может использовать для настройки приложения Xamarin. iOS. Кроме того, он покрывает использование групп приложений для обмена данными между расширением и его родительским приложением или между приложениями в группе.

## <a name="related-links"></a>Связанные ссылки

- [Примеры tvOS](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [Инструкции по программированию для настройки и настройки](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i)
- [нсусердефаултс](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSUserDefaults_Class/#//apple_ref/doc/constant_group/NSUserDefaults_Domains)
