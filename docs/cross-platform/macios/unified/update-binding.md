---
title: Миграция привязки в Unified API
description: В этой статье рассматриваются шаги, необходимые для обновления существующего Xamarin привязки проекта для поддержки Unified API-интерфейсы для приложений Xamarin.IOS и Xamarin.Mac.
ms.prod: xamarin
ms.assetid: 5E2A3251-D17F-4F9C-9EA0-6321FEBE8577
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 2d57f27bf0d3aaa2a7ba14f23481a8f2bb2d87f2
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61261161"
---
# <a name="migrating-a-binding-to-the-unified-api"></a>Миграция привязки в Unified API

_В этой статье рассматриваются шаги, необходимые для обновления существующего Xamarin привязки проекта для поддержки Unified API-интерфейсы для приложений Xamarin.IOS и Xamarin.Mac._

## <a name="overview"></a>Обзор

Начиная с 1 февраля 2015 г. компания Apple требует, что новый передачу iTunes и Mac App Store должны быть 64-разрядные приложения. Таким образом любое новое приложение Xamarin.iOS и Xamarin.Mac, необходимо использовать новый единый API вместо существующего классического MonoTouch и API-интерфейсы MonoMac для поддержки 64-разрядная версия.

Кроме того любой проект привязки Xamarin также должен поддерживать новые Unified API должны быть включены в 64-разрядная версия Xamarin.iOS или проект Xamarin.Mac. В этой статье описываются действия, необходимые для обновления существующего проекта привязки для использования на единый API.

## <a name="requirements"></a>Требования

Чтобы завершить действия, описанные в этой статье требуется следующее:

 -  **Visual Studio для Mac** -последнюю версию Visual Studio для Mac установлен и настроен на компьютере разработчика.
 -  **Apple Mac** — это Apple mac необходим для сборки проектов привязки для iOS и Mac.

Проекты привязки не поддерживаются в Visual studio на компьютере Windows.

## <a name="modify-the-using-statements"></a>Изменить с помощью инструкции

Унифицированными API упрощает проще, чем когда-либо для совместного использования кода между Mac и iOS, а также дает возможность поддержки 32- и 64 разрядных приложений с тем же двоичный. Путем перетаскивания _MonoMac_ и _MonoTouch_ префиксы из пространств имен, общего доступа достигается между проектами приложений Xamarin.Mac и Xamarin.iOS.

В результате нам потребуется изменять какие-либо привязки контракты (и других `.cs` файлы проекта привязки) для удаления _MonoMac_ и _MonoTouch_ префиксы из наших `using` инструкции.

Например, даны следующие операторы using в контракте привязки:

```csharp
using System;
using System.Drawing;
using MonoTouch.Foundation;
using MonoTouch.UIKit;
using MonoTouch.ObjCRuntime;
```

Мы бы отбросить `MonoTouch` префикс, полученный в следующем примере:

```csharp
using System;
using System.Drawing;
using Foundation;
using UIKit;
using ObjCRuntime;
```

Опять же, необходимо сделать это для любого `.cs` файл в нашем проекте привязки. Это изменение на месте следующим шагом является обновление наш проект привязки, чтобы использовать новые собственные типы данных.

Дополнительные сведения о на единый API, см. в разделе [единый API](~/cross-platform/macios/unified/index.md) документации. Дополнительные сведения о поддержке 32- и 64 разрядных приложений, так и сведения о платформах см. в разделе [32 и 64-разрядных платформы](~/cross-platform/macios/32-and-64/index.md) документации.

## <a name="update-to-native-data-types"></a>Обновление для собственных типов данных

Сопоставляет Objective-C `NSInteger` тип данных для `int32_t` на 32-разрядных системах, позволяющая `int64_t` на 64-разрядных системах. Чтобы сопоставить это поведение, на новый единый API заменяет прежнего использования `int` (в .NET определяется как всегда должны быть `System.Int32`) в новый тип данных: `System.nint`.

Вместе с новой `nint` тип данных, накладывает на единый API `nuint` и `nfloat` типы для сопоставления `NSUInteger` и `CGFloat` типов, а также.

Учитывая все вышесказанное, мы должны просмотрите наш API и убедитесь, что любой экземпляр `NSInteger`, `NSUInteger` и `CGFloat` , ранее было сопоставлено с `int`, `uint` и `float` обновляются к новому `nint`, `nuint` и `nfloat` типов.

Например если определение метода Objective-C:

```csharp
-(NSInteger) add:(NSInteger)operandUn and:(NSInteger) operandDeux;
```

Если предыдущий контракт привязки имеет следующее определение:

```csharp
[Export("add:and:")]
int Add(int operandUn, int operandDeux);
```

Мы бы обновили новую привязку, чтобы быть:

```csharp
[Export("add:and:")]
nint Add(nint operandUn, nint operandDeux);
```
Если более новые версии библиотека стороннего производителя чем мы изначально связанных в сопоставлении мы, нам нужно просмотреть `.h` файлы заголовков, библиотеки и узнать, являются ли любые существующие, явные вызовы `int`, `int32_t`, `unsigned int`, `uint32_t` или `float` будут обновлены до быть `NSInteger`, `NSUInteger` или `CGFloat`. Если Да, такие же изменения `nint`, `nuint` и `nfloat` будет необходимо привести к их сопоставления, а также типы.

Дополнительные сведения об этих изменениях типов данных, см. в разделе [собственные типы](~/cross-platform/macios/nativetypes.md) документа.

## <a name="update-the-coregraphics-types"></a>Обновление типов CoreGraphics

Точка, размер и прямоугольник типы данных, которые используются с `CoreGraphics` использовать 32- или 64 бит в зависимости от устройства, они выполняются. Изначально привязан Xamarin iOS и Mac интерфейсов API мы использовали существующих структур данных, которые произошли в соответствии с типами данных в `System.Drawing` (`RectangleF` к примеру).

Требования для поддержки 64 бита и новые типы данных в собственном формате, следующие изменения будет необходимо внести в существующий код при вызове `CoreGraphic` методы:

- **CGRect** -использование `CGRect` вместо `RectangleF` при определении плавающей точки прямоугольные области.
- **CGSize** -использование `CGSize` вместо `SizeF` при определении размеры с плавающей запятой (ширина и высота).
- **CGPoint** -использование `CGPoint` вместо `PointF` при определении с плавающей запятой указывать расположение (координаты X и Y).

Учитывая все вышесказанное, нам потребуется просмотрите наш API и убедитесь, что любой экземпляр `CGRect`, `CGSize` или `CGPoint` , ранее привязанного к `RectangleF`, `SizeF` или `PointF` изменить в собственный тип `CGRect`, `CGSize` или `CGPoint` напрямую.

Например если инициализатор Objective-C:

```csharp
- (id)initWithFrame:(CGRect)frame;
```

Если наш предыдущая привязка включили следующий код:

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (RectangleF frame);

```

Мы обновим этот код для:

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (CGRect frame);

```

Все изменения в коде теперь на месте, нам нужно изменить привязки проекта, либо поместить файл для привязки к унифицированными API.

## <a name="modify-the-binding-project"></a>Изменение проекта привязки

На завершающем этапе обновления наш проект привязки для использования унифицированными API, нужно либо изменить `MakeFile` , мы используем для сборки проекта или типа проекта Xamarin (если выполняется привязка из среды Visual Studio для Mac) и указать _btouch_  для привязки к унифицированными API вместо классической из них.


### <a name="updating-a-makefile"></a>Обновление файла MakeFile

При использовании файла makefile для построения нашего проекта привязки в Xamarin. Библиотеки DLL, нам потребуется включить `--new-style` параметр командной строки и вызова `btouch-native` вместо `btouch`.

Поэтому даны следующие `MakeFile`:

```csharp
BINDDIR=/src/binding
XBUILD=/Applications/Xcode.app/Contents/Developer/usr/bin/xcodebuild
PROJECT_ROOT=XMBindingLibrarySample
PROJECT=$(PROJECT_ROOT)/XMBindingLibrarySample.xcodeproj
TARGET=XMBindingLibrarySample
BTOUCH=/Developer/MonoTouch/usr/bin/btouch


all: XMBindingLibrary.dll

libXMBindingLibrarySample-i386.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphonesimulator -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphonesimulator/lib$(TARGET).a $@

libXMBindingLibrarySample-arm64.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphoneos -arch arm64 -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphoneos/lib$(TARGET).a $@

libXMBindingLibrarySample-armv7.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphoneos -arch armv7 -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphoneos/lib$(TARGET).a $@

libXMBindingLibrarySampleUniversal.a: libXMBindingLibrarySample-armv7.a libXMBindingLibrarySample-i386.a libXMBindingLibrarySample-arm64.a
    lipo -create -output $@ $^

XMBindingLibrary.dll: AssemblyInfo.cs XMBindingLibrarySample.cs extras.cs libXMBindingLibrarySampleUniversal.a
    $(BTOUCH) -unsafe -out:$@ XMBindingLibrarySample.cs -x=AssemblyInfo.cs -x=extras.cs --link-with=libXMBindingLibrarySampleUniversal.a,libXMBindingLibrarySampleUniversal.a

clean:
    -rm -f *.a *.dll
```

Нам нужно переключиться из вызова метода `btouch` для `btouch-native`, поэтому можно было бы настроить наше определение макрос следующим образом:

```csharp
BTOUCH=/Developer/MonoTouch/usr/bin/btouch-native
```

Мы бы обновили вызов `btouch` и добавьте `--new-style` следующим образом:

```csharp
XMBindingLibrary.dll: AssemblyInfo.cs XMBindingLibrarySample.cs extras.cs libXMBindingLibrarySampleUniversal.a
    $(BTOUCH) -unsafe --new-style -out:$@ XMBindingLibrarySample.cs -x=AssemblyInfo.cs -x=extras.cs --link-with=libXMBindingLibrarySampleUniversal.a,libXMBindingLibrarySampleUniversal.a
```

Теперь мы можем выполнить наших `MakeFile` в обычном режиме, для создания новой версии 64-разрядная версия API.

### <a name="updating-a-binding-project-type"></a>Обновление типа проекта привязки

Если мы используем Visual Studio для Mac при привязке шаблона проекта для построения нашего интерфейса API, необходимо обновить до новой версии единый API привязки шаблона проекта. Для этого проще всего начать новый проект привязки Unified API и копирования заново весь существующий код и параметры.

Выполните следующие действия:

1. Запустите Visual Studio для Mac.
2. Выберите **файл** > **новый** > **решения...**
3. В диалоговом окне нового решения выберите **iOS** > **единый API** > **привязки проект iOS**: 

    [![](update-binding-images/image01new.png "В диалоговом окне нового решения выберите iOS / единый API / Project привязки iOS")](update-binding-images/image01new.png#lightbox)
4. В диалоговом окне «Настройка нового проекта» введите **имя** новый проект привязки и нажмите кнопку **ОК** кнопки.
5. Включите 64-разрядной версии библиотеки Objective-C, который вы собираетесь создавать привязки для.
6. Скопируйте исходный код из существующего проекта привязки классический API 32-разрядная версия (такие как `ApiDefinition.cs` и `StructsAndEnums.cs` файлов).
7. Измените выше отмечалось файлы исходного кода.

Со всеми изменениями можно создать новый 64-разрядной версии API-интерфейса, как это делается 32-разрядной версии.

## <a name="summary"></a>Сводка

В этой статье было показано, изменения, которые необходимо внести в существующий проект привязки Xamarin для поддержки новых интерфейсов API, единой и 64-разрядных устройств и шаги, необходимые для создания нового 64-разрядная версия совместимую версию API.



## <a name="related-links"></a>Связанные ссылки

- [MAC и iOS](~/cross-platform/macios/index.md)
- [Unified API](~/cross-platform/macios/nativetypes.md)
- [32 или 64-разрядных платформ](~/cross-platform/macios/32-and-64/index.md)
- [Обновление имеющихся приложений iOS](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Unified API](~/cross-platform/macios/unified/index.md)
- [BindingSample](https://developer.xamarin.com/samples/monotouch/BindingSample/)
